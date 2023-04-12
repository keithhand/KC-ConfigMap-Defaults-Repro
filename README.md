# KC ConfigMap Defaults Repro
This was created as a POC to test a small section for this
[GitHub issue](https://github.com/kubecost/cost-analyzer-helm-chart/issues/1921).
TL;DR: Using "falsy" values in helm templates.

## Why does it work?
It has [something to do](https://github.com/helm/helm/issues/3164#issuecomment-636267669)
with variable typing and forcing the underlying template to use the typing of
the value read in the `values.yaml`. After the type is specified, values such
as `""` and `0.0` are appropriately assigned.

## To test
```sh
❯ helm template test . # tests for nothing being set

❯ helm template test . -f values-test.yaml # tests for setting "empty" and commented values for backwards compatibility
---
# Source: demo/templates/current-configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: current-pricing-configs
data:
  CPU: "90"
---
# Source: demo/templates/working-configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: pricing-configs-working
data:
  CPU: "90"
  GPU: "0"
  sharedNamespaces: ""
```
