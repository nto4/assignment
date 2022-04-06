# assignment
Commodo assignment unfinished


### Setup k8s 
preapare hosts file in ansible and gain acces for ansible(ssh-copy or extra arg username pw etc.)

ansible-playbook users.yml -u {your_username} --extra-vars 'ansible_sudo_pass={your_password}'
ansible-playbook install-k8s.yml -u {your_username} --extra-vars 'ansible_sudo_pass={your_password}'
ansible-playbook master.yml -u {your_username} --extra-vars 'ansible_sudo_pass={your_password}'

* on master   
kubeadm token create  --print-join-command

* on nodes  
sudo kubeadm join 172.31.200.60:6443 --token 00i857.hhwp40v1wayw2pte     --discovery-token-ca-cert-hash sha256:1bf47308c057ead3ad6d555c9792d41b46ade62fe965f79abc62c33eb37e0602{your key hash}

 * verify:
 kubectl get nodes

### Setup Jenking & Nexus
#### Jenkings 

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


#### Nexus

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

#Resources  
* https://buildvirtual.net/deploy-a-kubernetes-cluster-using-ansible/  (includes some misconfiguration of key generation and joining the cluster for nodes) I had to do these parts manually.   
* https://devopscube.com/setup-jenkins-on-kubernetes-cluster/   
* https://devopscube.com/setup-nexus-kubernetes/  



