# Namespace Provisioner configuration

Add this section to your `tap-values.yaml` file in order to use this configuration
when creating developer namespaces:

```yaml
namespace_provisioner:
  additional_sources:
  - git:
      url: https://github.com/alexandreroman/tap-demo-platform-gitops.git
      ref: origin/main
      subPath: namespace-provisioner/overlays
    path: _ytt_lib/customize
  - git:
      url: https://github.com/alexandreroman/tap-demo-platform-gitops.git
      ref: origin/main
      subPath: namespace-provisioner/resources
    path: _ytt_lib/namespace-provisioner-resources
```

Use these commands to create a developer namespace:

```shell
kubectl create ns my-dev-ns
kubectl label ns my-dev-ns apps.tanzu.vmware.com/tap-ns=""
```
