---
sidebar_position: 3930
title: ðªðªðª-5ï¸â£ð ð ð  â Jump Server
---

# Jump Server



ðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµ  Jump Server.
ðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµ  Jump Server.
ðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµ  Jump Server.

pc (internet)  <====>   vps (internet)
    vps (wireguard.Srv)   <======>   homelab-JumpServer: wireguard.Cli
        homelab-JumpServer <======> Homelab-other machine 


ðµ WHY 

    visit homelab`s internel server  via internet is a must.
    you can install vpn to any client in  homelab
    but best way is use jumpserver.

    any want connect into your homelab. 
    need login to jumpser first. then type in usernam/password.
    



ðµ Jump Server Function
    log everything you do! 



ðµ OpenSource Jump Server
    
    jumpserver/ teleport
    https://www.jumpserver.org/


    ð¶jms_all
        https://hub.docker.com/r/jumpserver/jms_all

    need mysql


ðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµ Prepair  - mysql 

ðµ Login 

    mycli -h DockerProd.ark -P 3306 -u root
    SHOW DATABASES;

ðµ Check Database 

    â simple info â   SHOW DATABASES;
    â detail info â   select * from information_schema.schemata;


ðµ create DB: DBjumpserver âï¸

    create database DBNAME CHARACTER SET utf8 COLLATE utf8_bin;
        create database DBjumpserver CHARACTER SET utf8 COLLATE utf8_bin;
            â¼ï¸ Database coding must requirements  uft8 â¼ï¸
            â¼ï¸ Database coding must requirements  uft8 â¼ï¸


ðµ Create user: â

    CREATE USER 'USERjumpserver'@'%' IDENTIFIED BY 'password';

        % only means. can remote to mysql use server`s lan ip.
        or bydefault. not allow user remote login.


ðµ Give Permit â

    GRANT ALL PRIVILEGES ON DBjumpserver.* TO 'USERjumpserver'@'%';
    FLUSH PRIVILEGES;

        only give USERjumpserver access DBjumpserver. 
        no access other db on mysql.


ðµ mysql remote test 

    mycli -h 172.16.1.140 -P 3306 -u USERjumpserver
    show databases;





ðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµ create key

ð¶ WHY 

    the data in jumpserver is encrypted! 
    you must create a key when create jumpserver.
    if need update docker some day.
    use the same key to unlock the old encrypted data ??

    you can create key any linux machine.  (linux need have /dev/urandom )
    just need keep the key! 



ð¶ Create SECRET_KEY

    if [ "$SECRET_KEY" = "" ]; then SECRET_KEY=`cat /dev/urandom | tr -dc A-Za-z0-9 | head -c 50`; echo "SECRET_KEY=$SECRET_KEY" >> ~/.bashrc; echo $SECRET_KEY; else echo $SECRET_KEY; fi


ð¶ Create  BOOTSTRAP_TOKEN

    if [ "$BOOTSTRAP_TOKEN" = "" ]; then BOOTSTRAP_TOKEN=`cat /dev/urandom | tr -dc A-Za-z0-9 | head -c 16`; echo "BOOTSTRAP_TOKEN=$BOOTSTRAP_TOKEN" >> ~/.bashrc; echo $BOOTSTRAP_TOKEN; else echo $BOOTSTRAP_TOKEN; fi




ðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµðµ Run Jumpserver

ðµ First time 

    need tell jumpserver docker how connect db.
    after connected.  jumpserver docker will create some mysql file.
    then copy that data out.

    next time. run a new jumpserver docker 
    no need type in mysql info.
    just need mount the mysql folder.


ð¶ first run

    docker run --name DPjumpserver -d \
      -v /mnt/ceph-img-Docker-prod/DPdata/jumpserver/data:/opt/jumpserver/data \
      -p 80:80 \
      -p 2222:2222 \
      -e SECRET_KEY=vvLHJn9gLneiZsXtPbr019zvFsUE4AmgyNM7psyMbMkWuu7tMf \
      -e BOOTSTRAP_TOKEN=f48gD26NeNgl2QfD \
      -e DB_HOST=dockerprod.ark \
      -e DB_PORT=3306 \
      -e DB_USER=USERjumpserver \
      -e DB_PASSWORD=xxxxxxxx \
      -e DB_NAME=DBjumpserver \
      jumpserver/jms_all:latest





ðµ web login 

    http://172.16.1.140/ui/#/
    http://172.16.1.140/core/auth/login/

    â¼ï¸ fuck chrome no web. safari works. â¼ï¸ 
    â¼ï¸ fuck chrome no web. safari works. â¼ï¸ 
    â¼ï¸ fuck chrome no web. safari works. â¼ï¸ 


need long time to start. 