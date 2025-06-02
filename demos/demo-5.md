# Demo: Container Networking Security Best Practices

## Objective

Demonstrate key best practices for securing container networking.

---

## Prerequisites

- Docker or Podman installed
- Basic knowledge of containers and networking

---

## 1. Isolate Containers with User-Defined Networks

```bash
# Create a user-defined bridge network
docker network create secure-net

# Run containers attached to the secure network
docker run -d --name app1 --network secure-net nginx
docker run -d --name app2 --network secure-net httpd
```

**Best Practice:** Use user-defined networks to control communication between containers.

---

## 2. Restrict Inter-Container Communication

```bash
# Create a network with ICC (Inter-Container Communication) disabled
docker network create --internal internal-net

# Containers on this network cannot access external networks
docker run -d --name internal-app --network internal-net nginx
```

**Best Practice:** Use `--internal` networks for sensitive workloads.

---

## 3. Limit Published Ports

```bash
# Only expose necessary ports to the host
docker run -d --name web --network secure-net -p 8080:80 nginx
```

**Best Practice:** Avoid using `--network host` and only publish required ports.

---

## 4. Use Network Policies (Kubernetes Example)

```yaml
# network-policy.yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
    name: deny-all
spec:
    podSelector: {}
    policyTypes:
    - Ingress
    - Egress
```

```bash
kubectl apply -f network-policy.yaml
```

**Best Practice:** Apply network policies to restrict pod communication.

---

## 5. Monitor Network Traffic

- Use tools like `cilium`, `calico`, or `wireshark` to monitor and log network traffic.
- Regularly review logs for suspicious activity.

---

## Cleanup

```bash
docker rm -f app1 app2 internal-app web
docker network rm secure-net internal-net
```

---

## Summary

- Use user-defined and internal networks
- Limit port exposure
- Apply network policies
- Monitor network traffic

---