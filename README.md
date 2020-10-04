# kates_docs
```
kubectl cluster-info
kubectl get pods -n kube-system
kubectl create deployment kubernetes-bootcamp --image=gcr.io/google-samples/kubernetes-bootcamp:v1
kubectl get deployments
kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}'
kubectl proxy
kubectl exec -ti kubernetes-bootcamp-57978f5f5d-wg7tf -- bash
kubectl get pods -l app=kubernetes-bootcamp
kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}'
kubectl label pod kubernetes-bootcamp-57978f5f5d-6glv2 kubernetes-bootcamp-57978f5f5d-nxgn7 app=v1 --overwrite
kubectl exec -ti kubernetes-bootcamp-57978f5f5d-6glv2 kubernetes-bootcamp-57978f5f5d-nxgn7 kubernetes-bootcamp-57978f5f5d-wg7tf -- bash
kubectl get deployments
kubectl get servives
kubectl scale deployments/kubernetes-bootcamp --replicas=0
kubectl get replicasets
kubectl autoscale deployment deployments/kubernetes-bootcamp --min=2 --max=10
kubectl describe replicasets

kubectl expose deployment/kubernetes-bootcamp --type="NodePort" --port 8080
kubectl get pods
kubectl set image deployments/kubernetes-bootcamp kubernetes-bootcamp=jocatalin/kubernetes-bootcamp:v2
kubectl describe pods | grep v1
kubectl describe pods | grep v2
kubectl describe services/kubernetes-bootcamp
export NODE_PORT=$(kubectl get services/kubernetes-bootcamp -o go-template='{{(index .spec.ports 0).nodePort}}')
echo NODE_PORT=$NODE_PORT
curl 192.168.0.156:30817
kubectl rollout status deployments/kubernetes-bootcamp
kubectl set image deployments/kubernetes-bootcamp kubernetes-bootcamp=gcr.io/google-samples/kubernetes-bootcamp:v10
kubectl rollout undo deployments/kubernetes-bootcamp
kubectl -n kube-system get deployments
kubectl -n kube-system scale deployments/kubernetes-dashboard --replicas=2
kubectl -n kube-system get pods -o wide

```
