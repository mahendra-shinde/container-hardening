# Demo 10: Kubernetes Network Policy Example

This demo shows how to use Kubernetes Network Policies to control traffic between two pods (`app1` and `app2`).

## Files
- `net-policy.yml`: Contains the pod definitions and the network policy.

## What This Demo Does
- Deploys two pods:
  - `app1-pod` (using image `mahendrshinde/myweb:1`)
  - `app2-pod` (using image `mahendrshinde/myweb:2`)
- Applies a NetworkPolicy that:
  - Allows ingress traffic to `app2` only from `app1`.
  - Allows egress traffic from `app2` only to `app1`.

## How to Use

1. **Apply the YAML file:**
   ```sh
   kubectl apply -f net-policy.yml
   ```

2. **Verify Pods are Running:**
   ```sh
   kubectl get pods -l app=app1
   kubectl get pods -l app=app2
   ```

3. **Test Network Policy:**
   - **From app1 to app2 (should fail):**
     ```sh
     kubectl exec app1-pod -- wget -qO- http://app2:80
     ```
   - **From app2 to app1 (should succeed):**
     ```sh
     kubectl exec app2-pod -- wget -qO- http://app1:80
     ```
   - **From any other pod (should be denied):**
     Deploy a test pod and try to access `app2-pod`.

4. **Clean Up:**
   ```sh
   kubectl delete -f net-policy.yml
   ```

## Notes
- Make sure your cluster supports Network Policies and has a compatible CNI plugin (e.g., Calico, Cilium).
- The demo assumes both pods expose an HTTP server on port 80. Adjust the test commands if your images use a different port.
