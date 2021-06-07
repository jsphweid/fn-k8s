# Cluster setup

1. install docker
2. I used a pretty basic https://k3d.io/ setup.

NOTE: since the models are private, being logged into dockerhub is necessary

setting up https://docs.nvidia.com/datacenter/cloud-native/kubernetes/dcgme2e.html?


### simple ops

deploy secret - kubectl create secret generic fn-secrets --from-env-file <(jq -r "to_entries|map(\"\(.key)=\(.value|tostring)\")|.[]" secrets.json)
deploy helm - `helm install fn fn`
kill - `helm uninstall fn`
