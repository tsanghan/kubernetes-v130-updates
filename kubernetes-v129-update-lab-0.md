1) Instantiate a `Pod`, named `longjing`, with `requests` properties with `kind-v129` context.

```
k run longjing --image tsanghan/kopi-teh:v1 --port 8000 --dry-run=client -oyaml | \
  k set resources --limits cpu=700m,memory=200Mi --requests cpu=700m,memory=200Mi -f - --local -oyaml | \
  egrep -v "{}|null" | \
  tee longjing.yaml | k --context kind-v129 apply -f -
```

2) View `resizePolicy` property from `longjing` `pod` live resource. Does it exists?
```
k --context kind-v129 get po longjing -oyaml | yq '.spec.containers.0.resizePolicy,"+++",.spec.containers.0.resources'
```

3) Change `cpu resource` from `700m` to `800m` in `longing.yaml` manifest and `apply`. What is the results?
```
sed 's/700/800/g' < longjing.yaml | k --context kind-v129 apply -f -
```

4) Instantiate a `Pod`, named `longjing`, with `requests` properties with `kind-v129-alpha` context.
```
k run longjing --image tsanghan/kopi-teh:v1 --port 8000 --dry-run=client -oyaml | \
  k set resources --limits cpu=700m,memory=200Mi --requests cpu=700m,memory=200Mi -f - --local -oyaml | \
  egrep -v "{}|null" | \
  tee longjing.yaml | k --context kind-v129-alpha apply -f -
```

5) View `resizePolicy` property from `longjing` `pod` live resource. Does it exists?
```
k --context kind-v129-alpha get po longjing -oyaml | yq '.spec.containers.0.resizePolicy,"+++",.spec.containers.0.resources'
```

6) Change `cpu resource` from `700m` to `800m` in `longing.yaml` manifest and `apply`. What is the results? <b>Did the `Pod` restart?</b>
```
sed 's/700/800/g' < longjing.yaml | k --context kind-v129-alpha apply -f -
```

7) View `resources` property from `longjing` live resource. Was `cpu` value changed?
```
k --context kind-v129-alpha get po longjing -oyaml | yq '.spec.containers.0.resizePolicy,"+++",.spec.containers.0.resources'
```

8) Deploy `oolong` with `reources` property in context `kind-v129-alpha`
```
k create deploy oolong --image tsanghan/kopi-teh:v1 --replicas 2 --port 8000 --dry-run=client -oyaml | \
  k set resources --limits cpu=700m,memory=200Mi --requests cpu=700m,memory=200Mi -f - --local -oyaml | \
  egrep -v "{}|null" | \
  tee oolong.yaml | k --context kind-v129-alpha apply -f -
```

9) View `resizePolicy` property from `oolong` `deployment` live resource. Does it exists?
```
k --context kind-v129-alpha get deploy oolong -oyaml | \
  yq '.spec.template.spec.containers.0.resizePolicy,"+++",.spec.template.spec.containers.0.resources'
```

10) View `resizePolicy` property from `oolong` `pods` live resource. Does it exists?
```
for p in $(k --context kind-v129-alpha get po -l app=oolong -oname); do k --context kind-v129-alpha get $p -oyaml | \
     yq '.spec.containers.0.resizePolicy,"+++",.spec.containers.0.resources'; echo "==="; done
```

11) Change `cpu resource` from `700m` to `800m` in `oolong.yaml` manifest and `apply`. What is the results? <b>Did the `oolong` `pods` restart?</b>
```
sed 's/700/800/g' < oolong.yaml | k --context kind-v129-alpha apply -f -
```
12) Delete `oolong` `deployment`.
```k --context kind-v129-alpha delete deploy oolong```

13) Deploy `oolong` with `reources` property in context `kind-v129-alpha` again.
```
k create deploy oolong --image tsanghan/kopi-teh:v1 --replicas 2 --port 8000 --dry-run=client -oyaml | \
  k set resources --limits cpu=700m,memory=200Mi --requests cpu=700m,memory=200Mi -f - --local -oyaml | \
  egrep -v "{}|null" | \
  tee oolong.yaml | k --context kind-v129-alpha apply -f -
```

14) View `resizePolicy` property from `oolong` `deployment` live resource. Does it exists?
```
k --context kind-v129-alpha get deploy oolong -oyaml | \
  yq '.spec.template.spec.containers.0.resizePolicy,"+++",.spec.template.spec.containers.0.resources'
```

15) View `resizePolicy` property from `oolong` `pods` live resource. Does it exists?
```
for p in $(k --context kind-v129-alpha get po -l app=oolong -oname); do k --context kind-v129-alpha get $p -oyaml | \
  yq '.spec.containers.0.resizePolicy,"+++",.spec.containers.0.resources'; echo "==="; done
```

16) Change `cpu resource` from `700m` to `800m` in `oolong` `pods` and `apply`. What is the results? <b>Did the `oolong` `pods` restart?</b>
```
for p in $(k --context kind-v129-alpha get po -l app=oolong -oname); do k --context kind-v129-alpha get $p -oyaml | \
  sed 's/700/800/g' | yq . | k --context kind-v129-alpha apply -f - ; done
```

17) View `resources` property from `oolong` `pods` live resource. Was `cpu` value changed?
```
for p in $(k --context kind-v129-alpha get po -l app=oolong -oname); do k --context kind-v129-alpha get $p -oyaml | \
  yq '.spec.containers.0.resizePolicy,"+++",.spec.containers.0.resources'; echo "==="; done
```