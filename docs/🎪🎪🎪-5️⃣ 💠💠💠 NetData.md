---
sidebar_position: 3930
title: πͺπͺπͺ-5οΈβ£π π π  β NetData
---

# Moniter βͺ  NetData 





π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅HomeLAB βΆ Moniter βͺ  NetData 




π΅ Manual 

πΆ Offical
https://learn.netdata.cloud/docs/agent/packaging/installer/methods/kubernetes


β­οΈβ­οΈβ­οΈβ­οΈβ­οΈβ­οΈβ­οΈ
https://www.gitiu.com/journal/netdata-install/




π΅ WHY  
zabbix + grafana too heavy. 


π΅ NetData Desc 
https://github.com/netdata/netdata




π΅ Docker install 


  docker run -d --name=netdata \
    -p 19999:19999 \
    -v netdataconfig:/etc/netdata \
    -v netdatalib:/var/lib/netdata \
    -v netdatacache:/var/cache/netdata \
    -v /etc/passwd:/host/etc/passwd:ro \
    -v /etc/group:/host/etc/group:ro \
    -v /proc:/host/proc:ro \
    -v /sys:/host/sys:ro \
    -v /etc/os-release:/host/etc/os-release:ro \
    --restart unless-stopped \
    --cap-add SYS_PTRACE \
    --security-opt apparmor=unconfined \
    netdata/netdata



π΅ Cloud free ... netdata ..



π΅  Usage.. server + agent 





π΅ Agent  - MAC 






π΅ Agent. esxi ??



