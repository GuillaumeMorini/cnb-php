# Building a PHP container image with Cloud-Native Buildpacks

This project shows how to build a PHP container image with
[Cloud-Native Buildpacks](https://buildpacks.io) (CNB) and
[Paketo Buildpacks](https://paketo.io).

You don't need to write a `Dockerfile` anymore: using CNB you get
secured up-to-date container images out of your source code.

You don't need to care about the runtime environment (is my PHP runtime up-to-date?):
CNB will automatically provision dependencies and configure your container.

## How to use it?

[Download and install the `pack` CLI](https://github.com/buildpacks/pack/releases).
You'll need a Docker daemon running to build container images.

Run this command to build a container image using a Paketo builder (providing PHP support):
```bash
$ pack build myorg/cnb-php --builder gcr.io/paketo-buildpacks/builder:full-cf
...
Successfully built image myorg/cnb-php
```

A lot of dependencies are downloaded in the first run.
These dependencies will be cached so that next runs are faster.

After a couple of minutes, your container image will be published 
to your local Docker daemon:
```bash
$ docker image ls
REPOSITORY                             TAG                  IMAGE ID    
myorg/cnb-php                          latest               b1a0d242e5ec
```

You can run this container right away:
```bash
$ docker run -e PORT=8080 --rm -p 8080:8080/tcp myorg/cnb-php
```

## Deploying to Kubernetes

Use these Kubernetes descriptors to deploy this app to your cluster:
```bash
$ kubectl apply -f k8s
```

This app will be deployed to namespace `cnb-php`:
```bash
kubectl -n cnb-php get pod,deployment,svc
NAME                       READY   STATUS    RESTARTS   AGE
pod/app-7c7957cb94-lpmk8   1/1     Running   0          35m

NAME                        READY   UP-TO-DATE   AVAILABLE   AGE
deployment.extensions/app   1/1     1            1           35m

NAME          TYPE           CLUSTER-IP      EXTERNAL-IP      PORT(S)        AGE
service/app   LoadBalancer   10.100.200.35   35.187.115.254   80:30355/TCP   35m
```

## Contribute

Contributions are always welcome!

Feel free to open issues & send PR.

## License

Copyright &copy; 2020 [VMware, Inc. or its affiliates](https://vmware.com).

This project is licensed under the [Apache Software License version 2.0](https://www.apache.org/licenses/LICENSE-2.0).
