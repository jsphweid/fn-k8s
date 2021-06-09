# Cluster setup

- install docker
- install minikube
- minikube addons enable ingress
- deploy helm stuff
- `sudo minikube start --driver=none --apiserver-ips 127.0.0.1 --apiserver-name localhost`

NOTE: since the models are private, being logged into dockerhub is necessary

setting up https://docs.nvidia.com/datacenter/cloud-native/kubernetes/dcgme2e.html?


### simple ops

deploy secret - kubectl create secret generic fn-secrets --from-env-file <(jq -r "to_entries|map(\"\(.key)=\(.value|tostring)\")|.[]" secrets.json)
deploy helm - `helm install fn fn`
kill - `helm uninstall fn`
