---
sidebar_position: 1930
title: ๐ช-9๏ธโฃ๐๐๐ CEPH โโ Use
---

# # Storage โถ CEPH โ Use



๐ต Good File 

โญ๏ธโญ๏ธโญ๏ธโญ๏ธโญ๏ธ
https://zhangzhuo.ltd/articles/2021/06/18/1623993444076.html


โญ๏ธโญ๏ธโญ๏ธโญ๏ธ
https://www.koenli.com/ef5921b8.html



https://blog.51cto.com/renlixing/3134294


๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต Basic Info 
๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต Basic Info
๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต Basic Info

๐ต POOL PG PGP Desc  

    ๐ถ Pool 

        ceph osd pool create poolname PG PGP 
        ceph osd pool create mytestpool 128 128 

        ๅๅปบpool ๆถๅๅฟ้กปๆๅฎ PG ๆฐ้.
        PG ๆฐ้ ๆฏ่ชๅจๅๅธๅฐ ๆๆ OSD็.
        ๆไปฅไฝ ็ Pool ไนๆฏๅๅธๅฐๆดไธช ้็พค็.



    ๐ถ PGs Desc  โ

        ceph cluster have lots physical disk.
            to manager disk easyier. we create PGs ( assign physical disk to some group )
                so we only need manage PGs.



    ๐ถ PG / PGP 

        PG ่ฆๆ นๆฎ OSD ๆฐ้่ฟ่ก่ฐๆด.
        PGP ๅ PG ็ๅผไฟๆไธ่ดๅฐฑ่ก.
        ๆนPG ็ๅๆถ่ฆ PGP ไธ่ตทๆน.


        ๅฐไบ50 osd ๅฏไปฅ็จ่ชๅจๆนๅผ. ๅๅคๅฐฑ่ฆ่ชๅทฑ็ฎไบ. 
            pg_autoscale_mode: on   ๅฐฑๅฏไปฅไบ

    

๐ต Storage Develop 

    DAS:  Dirtect Attached Storage 
        โ NAS:  Network Attached Storage 
            โ SAN:  Storage Area Network 
                โ Object Storage



๐ต CEPH Function:

    Object Storage โ like AWS S3    โ For Share              โ high speed 
    Block  Storage โ like iscsi     โ For One Device Only    โ Fast Speed 
    File   Storage โ like NFS/SMB   โ For Share              โ Slow Speed

        object storage have both advantage of  block storage and file storage.
            if data that don`t change too often.    โ Object storage is best.  like picture/video 
            if you install VM                       โ Block Storage is better.



 

๐ต CEPH FIle  Storage Workflow

    1. File   >> Object           cut big file to samll object(2M-4M)
        2. Object >> PG           give small object a PGs.
            3. PG     >> OSD      Write  small object to OSD


 
๐ต NEW OSD 

    when add new driver.
    it will automatic move date between osdes. 



๐ต Pool Commands 

    ๐ถ Pool 

        โ Create pool     ceph osd pool create CEPH_FS-APP 128 128 
        โ Rename Pool     ceph osd pool rename CEPH-Test CEPH_FS-NAS-Ceph
        โ List   pool     ceph osd lspools
        โ Delete Pool




    ๐ถ AutoScale Mode  

        โ Set Mode On    ceph osd pool set CEPH-Test pg_autoscale_mode on
        โ Check Mode     ceph osd pool autoscale-status
            default: on 


    ๐ถ Pool Size 

        ceph osd pool set CEPH-Test target_size_bytes 10G



    ๐ถ Pool replica size 

        ceph osd pool set CEPH-Test size 2
        
        default is 3 
        you can change this anytime.
        if you have space set 3/2
        if no space set to 1 at any time.


    ๐ถ Delete Pool โ

        โ Allow Delete pool 
            by default it is not allowed.

            ceph tell mon.\* injectargs '--mon-allow-pool-delete=true'


        โ How Delete 

            ceph osd pool delete {pool-name} {pool-name} --yes-i-really-really-mean-it
                type pool name twice. 
                    and follew --yes-i-really-really-mean-it

            ceph osd pool delete test-pool test-pool --yes-i-really-really-mean-it

            ceph osd pool delete OB-Zone-Ark.rgw.meta OB-Zone-Ark.rgw.meta --yes-i-really-really-mean-it




๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต Filesystem Demo 
๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต Filesystem Demo 
๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต Filesystem Demo 

๐ต Server โถ Pool Config 

    ๐ถ Create Pool for cephfs 

        ceph osd pool create CEPH_FS-NAS-Ceph 128 128 
        ceph osd pool create CEPH_FS-NAS-Ceph-metadata 128 128 


    ๐ถ Assign Pool Type to Filesystem Storage

        โ Desc 
            sudo ceph osd pool application enable PoolNAME StorageType
                ceph have tree storage type: file Type /Block Devie Type /Object Type.
                so you need set pool use which type.
                here cephfs type.
     
        โ Demo 
            sudo ceph osd pool application enable CEPH_FS-NAS-Ceph cephfs
            sudo ceph osd pool application enable CEPH_FS-NAS-Ceph-metadata cephfs


    ๐ถ Create fs:  cephfs

        ceph fs new cephfs CEPH_FS-NAS-Ceph-metadata CEPH_FS-NAS-Ceph


    ๐ถ Check fs status

        CEPH-MGR.Root ~ ceph fs ls
        name: cephfs, metadata pool: CEPH_FS-NAS-Ceph-metadata, data pools: [CEPH_FS-NAS-Ceph ]


    ๐ถ assign cephfs to node

        ceph orch apply mds cephfs --placement="1  CEPH-Node"


    ๐ถ check mds status 

        CEPH-MGR.Root ~ ceph orch ps --daemon-type mds
        NAME                         HOST       STATUS         REFRESHED  AGE  VERSION  IMAGE NAME             IMAGE ID      CONTAINER ID
        mds.cephfs.CEPH-Node.tevxqv  CEPH-Node  running (33m)  4m ago     33m  15.2.16  quay.io/ceph/ceph:v15  8d5775c85c6a  6e5f7903011d


๐ต Server โถ User Create 

    ๐ถ Create user for ceph-fs mount

        ceph auth get-or-create client.miranda mon 'allow r' mds 'allow r, allow rw path=/' osd 'allow rw pool=data' -o ceph.client.miranda.keyring

                 โ username:  client.miranda

            โผ๏ธ cd /etc/ceph/  firts.   user name must be like  client.xxxx /  only change miranda above. leave else โผ๏ธ 
            โผ๏ธ cd /etc/ceph/  firts.   user name must be like  client.xxxx /  only change miranda above. leave else โผ๏ธ 
    

    ๐ถ cat user token (option)

        ceph auth get-key client.miranda




๐ต Client โถ Mount (Ceph-fuse)

        โผ๏ธ ceph-fuse is like cephadm. no ububtu22 now. use ubuntu 20! โผ๏ธ 
        โผ๏ธ ceph-fuse is like cephadm. no ububtu22 now. use ubuntu 20! โผ๏ธ 
        โผ๏ธ ceph-fuse is like cephadm. no ububtu22 now. use ubuntu 20! โผ๏ธ 


    ๐ถ Insatll ceph-fuse 

        โ Ubuntu 20:   sudo apt install ceph-fuse  

    ๐ถ Make dir 

        sudo mkdir -p /etc/ceph


    ๐ถ Copy Moniter ceph.conf  to Client 

        sudo scp {user}@{server-machine}:/etc/ceph/ceph.conf /etc/ceph/ceph.conf
        sudo scp root@10.12.12.70:/etc/ceph/ceph.conf /etc/ceph/ceph.conf

            monitor node need enable root user & allow root ssh 


    ๐ถ Copy Moniter xxxx.keyring  to Client

        sudo scp {user}@{server-machine}:/etc/ceph/ceph.keyring /etc/ceph/ceph.keyring
        sudo scp root@10.12.12.70:/etc/ceph/ceph.client.miranda.keyring /etc/ceph/fs-miranda.keyring
            
            this download keyring  and renamed to fs-miranda.keyring 


    ๐ถ Make mount Folder 

        sudo mkdir /mnt/ceph-cephfs-MountPoint


    ๐ถ Mount cephfs 

        ceph-fuse [-n client.username] [ -m monaddr:port ] mountpoint [ fuse options ]
        sudo ceph-fuse -n client.miranda -m 10.12.12.70:6789 /mnt/ceph-cephfs-MountPoint


    ๐ถ check nfs 

        mount -l | grep ceph



๐ต Client โถ Auto Mount - ceph fs  ๐ฏ

    ๐ถ Create Script 

        sudo bash -c 'echo "
        #!/bin/bash
        ceph-fuse -n client.miranda -m 10.12.12.70:6789 /mnt/ceph-cephfs-MountPoint
        " > /root/crontab-script-ceph-fs-Mount-NoDEL.sh'


    ๐ถ Give Perssmion

            chmod +x /root/crontab-script-ceph-fs-Mount-NoDEL.sh



    ๐ถ crontab desc 

        โฆฟ  crontab -l   check  task
        โฆฟ  crontab -e   edit/add task
        โฆฟ  crontab -r   remove  task 


    ๐ถ enter crontab edit 

        crontab -e


    ๐ถ add a line & save file 

        @reboot /root/crontab-script-ceph-fs-Mount-NoDEL.sh


    ๐ถ reboot tast 


 
 









===============================================
๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ตBlock Device Demo
๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ตBlock Device Demo
๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ตBlock Device Demo

https://documentation.suse.com/zh-cn/ses/7/html/ses-all/cha-ceph-as-iscsi.html
??



๐ต Server โถ Create Block Device Pool
 



    ๐ถ create pool 

        ceph osd pool create CEPH_BD-K8s-Prod 128 128 
        ceph osd pool create CEPH_BD-K8s-Test 128 128 



    ๐ถ Change repeat size to 2

        ceph osd pool set CEPH_BD-K8s-Prod size 2
        ceph osd pool set CEPH_BD-K8s-Test size 2



    ๐ถ Init Pool 

        sudo rbd pool init CEPH_BD-K8s-Prod
        sudo rbd pool init CEPH_BD-K8s-Test 

            Filesystem and Object Gateway  No need Initialize 
            only Block Device need use rbd to initialize  


    ๐ถ list pool 

        ceph osd lspools


======================================

๐ต Server โถ Create User for Mount 

    โผ๏ธ By default Block Device Storage Use admin account to mount, No Need Create User. โผ๏ธ
    โผ๏ธ By default Block Device Storage Use admin account to mount, No Need Create User. โผ๏ธ
    โผ๏ธ By default Block Device Storage Use admin account to mount, No Need Create User. โผ๏ธ


    ๐ถ options 

        ceph auth get-or-create client.BD-User01 mon 'profile rbd' osd 'profile {profile name} [pool={pool-name}][, profile ...]' mgr 'profile rbd [pool={pool-name}]'

        โ read only user 
        ceph auth get-or-create client.BD-K8s-Prod-User01 mon 'profile rbd' osd 'profile rbd pool=CEPH_BD-K8s-Prod, profile rbd-read-only pool=images' mgr 'profile rbd pool=images'

        [client.BD-K8s-Prod-User01]
            key = AQCWq6FiJSG7Ah

        ceph auth get-or-create client.BD-K8s-Test-User01 mon 'profile rbd' osd 'profile rbd pool=CEPH_BD-K8s-Test, profile rbd-read-only pool=images' mgr 'profile rbd pool=images'

        [client.BD-K8s-Test-User01]
            key = AQCpq6Fiw/sJNBAAQx



========================================

๐ต Server โถ Create Image โ

    ๐ถ Image Desc 

        Pool is for ceph cluster;   Image is for Client to Mount;
            Image is like Hardware Disk let client mount & use.
                Create Image for pool_xxx  first.
                    Create User for Mount.
                        Client Mount.

        
        image is created under/inside pool. 


    ๐ถ Create Image 

        rbd create --size {megabytes} {pool-name}/{image-name}

        rbd create --size 10240 CEPH_BD-K8s-Prod/IMG-K8s-Prod
        rbd create --size 10240 CEPH_BD-K8s-Test/IMG-K8s-Test

            image is under pool.
            create 10G image under pool 


    ๐ถ List Image 

        rbd ls {poolname}
        rbd ls CEPH_BD-K8s-Prod
        rbd ls CEPH_BD-K8s-Test


    ๐ถ Check Image info 

        rbd info {pool-name}/{image-name}
        rbd info CEPH_BD-K8s-Prod/IMG-K8s-Prod
        rbd info CEPH_BD-K8s-Test/IMG-K8s-Test



    ๐ถ Resize Image  

        rbd resize --size 20480 IMG-K8s-Prod                     โ increase size 
        rbd resize --size 1024 IMG-K8s-Test --allow-shrink       โ decrease size  

            image only take space when use it. 
                if only create, no put data in. the real size is zero.

========================================

๐ต Client โถ Mapping Image  โ

    ๐ถ install Ceph Common

        U20-TempleOnly ~ sudo apt install ceph-common


    ๐ถ Copying Conf and Keyring from moniter node

        sudo scp root@10.12.12.70:/etc/ceph/ceph.conf /etc/ceph/ceph.conf
        sudo scp root@10.12.12.70:/etc/ceph/ceph.client.admin.keyring /etc/ceph/ceph.client.admin.keyring

            No Change filename on client side after copy
            when mapping. it use default name to find server info.
            or next mapping will wrong 


    ๐ถ mapping

        mapping is like put new disk to client server.
        if need use disk. still need format & mount.

        โ Check status    rbd showmapped 
        โ map             rbd map Pool_BD-K8s-Prod/IMG-K8s-Prod -p rbd





=====================

๐ต Client โถ Mount disk    โ

    atter Mapping successful.
        it is like a new hardware disk.
            format it to xfs &&  mount it; 
                then we can use it.


    ๐ถ check disk name 

        fdisk -l  
        Disk /dev/rbd0: 10 GiB, 10737418240 bytes, 20971520 sectors


    ๐ถ Format To XFS 

        mkfs.xfs /dev/rbd0 



    ๐ถ Make Mount Point 

        mkdir /mnt/ceph-img-k8s-prod


    ๐ถ Mount 

        mount /dev/rbd0 /mnt/ceph-img-k8s-prod/


    ๐ถ Verify 

        df -T 
        df -hl | grep rbd



๐ต Client โถ Auto Mount ๐ฏ

    ๐ถ Create Script 

        sudo bash -c 'echo "
        #!/bin/bash
        rbd map Pool_BD-K8s_Prod/IMG-K8s-Prod -p rbd
        mount /dev/rbd0 /mnt/ceph-img-k8s-prod/
        " > /root/crontab-script-ceph-DB-Mount-NoDEL.sh'


        ๐ถ Give Perssmion โผ๏ธ

            chmod +x /root/crontab-script-ceph-DB-Mount-NoDEL.sh



    ๐ถ crontab desc 

        โฆฟ  crontab -l   check  task
        โฆฟ  crontab -e   edit/add task
        โฆฟ  crontab -r   remove  task 


    ๐ถ enter crontab edit 

        crontab -e


    ๐ถ add a line & save file 

        @reboot /root/crontab-script-ceph-DB-Mount-NoDEL.sh


    ๐ถ reboot tast 




๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต
๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต  OB Demo  
๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต 
โโโ Give Up.  can not enable fuck serverice..



โผ๏ธ Offical manual ..

https://docs.ceph.com/en/pacific/radosgw/pools/


radosgw  commond 
https://www.mankier.com/8/radosgw-admin


๐ต AWS S3 Desc  

    Amazon Simple Storage Service

    offer a web.  in web you can control all.
    every file have a url. 
    you can use it in your web. 
    good for website!!! 



๐ต Basic info 

    rgw-us-east:
    realm: replicated
    zonegroup: us
    zone: us-east
    rgw-us-west:
    realm: replicated
    zonegroup: us
    zone: us-west



    zone + zone ... >> Zone Group 
        zone group + zone group + ... >> realm 

    all zone under the same zone group  can sync between zone.
        this make backup between two physical place possible.
            like Zone-Group-Global
                zone_1 put in china 
                zone_2 put in Us
                ...



    โผ๏ธ no commit no take effeci:             radosgw-admin period update --commit
    โผ๏ธ no commit no take effeci:             radosgw-admin period update --commit
    โผ๏ธ no commit no take effeci:             radosgw-admin period update --commit



 
 
๐ถ Add placement target....
 
    $ radosgw-admin zonegroup placement add \
        --rgw-zonegroup OB-Group-Ark \
        --placement-id temporary


    $ radosgw-admin zone placement add \
        --rgw-zone OB-Zone-Ark \
        --placement-id temporary \
        --data-pool default.rgw.temporary.data \
        --index-pool default.rgw.temporary.index \
        --data-extra-pool default.rgw.temporary.non-ec



๐ต 

๐ถ Create realm:   

    radosgw-admin realm create --rgw-realm=OB-Realm-Ark --default 


๐ถ Create zone group:  

    radosgw-admin zonegroup create --rgw-zonegroup=OB-Group-Ark --master --default


๐ถ Create zone:  

    radosgw-admin zone create --rgw-zonegroup=OB-Group-Ark --rgw-zone=OB-Zone-Ark --master --default


๐ถ update period

    radosgw-admin period update --rgw-realm=OB-Realm-Ark --commit 



๐ถ Create RGW  

    orch apply rgw <realm_name> <zone_name> 
        ceph orch apply rgw OB-Realm-Ark OB-Zone-Ark --placement="1 CEPH-Node"


๐ถ Check rgw 

    ceph orch ps --daemon-type rgw
    No daemons reported

    ๐ web:   No RGW service is running.

    



๐ถ Create USER  โ

    radosgw-admin user create --uid=rgw-user --display-name=rgw-user --system


    "user_id": "rgw-user",
    "display_name": "rgw-user",

            "user": "rgw-user",
            "access_key": "XD0QFJQCEJMSLEYQACGS",
            "secret_key": "WboesDxdzMezAMwQd85xpL26BAckiJfCvlZpt0Ci"
   


    ่ฎฐๅฝaccess_keyๅsecret_key็ๅผไฟๅญไธบ access_key.txt ๅsecret_key.txt ๏ผ้่ฟๅฝไปค้ๆๅฐdahsboardไธญใ

    ceph dashboard set-rgw-api-access-key -i accesskey.txt 
    ceph dashboard set-rgw-api-secret-key -i secretkey.txt 



 
  


๐ต OB  client ..

    # ๅฎ่ฃ AWS s3 API
    [root@ceph01 ~]# yum -y install s3cmd

    # ๅๅปบ็จๆท
    [root@ceph01 ~]# radosgw-admin user create --uid=s3 --display-name="objcet storage" --system

    # ่ทๅ็จๆท access_key ๅ secret_key
    [root@ceph01 ~]# radosgw-admin user info --uid=s3 | grep -E "access_key|secret_key"
                "access_key": "RPRUFOWDK0x",
                "secret_key": "32efWJ7OKx"

    # ็ๆ S3 ๅฎขๆท็ซฏ้็ฝฎ(่ฎพ็ฝฎไธไธๅๆฐ, ๅถไฝ้ป่ฎคๅณๅฏ)
    [root@ceph01 ~]# s3cmd --configure
    Access Key: RPRUFOx
    Secret Key: 32efWJ7OeKJx
    S3 Endpoint [s3.amazonaws.com]: ceph01
    DNS-style bucket+hostname:port template for accessing a bucket [%(bucket)s.s3.amazonaws.com]: %(bucket).ceph01
    Use HTTPS protocol [Yes]: no
    Test access with supplied credentials? [Y/n] y
    Save settings? [y/N] y
    Configuration saved to '/root/.s3cfg'

    # ๅๅปบๆกถ
    [root@ceph01 ~]# s3cmd mb s3://bucket
    Bucket 's3://bucket/' created

    # ๆฅ็ๅฝๅๆๆๆกถ
    [root@ceph01 ~]# s3cmd ls
    2020-06-28 03:02  s3://bucket
    -----------------------------------
    ยฉ่ไฝๆๅฝไฝ่ๆๆ๏ผๆฅ่ช51CTOๅๅฎขไฝ่็บขๅฐไธ้ด็ๅๅไฝๅ๏ผ่ฏท่็ณปไฝ่่ทๅ่ฝฌ่ฝฝๆๆ๏ผๅฆๅๅฐ่ฟฝ็ฉถๆณๅพ่ดฃไปป
    ไฝฟ็จcephadmๅฟซ้ๆญๅปบceph้็พค
    https://blog.51cto.com/hongchen99/2507660






 











๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต
๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต
๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต ceph auth / user permit 

๐ต WHY CephX 

    both ceph(server) and k8s(client) have a same key.
    use this can avoid hacker atteck. 

    if any client need use ceph.
    ceph have to create use first.
    then ceph will create a username & a key.

      [client.k8s]
          key = AQBTvaZi0HhkABAAgn 


๐ต CephX Auth 

    CephX manager whole ceph cluster login system.

    inside ceph:  
        not only user(clietn.admin .. ) need password. 
        mon/osd/mds need password to login ceph too.


    outside ceph: (like k8s cluster) 
        if k8s want connect/use ceph cluster must need four info.
            โ ceph cluster id:   fsid 
            โ ceph cluster monitor ip address. 
            โ username 
            โ password 


    we use mon node to build ceph cluster.
    so even you lost client.admin password. 
    mon node can reset password for admin.



๐ต permit analyze  

    CEPH-MGR.Root ~ ceph auth ls

    client.admin
        key: AQC08qBirX7TLhAAn
        caps: [mds] allow *
        caps: [mgr] allow *
        caps: [mon] allow *
        caps: [osd] allow *

    CEPH-MGR.Root ~ ceph auth ls
    client.k8s
        key: AQBTvaZi0HhkABAAgYs 
        caps: [mgr] profile rbd pool=Pool_BD-K8s_Test
        caps: [mon] profile rbd
        caps: [osd] profile rbd pool=Pool_BD-K8s_Test

    client.miranda
        key: AQC8nKFiQgA+A
        caps: [mds] allow r, allow rw path=/
        caps: [mon] allow r
        caps: [osd] allow rw pool=data


 
    ๐ถ permit 

        *  = rwx
        r:   allow read 
        w:   allow write 
        x:   allow run auth commonds (ceph auth list / ceph guth get)


    ๐ถ [mon] / [mgr] / [mds]  / [osd]

        โ caps:  capabilities 

        ceph cluster have a lot role.
        differnet role have differnet job/function.
        if you need give someone ceph`s permission.
        need add role permit first.  then set detail permit under that role. 

            caps: [osd] allow *               โ have all control to osd role.
            caps: [osd] allow rw pool=data    โ only read+wirte under data pool. 
            

    ๐ถ profile rbd pool=Pool_BD-K8s_Test




๐ต Set Permit Demo 

https://www.shuzhiduo.com/A/mo5k4alQdw/



๐ต Create/get User 

    ceph auth get-or-create client.k8s mon 'allow r' osd 'allow rw pool=Pool_BD-K8s_Test'


๐ต Change User Permit โ

    ceph auth get client.k8s

    ceph auth caps client.k8s mon 'allow r' osd 'allow rw pool=Pool_BD-K8s_Test'    โ rw  one pool
    ceph auth caps client.k8s mon 'allow r' osd 'allow *'                           โ rw  all pool 




