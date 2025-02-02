# Kubernetes practice using Kind

### Install Kind

```shell
go install sigs.k8s.io/kind@v0.26.0
```

You can install it using other methods described in https://kind.sigs.k8s.io/docs/user/quick-start/. Make sure `kind` is in your `PATH`. If you use `go install` and need to add it into your `PATH`, use the following command

```shell
export PATH=$(go env PATH)/bin:$PATH
```

To work with the cluster, you'll need `kubectl` and `helm`. You can get `kubectl` from https://kubernetes.io/docs/tasks/tools/ and `helm` from https://helm.sh/docs/intro/install/

### Create Cluster

```shell
kind create cluster --config=config.yaml
```

Verify the cluster is installed by using

```shell
kind get clusters
```

Verify the config has been copied/merged by Kind

```shell
kubectl config get-contexts
```

### Set worker roles for worker nodes

```shell
kubectl label node symon-cluster-worker node-role.kubernetes.io/worker=true
kubectl label node symon-cluster-worker2 node-role.kubernetes.io/worker=true
kubectl label node symon-cluster-worker3 node-role.kubernetes.io/worker=true
```

### Install Ingress

```shell
kubectl apply -f deploy-ingress-nginx.yaml
```

Wait until ingress ready

```shell
kubectl wait --namespace ingress-nginx --for=condition=ready pod --selector=app.kubernetes.io/component=controller --timeout=90s
```

### Create secret to pull from private registry

Make sure you logged in to your private registry

```shell
docker login https://<private registry url>
```

To create the registry credential secret (only applicable if your credential is written to your docker config file and it's unencrypted, default behavior in Linux)

```shell
kubectl create secret generic <secret name> --from-file=.dockerconfigjson=<path/to/.docker/config.json> --type=kubernetes.io/dockerconfigjson
```

### Create Config Map from yaml file

```shell
kubectl apply -f <config map file>.yaml
```

### Create Secret from kustomization yaml file

```shell
kubectl apply -k <directory of kustomization>
```

### Deploy debug pod

```shell
helm install debug ./charts/custom-chart
```

### Restart a service (deployment/daemonset)

Double check the resource that you want to restart. For example, if it's a deployment (managed by helm chart)

```shell
kubectl rollout restart deployment/<your deployment name>
```

### Create and attach docker container to Kind's network

```shell
docker run -it --rm --network=kind ubuntu:24.04 /bin/bash
```

