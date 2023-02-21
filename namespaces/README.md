# Namespace definitions

This directory contains `Namespace` definitions.
A GitOps based configuration is responsible for creating these definitions in a cluster.

## Set up GitOps reconciliation with kapp-controller

Apply this [kapp-controller](https://carvel.dev/kapp-controller/docs/latest/app-spec/)
configuration to monitor the `Namespace` definitions:

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: tap-ns
---
apiVersion: kappctrl.k14s.io/v1alpha1
kind: App
metadata:
  name: tap-ns
  namespace: tap-ns
spec:
  serviceAccountName: tap-ns-sa
  syncPeriod: 1m
  fetch:
  - git:
      url: https://github.com/alexandreroman/tap-demo-platform-gitops.git
      ref: origin/main
  template:
  - ytt:
      paths:
      - namespaces
  deploy:
  - kapp:
      rawOptions:
      - --dangerous-allow-empty-list-of-resources=true
---
apiVersion: v1
kind: ServiceAccount
metadata:
  name: tap-ns-sa
  namespace: tap-ns
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: tap-ns-cluster-role
rules:
- apiGroups: ["*"]
  resources: ["*"]
  verbs: ["*"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: tap-ns-cluster-role-binding
subjects:
- kind: ServiceAccount
  name: tap-ns-sa
  namespace: tap-ns
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: tap-ns-cluster-role
```
