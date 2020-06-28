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