---
sidebar_position: 2930
title: πͺπͺπ¬βΈοΈβΈοΈ3οΈβ£ STO β Demo
---


# Storage βΆ Demo


π΅ Must Know 

  πΆ rbd vs nfs 

    most cluster storage use rbd / nfs
        - rbd is for one machine/pod 
        - nfs if for lot machine/pod 


  πΆ pvc VS volume-folder 

    we use rbd 
      β means one pod need one pvc!
      β need create a lot pvc! 

      β means every folder you mount to docker/docker-compose
        you need create a pvc in k8s.

      pvc is like volume-folder.
          you delete pod. pvc still there.
          but you delete pvc. all data gone.


  πΆ PV / StorageCass

      pv just like a hardware disk.
      you just need decide which disk to use.
        there are lots kinds of disk -.- 

          - prod disk
          - test disk

          - fast disk 
          - slow disk 

          - backup-yes disk 
          - backup-no  disk  


  πΆ PV & PVC 

      PV:   like formated disk. 
      PVC:  like folder 

            pv & pvc is same to you .
            you delete pvc. it maybe auto delete pv for you.
                - depends is storage support this function 


  πΆ StorageCass & real storage cluster 




π΅ How PVC Work 

  you create pvc.
      π₯ k8s auto create pv. & bond to your pvc.


  you crteate pod & use    pvc 

  you delete  pod & delete pvc   
      you no need keep data, so you  delete your pvc.
      if you need keep data, never   delete your pvc. 
          π₯ k8s maybe auto delete pv for you !!!!!
                  - depends is storage support this function 



π΅ StorageClass config 

    StorageClass decide what kind of disk/pv  user can use. 

    StorageClass is bond to storage cluster pool. 
      diffeent pool have different config. 
        how config storageclass depends on your pool conig.

    you no need much kind of disk/storageclass.

        SC-Test.0 β Test pool    β  Normal disk with  0 copy  backup 
        SC-Prod.2 β Prod pool    β  Normal disk with  2 copy  backup
        SC-DB     β DB pool      β  high performace disk 





π΅ PVC  vs  Volume_folder 

  in docker β  volume is a folder  on host.
  in k8s    β  pvc    is a rbd disk on pod (not on host)


      in docker you can creat folder/file for volume very easy.
          it is local
      
      in k8s. you can only visit pvc folder in pod!
          it is rbd disk.
            rbd means one disk only allow one pod to use.
              some pods can not start without some config file.
                  you have to mount pvc to other pod in order to put file in pvc.
                    -.-







π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅
π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅ misc 
π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅



π΅ PV Access Modes β

  β RWO  ReadWriteOnce 
  β ROX  ReadOnlyMany 
  β RWX  ReadWriteMany  

  Access Modes is how disk can mount to node.

  most common is RWO:  
    disk mount to one node only. that node can read and write data.
  
  many storage like ceph support ROX: 
    Disk can mount to many node, but only one node can write.

  less storage support RWX: 
    disk can mount to many node. all node can read and write! 

  not all storage support three above mode.
  βΌοΈ ceph-csi only support RWO ROX.  No RWX  βΌοΈ
  βΌοΈ ceph-csi only support RWO ROX.  No RWX  βΌοΈ
  βΌοΈ ceph-csi only support RWO ROX.  No RWX  βΌοΈ


==
π΅ RBD-Pod Volume set

  pod 
    >> spec 
      >> container. 
        >> VolumeDevice      # βΌοΈ  this means usd rbd type storage          
          - name: KeepWeSame # βΌοΈset volume to use. name is just blow 3 line. must keep same
          - devicepaths      #  Path into Docker

  pod 
    >> spec 
      >> volumes:
        - name: KeepWeSame






π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅
π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅
π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅

π΅ one pvc . mount many folder. β 

  apiVersion: v1
  kind: Pod
  metadata:
    name: test
  spec:
    containers:
      - name: test
        image: nginx
        volumeMounts:
          # η½η«ζ°ζ?ζθ½½
          - name: config
            mountPath: /usr/share/nginx/html
            subPath: html
          # Nginx ιη½?ζθ½½
          - name: config
            mountPath: /etc/nginx/nginx.conf
            subPath: nginx.conf
    volumes:
      - name: config
        persistentVolumeClaim:
          claimName: test-nfs-claim





