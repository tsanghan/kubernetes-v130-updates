1. Create Service and Deployment for `lungo` with `kind-v130` context.
```
(k create service nodeport lungo --tcp 80:8000 --node-port 30008 --dry-run=client -oyaml && \
   echo "---" && \
   k create deployment lungo --image tsanghan/kopi-teh:v1 --replicas 2 --port 8000 --dry-run=client -oyaml) | \
   egrep -v "{}|null" | \
   k --context kind-v130 apply -f -
```
2. Create Service and Deployment for `lungo` with `kind-v130-alpha` context.
```
(k create service nodeport lungo --tcp 80:8000 --node-port 30008 --dry-run=client -oyaml && \
   echo "---" && \
   k create deployment lungo --image tsanghan/kopi-teh:v1 --replicas 2 --port 8000 --dry-run=client -oyaml) | \
   egrep -v "{}|null" | \
   k --context kind-v130-alpha apply -f -
```
3. Extract `iptables nat table` rules from a `node` into `iptables-nat.txt` file.
```
k --context kind-v130 -n kube-system exec -it \
  $(k --context kind-v130 -n kube-system get po -l k8s-app=kube-proxy --no-headers -oname | head -1) -- \
  iptables -L -t nat | tee iptables-nat.txt
```
4. Look for `lungo` `iptables Service Chain` in `iptables-nat.txt` file.
```
grep -A4 "Chain.*SVC.*$(grep lungo iptables-nat.txt | grep SVC | awk '{print $1}' | awk -F- '{print $3}')" iptables-nat.txt
```

5. We will now extract `nftables` equivalent rules into `nft-table-kube-proxy.txt` file. What `nftables` `table` will be the equivalent rules resite in?
```
TABLE_NAME=<replace me>
k --context kind-v130-alpha -n kube-system exec -it \
  $(k --context kind-v130-alpha -n kube-system get po -l k8s-app=kube-proxy --no-headers -oname | head -1) -- \
  nft list table ip "$TABLE_NAME" | tee nft-table-kube-proxy.txt
```

6. Look for `lungo` `nftables Service Chain` in `nft-table-kube-proxy.txt` file.
```
$ grep lungo nft-table-kube-proxy.txt | grep -A1 "chain service-"
```