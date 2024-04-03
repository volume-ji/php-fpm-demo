# php-fpm-demo
It is a demo of php-fpm deployed on a tke cluster in nginxingress

## Steps

### build image and push registry

```
cd dockerfile
docker build -t <registry>/<repository>:<tag>  -f Dockerfile ../
docker login <registry> --username <your user> --password <your password>
docker push <registry>/<repository>:<tag>
```

### deploy 
change deployment.yaml "<your image>" to real image

change deployment.yaml "<your image pull secret>" to real secret

configmap.yaml "SCRIPT_FILENAME" must be matched with Dockerfile "WORKDIR", this demo use directory "/project"

note: k8s must install nginxingress, ingress resource need set annotations: kubernetes.io/ingress.class: <your nginxingress classname>
It is necessary to add annotations in ingress resource: 
nginx.ingress.kubernetes.io/backend-protocol: FCGI
nginx.ingress.kubernetes.io/fastcgi-index: index.php
nginx.ingress.kubernetes.io/fastcgi-params-configmap

```
cd k8s
kubectl apply -f ./
```

## dockerfile
* php-runtime-Dockerfile can build php runtime image
* lumen-Dockerfile can build lumen framework depending on runtime image
* Dockerfile is used to build demo project which include installing runtime, lumen and COPY demo project(../public/*) 

## k8s
including deployment, service, configmap, ingress resource deployd on k8s