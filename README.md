# assignment
Commodo assignment unfinished,
I was proper setup k8s and deployed jenkins and nexus on that cluster.


### Setup k8s 
preapare hosts file in ansible and gain acces for ansible(ssh-copy or extra arg username pw etc.)
```
ansible-playbook users.yml -u {your_username} --extra-vars 'ansible_sudo_pass={your_password}'
ansible-playbook install-k8s.yml -u {your_username} --extra-vars 'ansible_sudo_pass={your_password}'
ansible-playbook master.yml -u {your_username} --extra-vars 'ansible_sudo_pass={your_password}'

* on master   
kubeadm token create  --print-join-command

* on nodes  
sudo kubeadm join 172.31.200.60:6443 --token 00i857.hhwp40v1wayw2pte     --discovery-token-ca-cert-hash sha256:1bf47308c057ead3ad6d555c9792d41b46ade62fe965f79abc62c33eb37e0602{your key hash}

 * verify:
 kubectl get nodes
```
### Setup Jenking & Nexus
#### Jenkings 
```
kubectl create namespace devops-tools





kubectl apply -f serviceAccount.yaml


kubectl get nodes

it creates a PersistentVolume volume in a specific node under /mnt location.
so u must edit in volume.yml
values:
- n-01-k8s-mb-test --> its your node name

kubectl create -f volume.yaml


kubectl apply -f deployment.yaml


* verify:
kubectl get deployments -n devops-tools
kubectl  describe deployments --namespace=devops-tools
kubectl get pods -n devops-tools

kubectl apply -f service.yaml



kubectl get pods --namespace=devops-tools

kubectl logs {your_jenkings_pod} --namespace=jenkins

if you have problem abouth unlock phase u can acces direcrly that pod 
kubectl exec -it ....
```

#### Nexus
```
kubectl get namespace 
--> u must see that namespace if it dos not exist create your namespace 

im using same namespace with jenkings --> devops-tools --> kubectl create namespace devops-tools

kubectl create -f nexus_deployment.yaml


kubectl describe service nexus-service -n devops-tools
kubectl apply -f nexus_service.yaml



* verify:  
kubectl get pod -n devops-tools


* unlock nexus  
kubectl exec nexus-55976bf6fd-xkcft -n devops-tools cat /nexus-data/admin.password
```
##### situation:

```
kube@m-01-k8s-mb-test:/home/mehmet$ kubectl get svc --namespace devops-tools
NAME              TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
jenkins-service   NodePort   10.107.19.200   <none>        8080:32000/TCP   6d18h
nexus-service     NodePort   10.101.131.54   <none>        8081:32001/TCP   6d17h
(reverse-i-search)`': kubectl get nod^C
kube@m-01-k8s-mb-test:/home/mehmet$ kubectl get pod -n devops-tools
NAME                       READY   STATUS    RESTARTS   AGE
jenkins-85fcfbb869-sqf9n   1/1     Running   0          6d18h
nexus-55976bf6fd-xkcft     1/1     Running   0          6d17h
kube@m-01-k8s-mb-test:/home/mehmet$ kubectl get services --namespace devops-tools
NAME              TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
jenkins-service   NodePort   10.107.19.200   <none>        8080:32000/TCP   6d18h
nexus-service     NodePort   10.101.131.54   <none>        8081:32001/TCP   6d17h
kube@m-01-k8s-mb-test:/home/mehmet$
```
![Alt text](/jenkings_dashboard.PNG?raw=true "Jenkings  Dashboard")


![Alt text](/Nexus_dashboard.PNG?raw=true "Nexus  Dashboard")

#Resources  
* https://buildvirtual.net/deploy-a-kubernetes-cluster-using-ansible/  (includes some misconfiguration of key generation and joining the cluster for nodes) I had to do these parts manually.   
* https://devopscube.com/setup-jenkins-on-kubernetes-cluster/   
* https://devopscube.com/setup-nexus-kubernetes/  






