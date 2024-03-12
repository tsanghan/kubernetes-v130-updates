1. Look for `admission` related `Resources` in `kind-v129` context.
```
k --context kind-v129 api-resources | grep admission
```
2. Look for `admission` related `Resources` in `kind-v129-alpha` context.
```
k --context kind-v129-alpha api-resources | grep admission
```
3. Pull a `ValidatingAdmissionPolicy` example manifest. View the file.
```
curl -sSL https://raw.githubusercontent.com/kubernetes/website/main/content/en/examples/validatingadmissionpolicy/basic-example-policy.yaml | \
  tee basic-example-policy.yaml; echo
```
4. Pull a `ValidatingAdmissionPolicy` `Binding` manifest. View the file.
```
curl -sSL https://raw.githubusercontent.com/kubernetes/website/main/content/en/examples/validatingadmissionpolicy/basic-example-binding.yaml | \
  tee basic-example-binding.yaml; echo
```
5. Create `test` namespace, and apply `environment=test` label in `kind-v129-alpha` context.
```
k create ns test --dry-run=client -ojson | jq '.metadata.labels = {"environment":"test"}' | k --context kind-v129-alpha apply -f -
```
6. Create `prod` namespace, with no label in `kind-v129-alpha` context.
```
k create ns prod --dry-run=client -ojson | k --context kind-v129-alpha apply -f -
```
7. Apply `basic-example-policy.yaml` manifest in `kind-v129-alpha` context.
```
k --context kind-v129-alpha apply -f basic-example-policy.yaml
```
8. Apply `basic-example-binding.yaml` manifest in `kind-v129-alpha` context.
```
k --context kind-v129-alpha apply -f basic-example-binding.yaml
```
9. Get `validatingadmissionpolicy` live `resource` in `kind-v129-alpha` context.
```
k --context kind-v129-alpha get validatingadmissionpolicy
```
10. Get `validatingadmissionpolicybinding` live `resource` in `kind-v129-alpha` context.
```
k --context kind-v129-alpha get validatingadmissionpolicybinding
```
11. Test in `test` namespace: Create `iron-goddess-of-mercy` with `5` `replicas` in `kind-v129-alpha` context. What is the result?
```
k -n test create deploy iron-goddess-of-mercy --image tsanghan/kopi-teh:v1 --replicas 5 --port 8000 --dry-run=client -oyaml | \
  k --context kind-v129-alpha apply -f -
```
12. Delete `iron-goddess-of-mercy` deployment in `test` namespace in `kind-v129-alpha` context.
```
^apply^delete
```
13. Test in `test` namespace: Create `iron-goddess-of-mercy` with `6` `replicas` in `kind-v129-alpha` context. What is the result?
```
k -n test create deploy iron-goddess-of-mercy --image tsanghan/kopi-teh:v1 --replicas 6 --port 8000 --dry-run=client -oyaml | \
  k --context kind-v129-alpha apply -f -
```
14. Test in `prod` namespace: Create `iron-goddess-of-mercy` with `5` `replicas` in `kind-v129-alpha` context. What is the result?
```
k -n prod create deploy iron-goddess-of-mercy --image tsanghan/kopi-teh:v1 --replicas 5 --port 8000 --dry-run=client -oyaml | \
  k --context kind-v129-alpha apply -f -
```
15. Delete `iron-goddess-of-mercy` deployment in `prod` namespace in `kind-v129-alpha` context.
```
^apply^delete
```
16. Test in `prod` namespace: Create `iron-goddess-of-mercy` with `6` `replicas` in `kind-v129-alpha` context. What is the result?
```
k -n prod create deploy iron-goddess-of-mercy --image tsanghan/kopi-teh:v1 --replicas 6 --port 8000 --dry-run=client -oyaml | \
  k --context kind-v129-alpha apply -f -
```