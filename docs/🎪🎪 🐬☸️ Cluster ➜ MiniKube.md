---
sidebar_position: 2930
title: πͺπͺπ¬βΈοΈ Cluster β MiniKube
---

# Minikube Build





π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅
π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅ minikube  - basic
π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅

π΅ Minikube Desc 

    minikube can make a k8s cluster at local machine very quick.
    it is good for test! 
    when we learn k8s we need a test enviroment which can rebuild easy.

    we have two option
        minikube + docker  β ...      β  easy
        minikube + Podman  β better   β  a little difficlut 


π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅ MacOS:  MiniKube + Docker β
π΅ Mac Install minikube  β
   
    πΆ Install Docker-desktop      

        brew install docker --cask
            (cask install app with GUI )

    πΆ Start Docker

        open app in gui. 

    πΆ Install Minikube 

        brew install minikube

    πΆ Start Minikube 

        minikube start / stop




π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅ MacOS:  MiniKube + Podman β

π΅ podman / docker cli / docker desktop 

    πΆ Docker cli vs Docker Desktop 

        docker cli only work on linux 
            no windows and macos 

        if windows and macos need use docker cli 
            it need install a vm to win/mac first
                then run docker cli in that vm.

        so docker-desktop is a vm under screen.

        so if possible install docker cli   not docker desktop.
            docker cli use much less ram.

            docker-desktop is still free. but have some limit.
                it begian to charge.



    πΆ Podman vs Docker

        new vision k8s stop support docker.
        and use podman instead.
            podman is opensource 
            docker begian charge money.

        in simple  podman is better..



π΅ Mac Install Podman & Minikube 

    brew install podman
    brew install Minikube


π΅ Podman Config 

    πΆ Create VM

        podman machine init --cpus 4 --memory 8192 --disk-size 80
        podman machine init --cpus 2 --memory 2048 --disk-size 20


    πΆ Start VM 

    	podman machine start

        after create a vm for podman.
        now we can use podman like 
            iMAC ~ podman image list --all
            REPOSITORY  TAG         IMAGE ID    CREATED     SIZE


    πΆ Give Podman Root permission 

        by default . podman permission is rootless.
        if we need set rootful permission  need stop podman first.

            iMAC ~ podman machine stop
                    Machine "podman-machine-default" stopped successfully
            iMAC ~ podman machine set --rootful


    πΆ Start Podman again

        podman machine start
                
            The system helper service is not installed; the default Docker API socket
            address can't be used by podman. If you would like to install it run the
            following commands:

                sudo /usr/local/Cellar/podman/4.1.0/bin/podman-mac-helper install
                podman machine stop; podman machine start

            You can still connect Docker API clients by setting DOCKER_HOST using the
            following command in your terminal session:

                export DOCKER_HOST='unix:///Users/techark/.local/share/containers/podman/machine/podman-machine-default/podman.sock'

            Machine "podman-machine-default" started successfully


            βΌοΈ there is warning during start . 
            βΌοΈ Follow your own warning:  run two command:

                sudo /usr/local/Cellar/podman/4.1.0/bin/podman-mac-helper install
                export DOCKER_HOST='unix:///Users/techark/.local/share/containers/podman/machine/podman-machine-default/podman.sock'


    πΆ Restart Podman 

        podman machine stop
        podman machine start
            now no waring.


    πΆ Test Podman

        podman run docker.io/hello-world

        podman run docker.io/hello-world


π΅ Minikube Start

    πΆ Debug Mode 

        minikube start --alsologtostderr -v=7

            if error. try:    minikube delete


    πΆ Start 

        minikube start
        minikube start --driver=podman --container-runtime=cri-o
        minikube start --cpus 4 --memory 7915 --disk-size=80G --driver=podman --network-plugin=cni --cni=auto


            π  Enabled addons: storage-provisioner, default-storageclass
            π‘  kubectl not found. If you need it, try: 'minikube kubectl -- get pods -A'
            π  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default



    πΆ Open dashboard

        minikube dashboard 
    

π΅ Minikube kubectl Config 

    βΌοΈβΌοΈ When use minikube kubectl. all command is little different βΌοΈβΌοΈ
    βΌοΈβΌοΈ When use minikube kubectl. all command is little different βΌοΈβΌοΈ
    βΌοΈβΌοΈ When use minikube kubectl. all command is little different βΌοΈβΌοΈ

        minikube kubectl -- create deployment nginxdepl --image=nginx
            you need add -- after kubectl


        or we setup alias. as follow.
        so we can use kubectl as normal minikube.

        https://minikube.sigs.k8s.io/docs/handbook/kubectl/

    πΆ 1. Make alias 

        alias kubectl="minikube kubectl --"

    πΆ 2. create a symbolic link

        ln -s $(which minikube) /usr/local/bin/kubectl


π΅ Minikube Config

    πΆ Config CPU & Memory

        minikube stop
        minikube config set memory 7000
        minikube config set cpus 4
        minikube start


π΅ Minikube Reset - option

    minikube delete --all
 
π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅ Minikube βΆ Demo π―

    βΌοΈ Offical Manual βΌοΈ
    https://minikube.sigs.k8s.io/docs/start/



π΅ Start Minikube

    πΆ Start Docker & Podman 

        if use docker-desktop. 
            have to run docker in gui first.
        if use podman. 
            need start podman first


    πΆ Start Minikube

        Minikube start


    πΆ Check Minikube Status

        iMAC ~ minikube status
        minikube
        type: Control Plane
        host: Running
        kubelet: Running
        apiserver: Running
        kubeconfig: Configured
        docker-env: in-use


π΅ 1. Create Deployment & export Port 
 
    πΆ Deploy Docker 

        kubectl create deployment dashy --image=lissy93/dashy
    
            β Delete Depoly 
                kubectl delete deploy <deployment name>
                kubectl delete deploy nginx

    πΆ Export  Server Port

        kubectl expose deployment dashy --port=80 --type=NodePort
                this is the port server use / not port you visit 


    πΆ Check 

        β Check Deploy     kubectl get deployments
        β Check Pods       kubectl get pods
        β Check Service    kubectl get services 


π΅ Run Service  (enable Out Cluster Visit ])

    minikube service dashy --url
    http://192.168.64.3:31147

        
        βΌοΈ maybe open a web for you. that web is wrong! close it.  still need one step. βΌοΈ
            keep that terminal/ no close if close web will close too.
            open a new termianl run below commond



π΅ Forword Port To Local

    kubectl port-forward service/NAME  LocalPort:ServerPort

    kubectl port-forward service/dashy 7000:80   β
    kubectl port-forward service/dashy 88:80     β
        βΌοΈ local port can not too small. local port under 100. need root permissionβΌοΈ


    πΆ Visit 

        http://localhost:7000
 
