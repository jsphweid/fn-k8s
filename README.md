# Cluster setup


https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html
https://minikube.sigs.k8s.io/docs/tutorials/nvidia_gpu/ (use 'None' path)
https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/

### simple ops

deploy secret - kubectl create secret generic fn-secrets --from-env-file <(jq -r "to_entries|map(\"\(.key)=\(.value|tostring)\")|.[]" secrets.json)
deploy helm - `helm install fn fn`
kill - `helm uninstall fn`
  