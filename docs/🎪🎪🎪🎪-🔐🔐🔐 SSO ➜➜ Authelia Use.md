---
sidebar_position: 4930
title: ๐ช๐ช๐ช๐ช-๐๐๐ SSO โโ Authelia USE
---

# SSO Authelia USE




๐ต goal

  authelia + openldap โ

  authelia + treafik 





๐ต 
1. 

0214.icu >>  vps traefik (ssl)
    vpn: openldap + authelia 

    if need traefik  combind authelia.. need in same docker network.????
    we can use public port..


or add a local traefik .




0214.icu >>  vps traefik (ssl)   <===>  VPN. traefik local. 
    lan:  traefik +  openldap + authelia 


. use swarn.  or...k8s.  or ...




๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต

๐ต How
use traefik middle ware.











๐ต traefik + authelia 


https://github.com/authelia/authelia/blob/master/examples/compose/lite/docker-compose.yml




https://wbsu2003.4everland.app/2022/03/09/%E5%8D%95%E7%82%B9%E7%99%BB%E5%BD%95%E6%9C%8D%E5%8A%A1Authelia%EF%BC%88%E4%B8%8B%E7%AF%87%EF%BC%89/





Outlineไฝฟ็จAutheliaๅฎ็ฐๆฌๅฐ่ฎค่ฏ


https://wbsu2003.4everland.app/2022/03/11/Outline%E4%BD%BF%E7%94%A8Authelia%E5%AE%9E%E7%8E%B0%E6%9C%AC%E5%9C%B0%E8%AE%A4%E8%AF%81/









https://www.youtube.com/watch?v=yHICIVhm_Lo&t=621s&ab_channel=Sagit





















    services:
      authelia:
        image: authelia/authelia
        container_name: authelia
        volumes:
          - ./authelia:/config
        networks:
          - net
        labels:
          - 'traefik.enable=true'
          - 'traefik.http.routers.authelia.rule=Host(`authelia.example.com`)'
          - 'traefik.http.routers.authelia.entrypoints=https'
          - 'traefik.http.routers.authelia.tls=true'
          - 'traefik.http.routers.authelia.tls.certresolver=letsencrypt'
          - 'traefik.http.middlewares.authelia.forwardauth.address=http://authelia:9091/api/verify?rd=https://authelia.example.com'  # yamllint disable-line rule:line-length
          - 'traefik.http.middlewares.authelia.forwardauth.trustForwardHeader=true'
          - 'traefik.http.middlewares.authelia.forwardauth.authResponseHeaders=Remote-User,Remote-Groups,Remote-Name,Remote-Email'  # yamllint disable-line rule:line-length
        expose:
          - 9091
        restart: unless-stopped
        healthcheck:
          disable: true
        environment:
          - TZ=Australia/Melbourne

      redis:
        image: redis:alpine
        container_name: redis
        volumes:
          - ./redis:/data
        networks:
          - net
        expose:
          - 6379
        restart: unless-stopped
        environment:
          - TZ=Australia/Melbourne

      traefik:
        image: traefik:v2.8
        container_name: traefik
        volumes:
          - ./traefik:/etc/traefik
          - /var/run/docker.sock:/var/run/docker.sock
        networks:
          - net
        labels:
          - 'traefik.enable=true'
          - 'traefik.http.routers.api.rule=Host(`traefik.example.com`)'
          - 'traefik.http.routers.api.entrypoints=https'
          - 'traefik.http.routers.api.service=api@internal'
          - 'traefik.http.routers.api.tls=true'
          - 'traefik.http.routers.api.tls.certresolver=letsencrypt'
          - 'traefik.http.routers.api.middlewares=authelia@docker'
        ports:
          - 80:80
          - 443:443
        command:
          - '--api'
          - '--providers.docker=true'
          - '--providers.docker.exposedByDefault=false'
          - '--entrypoints.http=true'
          - '--entrypoints.http.address=:80'
          - '--entrypoints.http.http.redirections.entrypoint.to=https'
          - '--entrypoints.http.http.redirections.entrypoint.scheme=https'
          - '--entrypoints.https=true'
          - '--entrypoints.https.address=:443'
          - '--certificatesResolvers.letsencrypt.acme.email=your-email@your-domain.com'
          - '--certificatesResolvers.letsencrypt.acme.storage=/etc/traefik/acme.json'
          - '--certificatesResolvers.letsencrypt.acme.httpChallenge.entryPoint=http'
          - '--log=true'
          - '--log.level=DEBUG'

      secure:
        image: traefik/whoami
        container_name: secure
        networks:
          - net
        labels:
          - 'traefik.enable=true'
          - 'traefik.http.routers.secure.rule=Host(`secure.example.com`)'
          - 'traefik.http.routers.secure.entrypoints=https'
          - 'traefik.http.routers.secure.tls=true'
          - 'traefik.http.routers.secure.tls.certresolver=letsencrypt'
          - 'traefik.http.routers.secure.middlewares=authelia@docker'
        expose:
          - 80
        restart: unless-stopped

      public:
        image: traefik/whoami
        container_name: public
        networks:
          - net
        labels:
          - 'traefik.enable=true'
          - 'traefik.http.routers.public.rule=Host(`public.example.com`)'
          - 'traefik.http.routers.public.entrypoints=https'
          - 'traefik.http.routers.public.tls=true'
          - 'traefik.http.routers.public.tls.certresolver=letsencrypt'
          - 'traefik.http.routers.public.middlewares=authelia@docker'
        expose:
          - 80
        restart: unless-stop




๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต๐ต config swarn first'

vps +  local doker. 




๐ต traefik + authelia 


https://github.com/authelia/authelia/blob/master/examples/compose/lite/docker-compose.yml




https://wbsu2003.4everland.app/2022/03/09/%E5%8D%95%E7%82%B9%E7%99%BB%E5%BD%95%E6%9C%8D%E5%8A%A1Authelia%EF%BC%88%E4%B8%8B%E7%AF%87%EF%BC%89/



ๆไบ Authelia๏ผๅ้ๅ Fail2ban ้ฒๆญขๆดๅ็?ด่งฃ๏ผๅฌ็ฝ่ฎฟ้ฎ็ๅฎๅจๆง้ฎ้ขไผๅพๅฐๅพๅคง็ไฟ้ใ

่ฝ็ถๅฎๆนๅผบ่ฐ OpenID Connect ไปๅคไบ้ข่ง้ถๆฎต๏ผไฝๅฎ้ไธAuthelia ๅทฒ็ปๆฏๆ OIDC ่ฎค่ฏ๏ผไธ่ฟ้ไบ็ฏๅน๏ผ่ฟๆฏ็ๅฐไธๅๅงใ

ไธๆ้ขๅ๐ใOutlineไฝฟ็จAutheliaๅฎ็ฐๆฌๅฐ่ฎค่ฏใ๏ผๆ็ซ?ๅฐ่ฎจ่ฎบๅฆไฝๅฎ็ฐ Outline ้่ฟ Authelia ็ OIDC ๅฎๆๆฌๅฐ่ฎค่ฏ๏ผ่ไธๅ้่ฆๅๅฉๅบไบๅฌ็ฝ็็ฌฌไธๆน่ฎค่ฏ



Outlineไฝฟ็จAutheliaๅฎ็ฐๆฌๅฐ่ฎค่ฏ


https://wbsu2003.4everland.app/2022/03/11/Outline%E4%BD%BF%E7%94%A8Authelia%E5%AE%9E%E7%8E%B0%E6%9C%AC%E5%9C%B0%E8%AE%A4%E8%AF%81/









https://www.youtube.com/watch?v=yHICIVhm_Lo&t=621s&ab_channel=Sagit