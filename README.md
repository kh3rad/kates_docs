# kates_docs
##how to install with kuberspray
based on [this kuberspray installation] (https://dzone.com/articles/kubespray-10-simple-steps-for-installing-a-product) we must disable selinux and set firewallrules ans install ansible and ssh-copy-id our servers and install kuberspray requirements with pip,and then we must copy inventory/sample as inventory/mycluster and create hosts.ini file on mycluster directory and declare our servers on that file.(fore detail please see [this kuberspray installation] (https://dzone.com/articles/kubespray-10-simple-steps-for-installing-a-product))
####for deploying cluster we must run below command:
```
ansible-playbook -i inventory/mycluster/hosts.ini cluster.yml
```
after installation we have a cluster with 3 etcd.3 master and 6 worker.

####for adding new node to our cluster we must add node to our hosts.ini and then run below command:
```
ansible-playbook -i inventory/mycluster/hosts.ini scale.yml
```
####for removing a node we must edit host.ini and in all section no need to change but remove all node in kube-node part and then run below command:
```
ansible-playbook -i inventory/mycluster/hosts.ini remove-node.yml
```
####for reset entire cluster and remove kubernetes from our server, run below command:
```
ansible-playbook -i inventory/mycluster/hosts.ini reset.yml
```
Have a good day ;)



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
