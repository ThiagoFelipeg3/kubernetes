## Commands

kind create cluster --config=<camilho arquivo yaml> --name=<nome cluster>

kubectl get nodes
kubectl get pod
kubectl get replicaset
kubectl describe pods <name>
kubectl delete pod <name>
kubectl run mytest --image=<imagem>
kubectl config get-clusters
kubectl apply -f k8s/pod.yaml
kubectl port-forward pod/goserver 8000:80