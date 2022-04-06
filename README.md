# assignment
Commodo assignment unfinished


### Setup k8s 
preapare hosts file in ansible and gain acces for ansible(ssh-copy or extra arg username pw etc.)

ansible-playbook users.yml -u {your_username} --extra-vars 'ansible_sudo_pass={your_password}'

#Resources  
* https://buildvirtual.net/deploy-a-kubernetes-cluster-using-ansible/  (includes some misconfiguration of key generation and joining the cluster for nodes) I had to do these parts manually.
