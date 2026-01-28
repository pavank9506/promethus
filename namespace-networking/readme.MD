# Network Policy for pavan Namespace

This file defines a Kubernetes NetworkPolicy for the `pavan` namespace.

## File
- `networkpolicy.yaml`: Namespace-level network policy for the `pavan` namespace.

## What This Policy Does
- **Allows all ingress and egress traffic within the `pavan` namespace.**
- Only pods in the `pavan` namespace can communicate with each other.
- Traffic from/to other namespaces is denied by default (if your cluster enforces network policies).

## Key Sections
- `podSelector: {}`: Applies to all pods in the namespace.
- `ingress`: Allows incoming traffic from any pod in the `pavan` namespace.
- `egress`: Allows outgoing traffic to any pod in the `pavan` namespace.
- `namespaceSelector`: Uses the label `name: pavan` to match the namespace.

## Usage
1. Ensure your namespace is labeled:
   ```sh
   kubectl label namespace pavan name=pavan
   ```
2. Apply the network policy:
   ```sh
   kubectl apply -f networkpolicy.yaml
   ```
3. Verify the policy:
   ```sh
   kubectl get networkpolicy -n pavan
   ```

## Notes
- This policy is effective only if your Kubernetes cluster has a network plugin that supports NetworkPolicy (e.g., Calico, Cilium, Weave).
- You can further restrict or expand the policy by editing the `ingress` and `egress` rules.
- For more restrictive security, consider allowing only specific pod labels or ports.

---
For more details, see the comments in the manifest or Kubernetes documentation on NetworkPolicy.
