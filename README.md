# Kubernetes
Este projeto é para fins de estudo.

## Commands

```
kind create cluster --config=<caminho arquivo yaml> --name=<nome cluster>

kubectl get nodes
kubectl get pod
kubectl get replicaset
kubectl get svc
kubectl get hpa
kubectl describe pods <name>
kubectl delete pod <name>
kubectl run mytest --image=<imagem>
kubectl config get-clusters
kubectl apply -f k8s/pod.yaml
kubectl port-forward pod/goserver 8000:80
kubectl port-forward svc/goserver-service 8000:80
kubectl top pod <nome> "para ver o uso de CPU e memoria"

Historia do deployment
kubectl rollout history deployment <name>

Voltar para a versão anterior ou para uma versão especifica:
kubectl rollout undo pod goserver
kubectl rollout undo pod goserver --to-revision=<numero da revisão>

Acessar a API do Kubernetes
kubectl proxy --port=8080

Acessar um Pod
kubectl exec -it <Nome do Pod> -- bash

Para escalar uma aplicação
kubectl scale statefulset mysql --replicas=5
kubectl scale deployment goserver --replicas=2
```


## ReplicaSet

Este recurso tem como responsabilidade monitorar a saúde de um pod para que quando cair por algum motivo
um erro por exemplo, o replicaset sobe uma nova instancia pod.

```
kubectl get replicaset
```
[doc ReplicaSet](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/)

### Problema com ReplicaSet

Quando realizamos a troca da versão da imagem docker e aplicamos a mudança com o comando apply

```
kubectl apply -f <caminho do arquivo yaml>
```

os pods não são atualizados com a nova versão, o grande problema disso tudo é que se um pod cair
o proprio Kubernetes sobe um novo pod com a versão atualizada, porém os pods antigo ficaram com
a versão anterior.

Para resolver isso é preciso derrubar todos os pods após realizar a alteração de uma imagem,
porém realizar isso manualmente não é interessante por isso o Kubernetes tem uma solução, um workload
chamado Deployment, ele atualiza todos os Pods quando e ReplicaSet.

Então quando atualizamos alguma versão da imagem e aplicar esta diferença o Deployment vai atualizar todos os pods e criar um novo replicaSet atualizado.

Com isso conseguimos ter **zero down time**

[doc Deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)


### Teste de stress com Fortio

```bash
kubectl run -it  fortio --rm --image=fortio/fortio -- load -qps 800 -t 120s -c 70 "http://goserver-service/healthz"
```