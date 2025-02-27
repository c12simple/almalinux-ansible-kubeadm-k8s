### Very Basic k8s cluster setup: Almalinux 9 + Ansible + Kubeadm (Vagrant+VBox env)

#### Install VirtualBox & Vagrant 
```
### Ubuntu Server example:
$ sudo apt install virtualbox virtualbox-ext-pack -y
$ echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
$ wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor | sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg
$ sudo apt update
$ sudo apt install vagrant
$ sudo reboot
```
#### Playground 
```
### Provisioning VMs
$ vagrant up
$ vagrant ssh node-1

### k8s cluster setup
[vagrant@node-1 ~]$ sudo dnf install git
[vagrant@node-1 ~]$ git clone https://github.com/adavarski/almalinux-ansible-kubeadm-k8s && cd almalinux-ansible-kubeadm-k8s
[vagrant@node-1 almalinux-ansible-kubeadm-k8s]$ sudo dnf install epel-release
[vagrant@node-1 almalinux-ansible-kubeadm-k8s]$ sudo dnf install ansible
[vagrant@node-1 almalinux-ansible-kubeadm-k8s]$ ssh-keygen
[vagrant@node-1 almalinux-ansible-kubeadm-k8s]$ ssh-copy-id 192.168.61.10
[vagrant@node-1 almalinux-ansible-kubeadm-k8s]$ ssh-copy-id 192.168.61.11
[vagrant@node-1 almalinux-ansible-kubeadm-k8s]$ ssh-copy-id 192.168.61.12
[vagrant@node-1 almalinux-ansible-kubeadm-k8s]$ ansible-playbook -i ./inventory  --become --become-user=root playbook.yaml -e 'ansible_python_interpreter=/usr/bin/python3'

### Check k8s
[vagrant@node-1 almalinux-ansible-kubeadm-k8s]$ kubectl cluster-info
Kubernetes control plane is running at https://192.168.56.11:6443
CoreDNS is running at https://192.168.56.11:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
[vagrant@node-1 almalinux-ansible-kubeadm-k8s]$ kubectl get node -o wide
NAME     STATUS   ROLES           AGE     VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE                    KERNEL-VERSION                CONTAINER-RUNTIME
node-1   Ready    control-plane   5m15s   v1.26.2   10.0.2.15     <none>        AlmaLinux 9.1 (Lime Lynx)   5.14.0-162.6.1.el9_1.x86_64   containerd://1.6.18
node-2   Ready    <none>          4m39s   v1.26.2   10.0.2.15     <none>        AlmaLinux 9.1 (Lime Lynx)   5.14.0-162.6.1.el9_1.x86_64   containerd://1.6.18
node-3   Ready    <none>          4m37s   v1.26.2   10.0.2.15     <none>        AlmaLinux 9.1 (Lime Lynx)   5.14.0-162.6.1.el9_1.x86_64   containerd://1.6.18
[vagrant@node-1 almalinux-ansible-kubeadm-k8s]$ kubectl get all --all-namespaces
NAMESPACE     NAME                                          READY   STATUS    RESTARTS        AGE
kube-system   pod/calico-kube-controllers-57b57c56f-knt6l   1/1     Running   0               4m59s
kube-system   pod/calico-node-c8gwq                         1/1     Running   0               4m59s
kube-system   pod/calico-node-cdfbv                         1/1     Running   0               4m40s
kube-system   pod/calico-node-qpvw6                         1/1     Running   0               4m40s
kube-system   pod/coredns-787d4945fb-c5lhh                  1/1     Running   0               4m59s
kube-system   pod/coredns-787d4945fb-dxjjr                  1/1     Running   0               4m59s
kube-system   pod/etcd-node-1                               1/1     Running   0               5m11s
kube-system   pod/kube-apiserver-node-1                     1/1     Running   0               5m15s
kube-system   pod/kube-controller-manager-node-1            1/1     Running   1 (3m16s ago)   5m11s
kube-system   pod/kube-proxy-bg5lf                          1/1     Running   0               4m40s
kube-system   pod/kube-proxy-kg88z                          1/1     Running   0               4m59s
kube-system   pod/kube-proxy-njz77                          1/1     Running   0               4m40s
kube-system   pod/kube-scheduler-node-1                     1/1     Running   1 (3m14s ago)   5m15s

NAMESPACE     NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)                  AGE
default       service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP                  5m14s
kube-system   service/kube-dns     ClusterIP   10.96.0.10   <none>        53/UDP,53/TCP,9153/TCP   5m12s

NAMESPACE     NAME                         DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR            AGE
kube-system   daemonset.apps/calico-node   3         3         3       3            3           kubernetes.io/os=linux   5m4s
kube-system   daemonset.apps/kube-proxy    3         3         3       3            3           kubernetes.io/os=linux   5m12s

NAMESPACE     NAME                                      READY   UP-TO-DATE   AVAILABLE   AGE
kube-system   deployment.apps/calico-kube-controllers   1/1     1            1           5m4s
kube-system   deployment.apps/coredns                   2/2     2            2           5m12s

NAMESPACE     NAME                                                DESIRED   CURRENT   READY   AGE
kube-system   replicaset.apps/calico-kube-controllers-57b57c56f   1         1         1       4m59s
kube-system   replicaset.apps/coredns-787d4945fb                  2         2         2       4m59s


```
#### TODO

- K8s HA ( multimaster )
- use ansible roles
- etc.
