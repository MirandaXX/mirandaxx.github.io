---
sidebar_position: 150
title: πͺ-5οΈβ£πΊπΊ Ubuntu β init
---

# Ubuntu Basic Config


π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅ disable cloud-init 

π΅ why 

    take too long to start sys. 


π΅ how 

    Prevent start
    * Create an empty file to prevent the service from starting  sudo touch /etc/cloud/cloud-init.disabled
    * 

    Uninstall
    * Disable all services (uncheck everything except "None"):  sudo dpkg-reconfigure cloud-init
    * 
    * Uninstall the package and delete the folders  sudo dpkg-reconfigure cloud-init
    *   sudo apt-get purge cloud-init
    *   sudo rm -rf /etc/cloud/ && sudo rm -rf /var/lib/cloud/
    * 
    * Restart the computer  sudo reboot




π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅ NIC  - static ip

π΅ Disable dhcp.

    πΆ why 

        dhcp take too long to start sys.
        set static ip in ubuntu. not in router.


    πΆ Demo - Ubuntu 22 π―

        /etc/netplan/ xxx.yaml 


        network:
            version: 2
            ethernets:
                ens160:
                    dhcp4: no
                    addresses: [192.168.21.82/24]
                    gateway4: 192.168.21.1
                    nameservers:
                        addresses: [8.8.8.8,8.8.4.4]





    πΆ check yaml & apply 

        sudo netplan apply





π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅ hostname & ssh 

πΆ hostname 

    hostnamectl set-hostname K3s


πΆ ssh allow root 

    sudo sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/g' /etc/ssh/sshd_config
    sudo service sshd restart


πΆ set root password 

    sudo passwd root


πΆ client upload ssh-key 

    ssh-copy-id -i ~/.ssh/id_rsa.pub -p 22 root@
    ssh-copy-id -i ~/.ssh/id_ed25519.pub -p 22 root@



π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅ zsh 

πΆ zsh 

    apt-get update -y &&  apt-get install zsh curl git -y &&  sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"


πΆ change zsh id 

    echo "PS1=\"%f%B%F{magenta}K3s%f%b %{\$fg[cyan]%}%c%{\$reset_color%} %{\$reset_color%}\${ret_status}\"" >> ~/.zshrc && source ~/.zshrc

    only need change K3s to your own name 






π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅ basic tool 

mtr 

htop 

ufw





π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅ Docker 

πΆ add docker's official gpg key . 

    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg


πΆ choose stable docker version 

    echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null


πΆ install

    sudo apt update 

    sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin -y


πΆ Check 

    sudo docker version  





π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅  β IDE β Code Server

    tool  code server 

    online ide.   
    a vscode run in remote server. 

    so we can open web ide local. and config...

    like  vscode `s ssh remote function..


    https://github.com/coder/code-server



    π΅ install 

        curl -fsSL https://code-server.dev/install.sh | sh
        systemctl enable --now code-server@$USER


    π΅ Config IP

        /root/.config/code-server/config.yaml
            default is local only if need remote..
            bind-addr: 127.0.0.1:8080  β  bind-addr: 0.0.0.0:8080

    π΅ restart 

        systemctl restart code-server@$USER


    π΅ open firewall port 

        ufw allow xxx
        ufw allow 10.214.214.0/24


    π΅ visit 
        xxxx:
        password is in config. 





π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅  vpn - wireguard 




