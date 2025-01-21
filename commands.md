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
