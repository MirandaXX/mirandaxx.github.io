---
sidebar_position: 2930
title: πͺπͺπ¬βΈοΈβΈοΈ0οΈβ£ K8s β Misc.9
---


# K8s Misc





π΅ POD Config

  πΆ spec
    spec-pod        (podname;                      volumes:disk mount to container; )
    spec-containers ( Container name;  image url;  export port; mount out path)



  πΆ metadata

      name        β pod name
      namespace   β set namespace.    βΌοΈ default namespace is: default βΌοΈ
      labels      β set label
      annotaton   β set note 


  πΆ name space

    not use k8s default namespace if possible.
    namespace is how you category all your dockeres.

      NS-Prod
      NS-Test
      NS-AIO


  πΆ Labels

    label-version
    label-prod/test 
    label-web/sql/app
    label-tool





π΅ Ingress 

  πΆ Needs 

    Ingre allow outside user. visit service inside cluster. 
      NodePort need use host node port, the port on host is limited!
      so need something better: Ingress 


  πΆ Desc 

    service use tcp/udp.
    ingress use http/https 

    if need use ingress.  have to install ingress controller first.
    like nginx ingress controller.








π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅  K8s  basic 


	https://www.qikqiak.com/k8s-book/
	https://www.qikqiak.com/k8s-book/
	https://www.qikqiak.com/k8s-book/

	

π΅ deployment / backup 

    you can deploy pod use kubectl create -f pod.yaml
    but you use k8s.  the pod must be important.
    you need at least run two pod at same time.
    in case one pod down. k8s will switch to use another one 
    just like back up. 


        apiVersion: apps/v1
        kind: Deployment
        spec:
        replicas: 2


π΅ RC:  Replication Controller

    auto increase/decrease pod number for you.
    when pod load is too heavy. 
    it auto deploy another for you. 




π΅ static pod 

    by default k8s auto assign a node for pod.
    but sometimes you want some pod run on a fixed node.
    than use this. 




π΅ service 

π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅ ConfigMap 
π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅ ConfigMap 
π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅ ConfigMap 

π΅ why 

    docker          β cmd line.
    docker compose  β docker-compose.yaml 
    k8s             β ConfigMap


π΅ Desc 

    docker config  need prepair before start container.
    k8s  configmap need prepair before start pod


    all config you need mount in docker;   you can add to configmap.

    configmap is a folder. 
        you can let one  pod use one  configmap file.
        you can let many pod use same configmap file. 


    pod will search all configmap file. 
    if found something for himself. pod will auto use it. (no need restart!)


    usually 
        if a pod need very much   config.    create a configmap file for it. 
        if a pod need very little config .   use same configmap file with other pod.



π΅ configmap vs config 

    you can create configmap from config file.
        but configmap is not config file 
        but configmap is not config file 
        but configmap is not config file 

    during import/create it will add some line to head & end

        kubectl describe cm custom-dashy.yaml
        Name:         custom-dashy.yaml
        Namespace:    default
        Labels:       <none>
        Annotations:  <none>



π΅ How use configmap in pod.yaml

	spec.env      β can give a diff name for pair.
	spec.envFrom  β no change name 
	spec.volumes  β 


    configMapKeyRef  β must choose configmap & choose key.
    configMapRef     β only choose configmap . no need choose key 





π΅ configmap β evn demo β

	[root@localhost ~]# vim demo.yaml
	apiVersion: v1  
	kind: ConfigMap               # π₯ 
	metadata:
	name: cm-podxx              # π₯ ConfigMap name.   pod need this name to use this caonfigmap
	data:                         # π₯ all list under data is what we set.
	hostname: "k8s"             # Key: hostname   value: k8s
	password: "123"             # Key: password   value: 123
	---
	apiVersion: v1
	kind: Pod
	metadata:
	name: zhangsan
	spec:
	containers:
	- name: zhangsan
		image: busybox

		env:                         # π₯ use env type
		- name: env-podxx-HostName   # π₯ set a env name. any name 
		valueFrom:
			configMapKeyRef:         # use configmap.
			name: cm-podxx            # π₯ choose ConfigMap name;  must same with the name in configmap.
			key: hostname             # π₯ choose what key to use; must same with the name in configmap.
										# π₯ no need set value -.-.  

		- name: env-podxx-Password    # π₯ set a env name
		valueFrom:
			configMapKeyRef:
			name: cm-podxx
			key: password






π΅ configmap β Mount demo β


πΆ Create configmap (import conf)

    kubectl create configmap xx                --from-file=yyyy
    kubectl create configmap cm-demo           --from-file=/root/redis.conf
    kubectl create configmap custom-dashy.yaml --from-file=/root/dashy.conf
    kubectl create configmap dashy-config --from-file=/root/dashy.conf



πΆ check configmap:         β kubectl get cm
πΆ check  detail            β kubectl describe cm xxxx




	apiVersion: v1
	kind: Pod
	metadata:
	name: nginx
	spec:
	containers:
	- name: nginx
		image: nginx
		ports:
		- containerPort: 80
		volumeMounts:
		- name: html                        # π₯ use volume: must same as volumes.name
		mountPath: /var/www/html          # π₯ set  intermal path
	volumes:
	- name: html                    # π₯ create volume name:   must same as volumeMounts.name
		configMap:                    # 
		name: nginx-html            # π₯ must same as the configmap file name in k8s. 







    πΆ deployment 

      deploy ment can deal with serverless docker very good.
      but not stateful docker.
      stateful docker is most difficult part in k8s.
      there is a lot to concern .







π΅ Storage 

        β some pod can     move beacuse they use nfs/smb 
        β some pod can not move beacuse they use rbd/iscsi 

    rbd driver β  can mount one  pod only  β  β can not move to other node 
    nfs driver β  can mount many pod       β  β can     move to other node 


        no need high disk performace β can use nfs/smb
        do need high disk performace β must rbd/iscis 

