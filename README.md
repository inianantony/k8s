# Pods

## Manually create a pod from command line

``` bash
kubectl run my-nginx --image=nginx:alpine
```

This command will create a pod in cluster under default namespace with nginx docker image. we can get details of the pod by

``` bash
kubectl get pods

kubectl describe pod my-nginx
```

However at this point we are not able to reach our pod from external world, so we need to expose it first by

``` bash
kubectl port-forkubeward my-nginx 8080:80
```

We can remove this pod by

``` bash
kubectl delete pod my-nginx
```

## use yml file to describe a pod

the file is nginx.pod.yml

a complete details of the pod can be retreived by 

``` bash
kubectl get pod my-nginx -o yaml
```

To get into the pod in shell

``` bash
kubectl exec my-nginx -it sh
```

## Probes

### Liveness Probe

Used to determine if the pod is healthy and running as expected

Example

``` yaml
spec:
    containers:
        - name: my-nginx
          image: nginx:alpine
          livenessProbe:
            httpGet: # a http get type of check on /index.html and on port 80 and expects success or failure or Unknown as result
                path: /index.html
                port: 80
            initialDelaySeconds: 15 # wait for 15 seconds before starting to check
            timeoutSeconds: 2 # timeout of checking is 2 seconds
            periodSeconds: 5 # check every 5 seconds
            failureThreshold: 1 # allow 1 failure before taking down the pod
```

### Readiness Probe

Used to determine if the pod is ready to receive request

Example

