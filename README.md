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

to delete the pod we can use

``` bash
kubectl delete pod -f nginx.pod.yml
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
            timeoutSeconds: 2 # timeout of checking is 2 seconds, default 1
            periodSeconds: 5 # check every 5 seconds, default 10
            failureThreshold: 1 # allow 1 failure before taking down the pod, default 3
```

### Readiness Probe

Used to determine if the pod is ready to receive request

Example

``` yaml
spec:
    containers:
        - name: my-nginx
          image: nginx:alpine
          readinessProbe:
            httpGet: # a http get type of check on /index.html and on port 80 and expects success or failure or Unknown as result
                path: /index.html
                port: 80
            initialDelaySeconds: 3 # wait for 3 seconds before starting to check
            periodSeconds: 5 # check every 5 seconds until its up and running
            failureThreshold: 1
```

### Try out the probes

Copy the nginx.pod.yml file and name as nginx.pod.probes.yml and add the readiness and liveness probes

``` bash
kubectl apply -f nginx.pod.probes.yml
```

Now `sh` into the shell of the pod and messup the index.html file

```bash
kubectl exec my-nginx -it sh

cd /usr/share/nginx/html

ls

rm index.html
```

running above cimmands will terminate the pod and this the terminal will exit as well. If you describe the pod and look for message section, you can see the liveness probe fail message.