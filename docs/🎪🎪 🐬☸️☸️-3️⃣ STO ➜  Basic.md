---
sidebar_position: 2930
title: πͺπͺπ¬βΈοΈβΈοΈ3οΈβ£ STO β Basic
---


# Storage βΆ PV PVC CSI






π΅ K8s Storage

    local    emptyDir   β                          β pod del, data del 
    local    hostPath   β  mount local  volume.    β pod del, data keep.

    nas      nfs        β  mount remote volume     β pod del, data keep.     β host down, data keep.
    nas      iscsi      β  mount remote volume     β pod del, data keep.     β host down, data keep.
    nas      rbd        β  mount remote volume     β pod del, data keep.     β host down, data keep.
    .... 


    k8s support a lot storage type.
      but ! every type of storage need different config!  not easy for use & config
        so  bset choose is use pv: persisentvolume



π΅ PV & PVC Desc β

    Pod (volume) 
        >> PVC                      β  docker user config  
            >> PV                   β  storage manager config
                >> RealStorage      β  ceph cluster


    PV:   Persistent Volume          β  like physical disk 
    PVC:  Persistent Volume Claim    β  choose which disk/pv  .  and decide the size of disk.

    PV:   Provide  Storage           β  config pv   is  it admin`s     job
    PVC:  Use      Storage           β  config pvc  is  docker user`s  job 


    PV can cofing on all type of storage.
      pv is just like a hard disk to user. 
        user just need config pvc (how to use disk. need how big & etc...)
        user konw nothing about your real storage. 
            user shoud not know this for security. and no need to know this.


          
π΅ StorageClass / Driver / Plugin

  πΆ WHY 

    PV & PVC make k8s storage config much easy.
        but config pv & pvc is still not that easy. 
          it is two people`s job! 
            if you want config pvc. must let it.admin config pv first.

    we have somthing better: Provisioner/StorageClass
        Provisioner/StorageClass can auotmatic create pv when docker user create pvc.
    
    Provisioner/StorageClass is just like driver for storage cluster.
        different storage cluster need different dirver 



  πΆ StorageClass / Driver _ CEPH-CSI

      StorageClass is not buildin.
      we need config StorageClass First! 
        we use ceph-rbd 
        so we need install ceph-csi plugin first.

        with this plugin. k8s can control ceph cluster.
        so k8s can create/delete ceph-rbd disk on ceph.



π΅ CEPH-CSI Desc

    CSI:   Container Storage Interface
      a dirver/plugin for container;   allow k8s control ceph cluster. 
        like NIC: make one pc connect other pc.
        here CSI: make K8s Storage Connect Ceph Storage. 








π΅ How 

  i have a detail demo under sto.rbd.ceph-csi install 
  this is simple demo 


  install cephadm to all k8s node 
    so k8s can support ceph storage. 


  install ceph-csi driver to all k8s node
    so k8s can support ceph much smart 
      like auto create pv for you .


  prepair pool & user in ceph cluster. 

  test pvc in k8s 





π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅ CEPH-CSI prepair 


π΅ install cephadm tool  all node

    sudo apt install -y cephadm 

      test ceph port 
        nmap -p 6789 10.12.12.70



π΅ config ceph-csi plugin/driver in all node


    git clone --depth 1 --branch devel https://github.com/ceph/ceph-csi.git

    cd /root/ceph-csi/deploy/rbd/kubernetes


πΆ 1. edit csi-config-map.yaml

  βΌοΈ only change id&ip βΌοΈ
  βΌοΈ only change id&ip βΌοΈ
  βΌοΈ only change id&ip βΌοΈ


    cat <<EOF > csi-config-map.yaml
    ---
    apiVersion: v1
    kind: ConfigMap
    data:
      config.json: |-
        [
          {
            "clusterID": "b57b2062-e75d-11ec-8f3e-45be4942c0cb",   # βΌοΈ ChanegMe-01 
            "monitors": [
              "10.12.12.70:6789"                                   # βΌοΈ ChangeMe-02
            ]
          }
        ]
    metadata:
      name: ceph-csi-config
    EOF



  kubectl apply -f csi-config-map.yaml
  kubectl get configmap



πΆ 2.  csi-rbd-secret.yaml        β  set Ceph Username & password  β

  βΌοΈ try use ceph admin if possible .  other user have unknow problem 

    cat <<EOF > csi-rbd-secret.yaml
    ---
    apiVersion: v1
    kind: Secret
    metadata:
      name: csi-rbd-secret
      namespace: default
    stringData:
      userID: admin                                         # βΌοΈ ChangeMe-01
      userKey: AQC08qBirxxxxx=     # βΌοΈ ChangeMe-02
    EOF



    kubectl apply -f csi-rbd-secret.yaml
    kubectl get secret



πΆ 3. csi-kms-config-map.yaml    β  set kms / just copy&paste     β

    cat <<EOF > csi-kms-config-map.yaml
    ---
    apiVersion: v1
    kind: ConfigMap
    data:
      config.json: |-
        {}
    metadata:
      name: ceph-csi-encryption-kms-config
    EOF



    kubectl apply -f csi-kms-config-map.yaml








πΆ 4. ceph-csi-config.yaml       β  unknow  / just copy&paste 

    cat <<EOF > ceph-config-map.yaml
    ---
    apiVersion: v1
    kind: ConfigMap
    data:
      ceph.conf: |
        [global]
        auth_cluster_required = cephx
        auth_service_required = cephx
        auth_client_required = cephx
      # keyring is a required key and its value should be empty
      keyring: |
    metadata:
      name: ceph-config
    EOF


      kubectl apply -f ceph-config-map.yaml







πΆ 5. Edit provisioner.yaml      - Option 

    by default it create 3 same pod for backup!
    we k3s for test. only need 1. 


      π» Check  replicas 
        cat csi-rbdplugin-provisioner.yaml | grep replicas
          replicas: 3

      β use sed change to 1 
        sed -i 's/replicas: 3/replicas: 1/g' csi-rbdplugin-provisioner.yaml

      β check result 
        cat csi-rbdplugin-provisioner.yaml | grep replicas
          replicas: 1



πΆ apply other. 

    kubectl create -f csi-provisioner-rbac.yaml
    kubectl create -f csi-nodeplugin-rbac.yaml

    kubectl create -f csi-rbdplugin-provisioner.yaml
    kubectl create -f csi-rbdplugin.yaml




πΆ check status 
    now. ceph-csi is done. you can test.it.
      this need take some time to finisth.

    kubectl get pods
    NAME                                         READY   STATUS    RESTARTS   AGE
    csi-rbdplugin-xp84x                          3/3     Running   0          2m47s
    csi-rbdplugin-7bj26                          3/3     Running   0          2m47s
    csi-rbdplugin-provisioner-5d969665c5-2gvbq   7/7     Running   0          2m51s




π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅ ceph  prepair 

π΅ ceph  prepair 

  πΆ Create Ceph K3s Pool:    CEPH_BD-K3s

      ceph osd lspools
      ceph osd pool create CEPH_BD-K3s 128 128 
      ceph osd pool set CEPH_BD-K3s size 1
      ceph osd pool set CEPH_BD-K3s target_size_bytes 1000G

      sudo rbd pool init CEPH_BD-K3s
      rbd ls CEPH_BD-K3s



  πΆ Create Ceph K3s User : k3s 

    ceph auth get-or-create client.k3s mon 'allow r' osd 'allow rw pool=CEPH_BD-K3s'

        [client.k3s]
                key = AQAJcctixrxxxx


  πΆ get admin key          β     ceph auth get client.admin

        key = AQC08qBixxxx09AX7TtstKNAA==


  πΆ get cluster id / fsid  β     ceph mon dump
        fsid b57b2062-e75d-11ec-8f3e-45be4942c0cb




π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅ k8s pvc demo 

π΅ Config StorageClass β




      cat <<EOF > csi-config-map.yaml
      ---
      apiVersion: v1
      kind: ConfigMap
      data:
        config.json: |-
          [
            {
              "clusterID": "b57b2062-e75d-11ec-8f3e-45be4942c0cb",   # βΌοΈ ChanegMe-01 
              "monitors": [
                "10.12.12.70:6789"                                   # βΌοΈ ChangeMe-02
              ]
            }
          ]
      metadata:
        name: ceph-csi-config
      EOF


    cat <<EOF > sto-sc-cephcsi-rbd-k3s.yaml
    ---
    apiVersion: storage.k8s.io/v1
    kind: StorageClass
    metadata:
      name: sto-sc-cephcsi-rbd-k3s                          # βΌοΈ Change Me 03
    provisioner: rbd.csi.ceph.com
    parameters:
      clusterID: b57b2062-e75d-11ec-8f3e-45be4942c0cb      # βΌοΈ  Change Me 01
      pool: CEPH_BD-K3s                               # βΌοΈ  Change Me 02
      imageFeatures: layering
      csi.storage.k8s.io/provisioner-secret-name: csi-rbd-secret
      csi.storage.k8s.io/provisioner-secret-namespace: default
      csi.storage.k8s.io/controller-expand-secret-name: csi-rbd-secret
      csi.storage.k8s.io/controller-expand-secret-namespace: default
      csi.storage.k8s.io/node-stage-secret-name: csi-rbd-secret
      csi.storage.k8s.io/node-stage-secret-namespace: default
    reclaimPolicy: Delete
    allowVolumeExpansion: true
    mountOptions:
      - discard
    EOF



    kubectl apply -f sto-sc-cephcsi-rbd-k3s.yaml





π΅ pvc create demo

    cat <<EOF > pvc-k3s-db-mysql.yaml
    ---
    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: pvc-k3s-db-mysql                         # βΌοΈ Must         set pvc name
    spec:
      accessModes:
        - ReadWriteOnce                         # βΌοΈ ceph available Option: ReadWriteOnce/ReadOnlyMany
      volumeMode: Block                         # βΌοΈ NoChange:    this is block type. 
      resources:
        requests:
          storage: 500Gi                         # βΌοΈ Option       Change Size
      storageClassName: sto-sc-cephcsi-rbd-k3s   # βΌοΈ Must         Chaneg To your sc name 
    EOF




    kubectl apply -f pvc-k3s-db-mysql.yaml




π΅ PVC debug 

  πΆ check sc pv pvc status 

    kubectl get sc
    kubectl get pv
    kubectl get pvc    β  if ok. it should bond to sc!   


    K3s ~ kubectl get pvc
    NAME          STATUS    VOLUME   CAPACITY   ACCESS MODES   STORAGECLASS             AGE
    sto-pvc-k3s   Pending β                                     sto-sc-cephcsi-rbd-k3s   37m


  πΆ kubectl describe pvc





π΅ pvc use demo


    cat <<EOF > pod-netdebug.yaml
    ---
    apiVersion: v1
    kind: Pod
    metadata:
      name: pod-netdebug                       # βΌοΈ set pod name 
    spec:
      containers:
        - name: nettools                        # βΌοΈ any name
          image: travelping/nettools            # βΌοΈ image url must right
          command:                              # βΌοΈ must run something. ro docker die.
            - sleep
            - "infinity"
          volumeDevices:
            - name: ceph-k3s-pod-nettools        # βΌοΈ any name... same with blow: volumes.name
              devicePath: /tmp                   # βΌοΈ container inside  path
      volumes:
        - name: ceph-k3s-pod-nettools            # βΌοΈ any name... same wuith up:  volumeDevices.name
          persistentVolumeClaim:
            claimName: sto-pvc-k3s               # βΌοΈ use your pvc name
    EOF


    kubectl apply -f pod-netdebug.yaml





π΅ check ceph. 

    CEPH-MGR.Root ~ rbd -p Pool_BD-K8s_Prod ls
    IMG-K8s-Prod
    IMG-K8s-Prod-NoCSI
    csi-vol-59b173b3-ee1b-11ec-8899-0e9db053906f


    βΌοΈ deleted pvc delete disk in ceph;   delete pod. disk still keep βΌοΈ
    βΌοΈ deleted pvc delete disk in ceph;   delete pod. disk still keep βΌοΈ
    βΌοΈ deleted pvc delete disk in ceph;   delete pod. disk still keep βΌοΈ

    pvc is like disk.  you can mount all pod folder into one pvc. 
    but never delete pvc.

    but. how to create folder. in pvc? 
    how my k8s node can visit pvc folder. 


