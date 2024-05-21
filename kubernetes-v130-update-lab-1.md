1. Study the 2 pod declarations below.

DO NOT USE `Unconfied` in your Pod. This is just to demonstrate the change in API, not a demo on AppArmor.

pod-annotation.yaml
===================
```
apiVersion: v1
kind: Pod
metadata:
  name: hello-apparmor
  annotations:
    container.apparmor.security.beta.kubernetes.io/hello: unconfined
spec:
  containers:
  - name: hello
    image: busybox:1.28
    command: [ "sh", "-c", "echo 'Hello AppArmor!' && sleep 1h" ]
```
pod-securitycontext.yaml
========================
```
apiVersion: v1
kind: Pod
metadata:
  name: hello-apparmor
spec:
  containers:
  - name: hello
    image: busybox:1.28
    command: [ "sh", "-c", "echo 'Hello AppArmor!' && sleep 1h" ]
    securityContext:
      appArmorProfile:
        type: Unconfined
```

2. Apply `pod-annotation.yaml` on to kind-v129 cluster.
```
k --context kind-v129 apply -f pod-annotation.yaml
```
Was it sucessful? Did the Pod starts? Was there any warning?

3. Delete the Pod on kind-v129 cluster.
```
k --context kind-v129 delete -f pod-annotation.yaml --now
```
Was the Pod deleted?

4. Apply `pod-securitycontext.yaml` on to kind-v129 cluster.
```
k --context kind-v129 apply -f pod-securitycontext.yaml
```
Was it sucessful? Did the Pod starts? Was there any Error?

5. Apply `pod-annotation.yaml` on to kind-v130 cluster.
```
k --context kind-v130 apply -f pod-annotation.yaml
```
Was it sucessful? Did the Pod starts? Was there any warning?

6. Delete the Pod on kind-v130 cluster.
```
k --context kind-v130 delete -f pod-annotation.yaml --now
```
Was the Pod deleted?

7. Apply `pod-securitycontext.yaml` on to kind-v130 cluster.
```
k --context kind-v130 apply -f pod-securitycontext.yaml
```
Was it sucessful? Did the Pod starts? Was there any Error?

8. Delete the Pod on kind-v130 cluster.
```
k --context kind-v130 delete -f pod-annotation.yaml --now
```
Was the Pod deleted?

