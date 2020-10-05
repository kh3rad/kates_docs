# kates_docs
## how to install with kuberspray
based on [this kuberspray installation] (https://dzone.com/articles/kubespray-10-simple-steps-for-installing-a-product) we must disable selinux and set firewallrules ans install ansible and ssh-copy-id our servers and install kuberspray requirements with pip,and then we must copy inventory/sample as inventory/mycluster and create hosts.ini file on mycluster directory and declare our servers on that file.(fore detail please see [this kuberspray installation] (https://dzone.com/articles/kubespray-10-simple-steps-for-installing-a-product))
#### for deploying cluster we must run below command:
```
ansible-playbook -i inventory/mycluster/hosts.ini cluster.yml
```
after installation we have a cluster with 3 etcd.3 master and 6 worker.

#### for adding new node to our cluster we must add node to our hosts.ini and then run below command:
```
ansible-playbook -i inventory/mycluster/hosts.ini scale.yml
```
#### for removing a node we must edit host.ini and in all section no need to change but remove all node in kube-node part and then run below command:
```
ansible-playbook -i inventory/mycluster/hosts.ini remove-node.yml
```
#### for reset entire cluster and remove kubernetes from our server, run below command:
```
ansible-playbook -i inventory/mycluster/hosts.ini reset.yml
```
Have a good day ;)

## kubernetes: An overview
if you have 10 minute time, please read [this article] (https://medium.com/@fajlinuxblog/kubernetes-an-overview-deafb3b1fbba), in this article, first we get data about kubernetes architecture and components running on each type of node.
### Master Components
* API Server
* Controller Manager
* Kube Scheduler
* ETCD
### Worker Node
* kubelet
The main Kubernetes component on each cluster’s node which speaks to the API server
* kube proxy
like a reverse-proxy service to forwarding requests to an appropriate service or applications inside a Kubernetes private network.
```
kubectl -n kube-system exec -ti kube-proxy-5ctt2 -- iptables --table nat --list
```
### Key concept for using kubernetes
* pod
The pod is a smallest object in the k8s.
* replicasets
A ReplicaSet is an object responsible for ensuring a number of pods running on the node.
* deployment
It is one of the main controllers used, Deployment ensures that a certain number of replicas of a pod through another controller called ReplicaSet is running on the worker nodes in the cluster.
* jobs and cronjobs
As described by official documentation a job creates one or more
pods and ensures that a specified number of them successfully terminate.
* service
A Service routes traffic across a set of Pods. Services are the abstraction that allow pods to die and replicate in Kubernetes without impacting your application.
* Volumes 
Data in containers is ephemeral, i.e. If the container falls and kubelet is recreated, then all data from the old container will be lost . To overcome this problem, Kubernetes uses Volumes.
we need PersistentVolume() and PersistentVolumeClaim(pvc) and Pods use claims as Volumes. we have 3 Access Mode:
** ReadWriteOnce (RWO)
** ReadOnlyMany (RO)
** ReadWriteMany (RWX)
### Clean up resources
To clean up all resources we cause below command:
```
kubectl delete pod <POD NAME>
kubectl delete pvc <PVC NAME>
kubectl delete pv <PV NAME>
kubectl delete service <SERVICE NAME>
kubectl delete deploymeny <DEPLOYMENT NAME>
```
## Kubernetes : Deploy a production ready cluster with Ansible.
this article is aggregation of upper articles and is about [Deplyment of k8s] (https://medium.com/@fajlinuxblog/kubernetes-deploy-a-production-ready-cluster-with-ansible-ceaf7d3a9997), in this article we use haproxy as a master loadbalancer, 
### Launch a POD and a service to conclude the tests.
this is some sample;)
#### POD
```
kubectl apply -f https://k8s.io/examples/application/deployment.yaml
```
#### Service
```
kubectl expose deployment/nginx-deployment --type="NodePort" --port 80
```
## Performing a Rolling Update
this is a [google tutorial] (https://kubernetes.io/docs/tutorials/kubernetes-basics/update/update-intro/) and in this tutorial we have a picture and commands for updating a pod.




```
kubectl cluster-info
kubectl config view
kubectl get pods -n kube-system
kubectl get pods --all-namespaces
kubectl create deployment kubernetes-bootcamp --image=gcr.io/google-samples/kubernetes-bootcamp:v1
kubectl get deployments
kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}'
kubectl proxy
kubectl get cs
kubectl exec -ti kubernetes-bootcamp-57978f5f5d-wg7tf -- bash
kubectl get pods -l app=kubernetes-bootcamp
kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}'
kubectl label pod kubernetes-bootcamp-57978f5f5d-6glv2 kubernetes-bootcamp-57978f5f5d-nxgn7 app=v1 --overwrite
kubectl exec -ti kubernetes-bootcamp-57978f5f5d-6glv2 kubernetes-bootcamp-57978f5f5d-nxgn7 kubernetes-bootcamp-57978f5f5d-wg7tf -- bash
kubectl get deployments
kubectl get servives
kubectl get svc
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
