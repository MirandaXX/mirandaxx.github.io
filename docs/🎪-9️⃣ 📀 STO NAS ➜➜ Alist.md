---
sidebar_position: 1910
title: πͺ-9οΈβ£π STO NAS ββ Cloud Driver
---

# NAS βΆ Cloud β Alist




π΅ Needs: Sync β  cloud & nas 

    1. use synology cloud sync app: sync all dropbox & google drive to NAS
    2. use nas / alist to manage synced folder in NAS


π΅ Needs: Sync β  local & local 


π΅ Need: Manager β

    connect all cloud drive & local drive to alist.
    use alist website to manage all dirve.

    you can share file to other people use alist too. 
    have guest mode. 




π΅ Misc 

    s3cmd?  
        linux mount nas`s s3.
        use rsync. put all config in  s3






π΅ Needs:  Version Control / git 





π΅ Need: Share / web/ internet / webdav

	how share file to other people .











π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅  Alist  Demo 

π΅ Alist - Docker 

	docker run -d --restart=always 
	-v /etc/alist:/opt/alist/data 
	-p 5244:5244 
	--name="alist" 
	xhofe/alist:latest




π΅ Login 

    http://10.1.1.89:5244/@manage/login

    πΆ default password 

        check log
        docker exec -it alist ./alist -password



π΅ change admin password.


π΅ add local folder

    πΆ mount 

        mount nas folder into docker.
        alist mount local folder.


    πΆ add account/folder

        type: native (local dirve)
        virtual path:   /xxx   β what path show on alist dashboard.
        index (set folder order)
        /


        extract_folderβ
            front  when order. put folder at head
            back:  when order. put folder at last



π΅ add folder password 

    alisy.0214.icu/Dir1/xxx
    alisy.0214.icu/Dir1/yyy

    if want give folder Dir1  add password 

    meta
    path: /Dir1
    password:  set a password 
    save 



π΅ set alist reserve proxy 

    use traefik 






π΅ Add remote google Driver  ββββββ

	βΌοΈ  Offical best manual    https://alist-doc.nn.ci/docs/driver/googledrive/
	βΌοΈ  Offical best manual    https://alist-doc.nn.ci/docs/driver/googledrive/
	βΌοΈ  Offical best manual    https://alist-doc.nn.ci/docs/driver/googledrive/


πΆ token tool:

    https://tool.nn.ci/google/request


