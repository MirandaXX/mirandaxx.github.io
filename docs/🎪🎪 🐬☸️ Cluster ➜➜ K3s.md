---
sidebar_position: 2930
title: πͺπͺπ¬βΈοΈ Cluster ββ K3s
---

# K3s




π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅  LAB. K3s β vps  
π΅ why  

    k8s / minikube need 2G+ ram. 
    k3s only need. 521 MB  + 


π΅ K3s cluster.

    k3s: master
    vps: worker



π΅ Hostname & hosts 

  πΆ hostname 

    hostnamectl set-hostname K3s.MGR
    hostnamectl set-hostname K3s.DKT
    hostnamectl set-hostname K3s.VPS


  πΆ hosts file 

    vi /etc/hosts

    172.16.1.33      K3s.MGR
    172.16.1.144     K3s.DKT
    10.214.214.214   K3s.VPS


      if node use diff ip.  
      just make sure node can ping other nodes.
      choose any ip works.


π΅ install docker:  all node 



π΅ master node

  πΆ install last version   β    curl -sfL https://get.k3s.io | INSTALL_K3S_CHANNEL=latest sh -  
  
  πΆ uninstall   - option   β    /usr/local/bin/k3s-uninstall.sh

  πΆ check  Status          β    systemctl status k3s
  
  πΆ check  Node            β    sudo kubectl get nodes -o wide

  πΆ open firewall          β    ufw allow 6443,443 proto tcp

  πΆ get join token         β    sudo cat /var/lib/rancher/k3s/server/node-token



π΅ work node join k3s 

    curl -sfL http://get.k3s.io | K3S_URL=https://<master_IP>:6443 K3S_TOKEN=<join_token> INSTALL_K3S_CHANNEL=latest sh - 

    curl -sfL http://get.k3s.io | K3S_URL=https://10.214.214.33:6443 K3S_TOKEN=K10cdcb44scaeacc87089a29910422e3d873f2eb4a245fdbc9c14::server:9bb20d4d146766c028555f520af8c243 INSTALL_K3S_CHANNEL=latest sh - 
    curl -sfL http://get.k3s.io | K3S_URL=https://172.16.1.33:6443 K3S_TOKEN=K10cdcbs6as10422e3d873f2eb4a245fdbc9c14::server:9bb20d4d146766c028555f520af8c243 INSTALL_K3S_CHANNEL=latest sh - 


  πΆ leave k3s   β     /usr/local/bin/k3s-killall.sh






π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅ 

π΅ lens add k3s 

  /etc/rancher/k3s/k3s.yaml
    change ip inside. 
      server: https://127.0.0.1:6443
      server: https://172.16.1.144:6443



