# Cluster setup

- install docker and login to dockerhub
- install minikube
- `kubectl create secret generic regcred --from-file=.dockerconfigjson=<path/to/docker/config.json> --type=kubernetes.io/dockerconfigjson`
- `apt-get -y install socat`
- deploy helm stuff

NOTE: since the models are private, being logged into dockerhub is necessary

setting up https://docs.nvidia.com/datacenter/cloud-native/kubernetes/dcgme2e.html?

- https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html
- https://minikube.sigs.k8s.io/docs/tutorials/nvidia_gpu/ (use 'None' path)
- https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/
- install cert manager `kubectl apply -f https://github.com/jetstack/cert-manager/releases/download/v1.3.1/cert-manager.yaml`

### simple ops

- deploy secret - `kubectl create secret generic fn-secrets --from-env-file <(jq -r "to_entries|map(\"\(.key)=\(.value|tostring)\")|.[]" secrets.json)`
- deploy helm - `helm install fn fn`
- kill - `helm uninstall fn`
  

# grand sequence

### start cluster

- `sudo minikube start --driver=none --apiserver-ips 127.0.0.1 --apiserver-name localhost`

### dashboard

- `sudo minikube dashboard` - runs the dashboard
- `sudo kubectl proxy --address='0.0.0.0' --disable-filter=true` - allows dashboard to be accessible by local IP (or any IP? could be bad)

### cert manager

- `sudo kubectl apply -f https://github.com/jetstack/cert-manager/releases/download/v1.3.1/cert-manager.yaml`

### maybe better....? does it create namespace?
```
kubectl create ns ingress-nginx
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
helm install ingress-nginx --namespace ingress-nginx ingress-nginx/ingress-nginx
```

The thing about this is is that I haven't figured out how to append to the existing ConfigMap (where you can change Too Many Requests to 429 for instance). In the nginx controller pod, you can see the arguments (`--configmap=$(POD_NAMESPACE)/ingress-nginx-controller
`), but if you name a configmap that, helm can't deploy because it already exists (although this seems what stackoverflow suggests to do...). For now I'm just going to manually edit it.



### delete cluster

- `sudo helm ls --all --short | xargs -L1 sudo helm delete` - remove all helm
- `sudo minikube delete`