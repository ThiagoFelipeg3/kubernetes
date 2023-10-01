## Commands

kubectl get nodes
kubectl get pod
kubectl describe pods <name>
kubectl delete pod <name>
kubectl run mytest --image=<imagem>
kubectl config get-clusters
kubectl apply -f k8s/pod.yaml
kubectl port-forward pod/goserver 8000:80