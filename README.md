```
$ vagrant up
$ vagrant ssh node-1

[vagrant@node-1 ~]$ sudo dnf install git
[vagrant@node-1 ~]$ git clone https://github.com/adavarski/almalinux-ansible-kubeadm-k8s && cd almalinux-ansible-kubeadm-k8s
[vagrant@node-1 almalinux-ansible-kubeadm-k8s]$ sudo dnf install epel-release
[vagrant@node-1 almalinux-ansible-kubeadm-k8s]$ sudo dnf install ansible
[vagrant@node-1 almalinux-ansible-kubeadm-k8s]$ ssh-keygen
[vagrant@node-1 almalinux-ansible-kubeadm-k8s]$ ssh-copy-id 192.168.56.11
[vagrant@node-1 almalinux-ansible-kubeadm-k8s]$ ssh-copy-id 192.168.56.12
[vagrant@node-1 almalinux-ansible-kubeadm-k8s]$ ssh-copy-id 192.168.56.13
[vagrant@node-1 almalinux-ansible-kubeadm-k8s]$ ansible-playbook -i ./inventory  --become --become-user=root playbook.yaml -e 'ansible_python_interpreter=/usr/bin/python3'

```
