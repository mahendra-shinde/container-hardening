# Kubernetes Policy 

Kubernetes policy engines like **Kyverno** and **Open Policy Agent (OPA)** help enforce security best practices and compliance by validating and mutating resource configurations.

### Kyverno

Kyverno is a Kubernetes-native policy engine that allows you to write policies as Kubernetes resources.

### Installing Kyverno in a Kubernetes Cluster

To install Kyverno in your Kubernetes cluster, apply the official installation manifest:

```bash
kubectl apply -f https://github.com/kyverno/kyverno/releases/download/v1.13.0/install.yaml
```

This command deploys Kyverno and its required components into your cluster.  
After installation, verify that the Kyverno pods are running:

```bash
kubectl get pods -n kyverno
```

For more details and advanced installation options, refer to the [Kyverno documentation](https://kyverno.io/docs/installation/).


- **Example Policy:** Enforce that all pods requesting confidential resources must have KBS annotations.

    > [Policy Manifest](./demo-8-files/policy.yaml) 

- **Apply the Policy:**
    ```bash
    kubectl apply -f demo-8-files/policy.yaml
    ```

- **Test Policy** using a sample Pod

    > [Sample Pod](./demo-8-files/pod.yaml)

    ```bash
    kubectl apply -f demo-8-files/pod.yaml
    ```

> Expected : An Error while deploying pod

    ```
    Error from server: error when creating "pod.yaml": admission webhook "validate.kyverno.svc-fail" denied the request:

    resource Pod/dev/pod3 was blocked due to the following policies

    require-kbs-annotation:
    check-kbs-annotation: 'validation error: Pods running confidential workloads must
        have the KBS annotation. rule check-kbs-annotation failed at path /metadata/annotations/kbs.confidentialcontainers.io/enabled/'
    ```