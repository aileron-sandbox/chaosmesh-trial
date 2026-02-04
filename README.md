# chaosmesh-trial

Try chaos-engineering using ChaosMesh

## Try on local

### Setup kind

```sh
kind create cluster --config kind-cluster.yaml
```

### Install ChaosMesh

```sh
helm repo add chaos-mesh https://charts.chaos-mesh.org
kubectl create ns chaos-mesh
helm install chaos-mesh chaos-mesh/chaos-mesh -n=chaos-mesh --set chaosDaemon.runtime=containerd --set chaosDaemon.socketPath=/run/containerd/containerd.sock --set dashboard.service.nodePort=32333
```

If you need dashboard,

```sh
kubectl apply -f rbac.yaml
kubectl create token account-cluster-manager
```

### Run fortio

```sh
kubectl apply -f fortio/fortio_deploy.yaml
```

And, check response.

```sh
curl localhost:30080/debug
```

### Run aileron-gateway

```sh
kubectl apply -f aileron/aileron_deploy.yaml
```

And, check response.

```sh
curl localhost:30081/debug
```

## Run k6

```sh
k6 run --quiet --summary-trend-stats "min,avg,med,max,p(90),p(95),p(96),p(97),p(98),p(99),p(99.9),p(99.99),p(100)" k6/script.js
```
