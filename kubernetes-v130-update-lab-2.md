1. Deploy Metrics Server to kind-v130 cluster
```
kubectx kind-v130
metrics-server.sh
```

2. Run and expose php-apache server using manifest show below
php-apache.yaml
===============
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: php-apache
spec:
  selector:
    matchLabels:
      run: php-apache
  template:
    metadata:
      labels:
        run: php-apache
    spec:
      containers:
      - name: php-apache
        image: registry.k8s.io/hpa-example
        ports:
        - containerPort: 80
        resources:
          limits:
            cpu: 500m
          requests:
            cpu: 200m
---
apiVersion: v1
kind: Service
metadata:
  name: php-apache
  labels:
    run: php-apache
spec:
  ports:
  - port: 80
  selector:
    run: php-apache
```

3. Create the HorizontalPodAutoscaler
```
k autoscale deployment php-apache --cpu-percent=50 --min=1 --max=10
```

4. Check the current status of the newly-made HorizontalPodAutoscaler
```
kubectl get hpa
```
You should see `0% / 50%` in the `TARGET` column. If not wait for a moment and rerun the above command.

5. Increase the load
In one SSH terminal/Terminal panel
```
kubectl run -i --tty load-generator --rm --image=busybox:1.28 --restart=Never -- /bin/sh -c "while sleep 0.01; do wget -q -O- http://php-apache; done"
```
In another SSH terminal/Terminal panel
```
kubectl get hpa php-apache --watch
```
Do you see that your Deployment automatically scale up to handle more load?

In first SSH terminal/Terminal panel, stop the `load-generator` pod.
```
Ctrl-C
```
In second SSH terminal/Terminal panel, watch till deployment autoscale back to 1 pod.

6. Delete php-apache pod and service
```
k delete -f php-apache.yaml
```