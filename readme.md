# Kubernetes setup and installation
- This guide shows how to setup and intall a working kubernetes cluster using a amd64 machine as a master node and multiple Raspberry Pi's (arm) as worker nodes.

## Prerequisite
- All the devices must be assigned a static IP addresses on the network that are being used.

## Install OS
#### Master
- Install Ubuntu 18.04.4 LTS on the master node (amd64).
#### Raspberry Pi
- Install Raspbian Buster Lite February 2020 on the worker nodes/Pi's (arm).

## Enable SSH
#### Master
- Run the following commands.
```sh
$ sudo systemctl enable ssh
$ sudo systemctl start ssh
```
#### Raspberry Pi
- Create an empty file on the microSD card named 'ssh' or run the following commands.
```sh
$ sudo systemctl enable ssh
$ sudo systemctl start ssh
```

## Disable firewall
#### Master
- Run the following command.
```sh
$ sudo ufw disable
```
- Reboot
```sh
$ sudo reboot
```
#### Raspberry Pi
- Create a startup script on the Pi's.
```sh
$ sudo nano /etc/init.d/iptablesScript.sh
```
- Copy the following to the file and save.
```sh
#!/bin/sh
sudo iptables -P FORWARD ACCEPT
```
- Create this file.
```sh
$ sudo nano /lib/systemd/system/startup.service
```
- Copy the following to the file and save.
```sh
[Unit]
Description=My Startup Service
After=multi-user.target

[Service]
Type=idle
ExecStart=/etc/init.d/iptablesScript.sh

[Install]
WantedBy=multi-user.target
```
- Run the following commands.
```sh
$ sudo chmod 644 /lib/systemd/system/startup.service
$ sudo systemctl daemon-reload
$ sudo systemctl enable startup.service
```
- Reboot
```sh
$ sudo reboot
```

## Edit host name
#### Master
- Edit this file and change the hostname to 'master-node' (you may choose a different name) and save the file.
```sh
$ sudo nano /etc/hosts
```
- Run this command to change the hostname in /etc/hostname.
```sh
$ sudo hostnamectl set-hostname master-node
```
#### Raspberry Pi
- Edit this file and change the hostname to 'pi01' (or 'pi02', depending on how many Pi's you are using) and save the file.
```sh
$ sudo nano /etc/hosts
```
- Run this command to change the hostname in /etc/hostname.
```sh
$ sudo hostnamectl set-hostname pi01
```

## Configure cgroup boot options
#### Master & Raspberry Pi
- Create or edit this file.
```sh
$ sudo nano /boot/cmdline.txt
```
- Append this to the end of the file.
```sh
cgroup_enable=cpuset cgroup_memory=1 cgroup_enable=memory
```

## Install updates
#### Master & Raspberry Pi
- Run the following command.
```sh
$ sudo apt update && sudo apt dist-upgrade
```

## Permanently disable swap
#### Master
- Run the following command.
```sh
$ sudo swapoff -a && sudo sed -i.bak '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab
```
- Reboot
```sh
$ sudo reboot
```
#### Raspberry Pi
- Run the following commands.
```sh
$ sudo dphys-swapfile swapoff
$ sudo dphys-swapfile uninstall
$ sudo apt purge dphys-swapfile
```
- Reboot
```sh
$ sudo reboot
```

## Install Docker
#### Master & Raspberry Pi
- Install curl (if not already installed).
```sh
$ sudo apt install curl
```
- Run the following commands.
```sh
$ curl -sSL get.docker.com | sh
$ sudo usermod -aG docker 'username'
```

## Configure Docker daemon options
#### Master & Raspberry Pi
- Create or edit this file.
```sh
$ sudo nano /etc/docker/daemon.json
```
- Copy this to the file.
```sh
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2"
}
```
- Reboot.
```sh
$ sudo reboot
```

## Setup Kubernetes repository
#### Master & Raspberry Pi
- Create or edit this file.
```sh
$ sudo nano /etc/apt/sources.list.d/kubernetes.list
```
- Append this to the file.
```sh
deb http://apt.kubernetes.io/ kubernetes-xenial main
```
- Add GPG key.
```sh
$ curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
```

## Install Kubernetes.
#### Master & Raspberry Pi
- Update, run the following command. Make sure that this completes successfully, if not try to run the command again.
```sh
$ sudo apt update
```
- Install kubernetes.
```sh
$ sudo apt install kubeadm kubectl kubelet
```

## Setup Kubernetes.
#### Master
- Set /proc/sys/net/bridge/bridge-nf-call-iptables to 1. Run the following command.
```sh
$ sysctl net.bridge.bridge-nf-call-iptables=1
```
- Initialize kubeadm.
```sh
$ sudo kubeadm init --pod-network-cidr=10.244.0.0/16 --v=5
```
- Setup directories, run the following commands. If the output text of *kubeadm init* recommends different commands, run those instead and skip this step.
```sh
$ mkdir -p ~.kube
$ sudo cp /etc/kubernetes/admin.conf ~/.kube/config
$ sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

## Setup pod network.
#### Master
- Setup flannel pod network.
```sh
$ kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/2140ac876ef134e0ed5af15c65e414cf26827915/Documentation/kube-flannel.yml
```
- Check if all pods are up and running.
```sh
$ kubectl get pods --all-namespaces -o wide
```

## Join worker nodes to the Kubernetes cluster
#### Raspberry Pi
- Join all Pi's to the cluster using the join command from the text output of *kubeadm init*. It should look something like this.
```sh
$ kubeadm join <master-ip-address>:<port> --token <token> --discovery-token-ca-cert-hash <token>
```
- Check if all nodes status are ready.
```sh
$ kubectl get nodes -o wide
```

[//]: # (These are reference links used in the body of this note and get stripped out when the markdown processor does its job. There is no need to format nicely because it shouldn't be seen. Thanks SO - http://stackoverflow.com/questions/4823468/store-comments-in-markdown-syntax)


   [dill]: <https://github.com/joemccann/dillinger>
   [git-repo-url]: <https://github.com/joemccann/dillinger.git>
   [john gruber]: <http://daringfireball.net>
   [df1]: <http://daringfireball.net/projects/markdown/>
   [markdown-it]: <https://github.com/markdown-it/markdown-it>
   [Ace Editor]: <http://ace.ajax.org>
   [node.js]: <http://nodejs.org>
   [Twitter Bootstrap]: <http://twitter.github.com/bootstrap/>
   [jQuery]: <http://jquery.com>
   [@tjholowaychuk]: <http://twitter.com/tjholowaychuk>
   [express]: <http://expressjs.com>
   [AngularJS]: <http://angularjs.org>
   [Gulp]: <http://gulpjs.com>

   [PlDb]: <https://github.com/joemccann/dillinger/tree/master/plugins/dropbox/README.md>
   [PlGh]: <https://github.com/joemccann/dillinger/tree/master/plugins/github/README.md>
   [PlGd]: <https://github.com/joemccann/dillinger/tree/master/plugins/googledrive/README.md>
   [PlOd]: <https://github.com/joemccann/dillinger/tree/master/plugins/onedrive/README.md>
   [PlMe]: <https://github.com/joemccann/dillinger/tree/master/plugins/medium/README.md>
   [PlGa]: <https://github.com/RahulHP/dillinger/blob/master/plugins/googleanalytics/README.md>
