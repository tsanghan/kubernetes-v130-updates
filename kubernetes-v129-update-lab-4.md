1. `Whoami` in `kind-v128` context.
```
k --context kind-v128 auth whoami
```
2. `Whoami` in `kind-v129` context.
```
k --context kind-v129 auth whoami
```
3. List `clusterrole` with name `cluster-admin` in `kind-v128` context.
```
k --context kind-v128 get clusterrole | grep cluster-admin
```
4. List `clusterrolebinding` that bind the above `clusterrole` to `cluster-amdin` in `kind-v128` context.
```
k --context kind-v128 get clusterrolebinding | grep cluster-admin
```
5. Get live `clusterrole` resource with name `cluster-admin` in `kind-v128` context.
```
k --context kind-v128 get clusterrole cluster-admin -oyaml
```
6. Get live `clusterrolebinding` resource with name `cluster-admin` in `kind-v128` context.
```
k --context kind-v128 get clusterrolebinding cluster-admin -oyaml
```
7. List `clusterrole` with name `cluster-admin` in `kind-v129` context.
```
k --context kind-v129 get clusterrole | grep cluster-admin
```
8. List `clusterrole` with name `cluster-admin` in `kind-v129` context.
```
k --context kind-v129 get clusterrolebinding | grep cluster-admin
```
9. Get live `clusterrole` resource with name `cluster-admin` in `kind-v129` context.
```
k --context kind-v129 get clusterrole cluster-admin -oyaml
```
10. Get live `clusterrolebinding` resource with name `cluster-admin` in `kind-v129` context.
```
k --context kind-v129 get clusterrolebinding cluster-admin -oyaml
```
11. Get live `clusterrolebinding` resource with name `kubeadm:cluster-admin` in `kind-v129` context.
```
k --context kind-v129 get clusterrolebinding kubeadm:cluster-admins -oyaml
```
12. Get and Save a copy of `cluster-admin` `clusterrolebinding` as `v128-cluster-admin-ClusterRoleBinding.yaml` file in `kind-v128` context.
```
k --context kind-v128 get clusterrolebinding cluster-admin -oyaml | tee v128-cluster-admin-ClusterRoleBinding.yaml
```
13. `DELETE` `cluster-admin` `clusterrolebinding` in `kind-v128` context.
```
k --context kind-v128 delete clusterrolebinding cluster-admin
```
14. `whoami` in `kind-v128` context?
```
k --context kind-v128 auth whoami
```
15. Can you still `create` `pod` in `kind-v128` context?
```
k --context kind-v128 auth can-i create po
```
16. Can you still `create` `deployment` in `kind-v128` context?
```
k --context kind-v128 auth can-i create deploy
```
17. Can you still `delete` `pod` in `kind-v128` context?
```
k --context kind-v128 auth can-i delete po
```
18. Can you still `delete` `deployment` in `kind-v128` context?
```
k --context kind-v128 auth can-i delete deploy
```
19. Get and Save a copy of `kubeadm:cluster-admin` `clusterrolebinding` as `v129-kubeadm_cluster-admins-ClusterRoleBinding.yaml` file in `kind-v129` context.
```
k --context kind-v129 get clusterrolebinding kubeadm:cluster-admins -oyaml | tee v129-kubeadm_cluster-admins-ClusterRoleBinding.yaml
```
20. `DELETE` `cluster-admin` `clusterrolebinding` in `kind-v129` context.
```
k --context kind-v129 delete clusterrolebinding cluster-admin
```
21. `DELETE` `cluster-admin` `kubeadm:clusterrolebinding` in `kind-v129` context.
```
k --context kind-v129 delete clusterrolebinding kubeadm:cluster-admins
```
22. `whoami` in `kind-v129` context?
```
k --context kind-v129 auth whoami
```
23. Can you still `create` `pod` in `kind-v129` context?
```
k --context kind-v129 auth can-i create po
```
24. Can you still `create` `deployment` in `kind-v129` context?
```
k --context kind-v129 auth can-i create deploy
```
25. Can you still `delete` `pod` in `kind-v129` context?
```
k --context kind-v129 auth can-i delete po
```
26. Can you still `delete` `deployment` in `kind-v129` context?
```
k --context kind-v129 auth can-i delete deploy
```
27. Ok, so you are now logout of `kind-v129` context. What can you do now? `BREAK GLASS!!`
```
docker cp v129-control-plane:/etc/kubernetes/super-admin.conf super-admin.conf
SERVER=$(yq '.clusters[] | select(.name == "kind-v129") | .cluster.server' ~/.kube/config) yq -i '.clusters[0].cluster.server = env(SERVER)' super-admin.conf
```
28. `whoami` with `super-admin.conf`?
```
k --kubeconfig super-admin.conf auth whoami
```
29. Can you `create` `pod` with `super-admin.conf`?
```
k --kubeconfig super-admin.conf auth can-i create po
```
30. Can you `create` `deployment` with `super-admin.conf`?
```
k --kubeconfig super-admin.conf auth can-i create deploy
```
31. Can you `delete` `pod` with `super-admin.conf`?
```
k --kubeconfig super-admin.conf auth can-i delete po
```
32. Can you `delete` `deployment` with `super-admin.conf`?
```
k --kubeconfig super-admin.conf auth can-i delete deploy
```
33. With `super-admin.cond`, apply `v129-kubeadm_cluster-admins-ClusterRoleBinding.yaml`.
```
k --kubeconfig super-admin.conf apply -f v129-kubeadm_cluster-admins-ClusterRoleBinding.yaml
```
Why `--context kind-v129` is not required for steps 28-33?

35. `whoami` in `kind-v129` context?
```
k --context kind-v129 auth whoami
```
36. Can you `create` `pod` in `kind-v129` context now?
```
k --context kind-v129 auth can-i create po
```
37. Can you `create` `deployment` in `kind-v129` context now?
```
k --context kind-v129 auth can-i create deploy
```
38. Can you `delete` `pod` in `kind-v129` context now?
```
k --context kind-v129 auth can-i delete po
```
39. Can you `delete` `deployment` in `kind-v129` context now?
```
k --context kind-v129 auth can-i delete deploy
```