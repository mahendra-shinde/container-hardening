# Module 8: Orchestration Security Hardening Using KBS

## Overview

This module covers best practices for securing container orchestration platforms (such as Kubernetes) using Key Broker Service (KBS) for confidential computing and secret management.

## 1. Introduction to KBS

**Key Broker Service (KBS)** is a component that enables secure key management and attestation for confidential workloads. It acts as a bridge between confidential containers and trusted key sources, ensuring that secrets and keys are only released to trusted, attested workloads.

## 2. Why Use KBS in Orchestration?

- **Confidentiality:** Protects sensitive data and secrets from unauthorized access, even from cluster administrators.
- **Attestation:** Ensures only trusted workloads can access secrets.
- **Centralized Key Management:** Simplifies secret rotation and auditing.

## 3. Architecture Overview

```
+-------------------+      +-------------------+      +-------------------+
|   Confidential    |<---->|       KBS         |<---->|   Key Management  |
|     Workload      |      |                   |      |     Service       |
+-------------------+      +-------------------+      +-------------------+
        |                        ^                            ^
        v                        |                            |
+-------------------+      +-------------------+      +-------------------+
|   Orchestrator    |      |   Attestation     |      |   Secret Storage  |
+-------------------+      +-------------------+      +-------------------+
```

---

## 4. Setting Up KBS in Kubernetes

### Prerequisites

- A Kubernetes cluster (v1.20+ recommended)
- Confidential Computing-enabled nodes (e.g., Intel SGX, AMD SEV)
- KBS deployment manifests or Helm charts

### Steps

1. **Deploy KBS**

   Deploy KBS as a service in your cluster:

   ```bash
   kubectl apply -f https://raw.githubusercontent.com/confidential-containers/kbs/main/deploy/k8s/kbs.yaml
   ```

2. **Configure Attestation**

   - Ensure your nodes support hardware-based attestation.
   - Configure KBS to use the appropriate attestation service (e.g., Intel SGX DCAP, AMD SEV).

3. **Integrate with Confidential Containers**

   - Annotate your pod specs to request confidential resources.
   - Use KBS as the key provider for your confidential workloads.

   Example pod annotation:

   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: confidential-app
     annotations:
       kbs.confidentialcontainers.io/enabled: "true"
   spec:
     containers:
     - name: app
       image: myrepo/confidential-app:latest
   ```

4. **Secret Injection**

   - Store secrets in your key management service (e.g., HashiCorp Vault, Azure Key Vault).
   - Configure KBS to retrieve and inject secrets only after successful attestation.

---

## 5. Best Practices

- **Restrict KBS Access:** Only allow access from attested workloads.
- **Audit KBS Logs:** Monitor for unauthorized access attempts.
- **Rotate Keys Regularly:** Use KBS to automate key rotation.
- **Network Policies:** Restrict network access to KBS endpoints.

---

## 6. Example: Deploying a Confidential Workload

1. **Store a secret** in your key management service.
2. **Configure KBS** to access the secret.
3. **Deploy a pod** with the required annotations.
4. **Verify** that the workload receives the secret only after attestation.


## 7. Pod Security Admission and Pod Security Standards

Kubernetes provides built-in mechanisms to enforce security controls at the pod level using **Pod Security Admission (PSA)** and **Pod Security Standards (PSS)**.

### Pod Security Admission (PSA)

PSA is a Kubernetes admission controller that evaluates pods against predefined security standards when they are created or updated. It helps prevent insecure workloads from running in your cluster.

- **Enable PSA:**  
    PSA is enabled by default in Kubernetes v1.25+.
- **Namespace Labels:**  
    Apply labels to namespaces to enforce the desired security level:
    - `pod-security.kubernetes.io/enforce: restricted`
    - `pod-security.kubernetes.io/audit: baseline`
    - `pod-security.kubernetes.io/warn: privileged`

    Example:
    ```bash
    kubectl label namespace confidential-workloads pod-security.kubernetes.io/enforce=restricted
    ```

### Pod Security Standards (PSS)

PSS defines three policy levels:
- **Privileged:** No restrictions; allows all pod configurations.
- **Baseline:** Minimally restrictive; prevents known privilege escalations.
- **Restricted:** Heavily restricted; enforces best practices for confidential and secure workloads.

**Recommendation:**  
For confidential workloads using KBS, use the `restricted` policy to maximize security.

## 8. Secure Configurations Using Kyverno & Open Policy Agent (OPA)

Kubernetes policy engines like **Kyverno** and **Open Policy Agent (OPA)** help enforce security best practices and compliance by validating and mutating resource configurations.

### Kyverno

Kyverno is a Kubernetes-native policy engine that allows you to write policies as Kubernetes resources.

- **Example Policy:** Enforce that all pods requesting confidential resources must have KBS annotations.

    ```yaml
    apiVersion: kyverno.io/v1
    kind: ClusterPolicy
    metadata:
        name: require-kbs-annotation
    spec:
        validationFailureAction: enforce
        rules:
            - name: check-kbs-annotation
                match:
                    resources:
                        kinds:
                            - Pod
                validate:
                    message: "Pods running confidential workloads must have the KBS annotation."
                    pattern:
                        metadata:
                            annotations:
                                kbs.confidentialcontainers.io/enabled: "true"
    ```

- **Apply the Policy:**
    ```bash
    kubectl apply -f require-kbs-annotation.yaml
    ```

### Open Policy Agent (OPA) / Gatekeeper

OPA with Gatekeeper enables policy enforcement using the Rego language.

- **Example ConstraintTemplate:** Require KBS annotation on confidential pods.

    ```yaml
    apiVersion: templates.gatekeeper.sh/v1beta1
    kind: ConstraintTemplate
    metadata:
        name: kbsannotationrequired
    spec:
        crd:
            spec:
                names:
                    kind: KBSAnnotationRequired
        targets:
            - target: admission.k8s.gatekeeper.sh
                rego: |
                    package k8srequiredannotations

                    violation[{"msg": msg}] {
                        input.review.kind.kind == "Pod"
                        not input.review.object.metadata.annotations["kbs.confidentialcontainers.io/enabled"]
                        msg := "KBS annotation is required for confidential workloads."
                    }
    ```

- **Apply the ConstraintTemplate and Constraint:**
    ```bash
    kubectl apply -f kbsannotationrequired-template.yaml
    kubectl apply -f kbsannotationrequired-constraint.yaml
    ```

**Tip:** Use Kyverno or OPA to enforce additional security controls, such as restricting hostPath volumes, enforcing image registries, or requiring resource limits.

---
**References:**

- [Pod Security Admission](https://kubernetes.io/docs/concepts/security/pod-security-admission/)
- [Pod Security Standards](https://kubernetes.io/docs/concepts/security/pod-security-standards/)
- [Confidential Containers KBS Documentation](https://github.com/confidential-containers/kbs)
- [Kubernetes Confidential Containers](https://confidentialcontainers.org/)
- [Kubernetes Secrets Management](https://kubernetes.io/docs/concepts/configuration/secret/)

