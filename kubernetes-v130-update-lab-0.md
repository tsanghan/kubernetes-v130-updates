1) You will need 4 Kubernetes clusters for today's session.
```
cd ~/Projects/kind
source bashrc
kind create cluster --name v128 --config kind-v1.28.yaml && \
kind create cluster --name v129 --config kind-v1.29.yaml && \
kind create cluster --name v130 --config kind.yaml &&
kind create cluster --name v130-alpha --config kind-alpha.yaml && \
k foreach -q -- apply -f https://raw.githubusercontent.com/projectcalico/calico/$KX_CALICO_VER/manifests/calico.yaml
```
