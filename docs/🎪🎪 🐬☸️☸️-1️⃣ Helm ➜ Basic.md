---
sidebar_position: 2930
title: πͺπͺπ¬βΈοΈβΈοΈ1οΈβ£ Helm β Basic
---

# Helm Basic


    https://www.youtube.com/watch?v=6mHgb3cDOjU&list=PLmOn9nNkQxJHYUm2zkuf9_7XJJT8kzAph&index=45&ab_channel=%E5%B0%9A%E7%A1%85%E8%B0%B7IT%E5%9F%B9%E8%AE%AD%E5%AD%A6%E6%A0%A1


π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅ Hlem Install 

π΅ K8s tool

    πΆ K8s APP-GUI  β lens  β manager local+remote cluster.  β best 
    πΆ K8s CMD-GUI  β k9s   β manager local cluster.
    πΆ K8s package  β helm  β like apt.  make k8s deploy pod very easy.


        π» lens enable prometheus β

            lens have builded in promethes
            but you need able it for all cluster.

            cluster >> setting >> lens metric >> enable all >> restart app 


π΅ Helm Desc 

    π manual   https://helm.sh/zh/docs/topics/charts/
    π manual   https://helm.sh/zh/docs/topics/charts/
    π manual   https://helm.sh/zh/docs/topics/charts/


    helm prepair almost all for you. 
        but helm is temple! 
        temple means you have to change some config before use it .

        π₯ helm-V3 is for local cluster. not for remote manage.
            - k8s manager node:      install helm 
            - k3s maneger node:      install helm 
            - minikube    node:      install helm 


π΅ Helm basic 

    helm    β mamage app 
    chart   β app         (exe dmg )
    repo    β app storage (where to doanload app) 
    release β app vision  (like git version )


π΅ Helm Use

    install helm to cluster maneger node.

    add helm app repo  then install helm app

    config app & run 


π΅ Hlem install

    πΆ mac 

        brew install helm
        helm repo add bitnami https://charts.bitnami.com/bitnami
        helm repo update


    πΆ Ubuntu - apt 

        curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
        sudo apt-get install apt-transport-https --yes
        echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
        sudo apt-get update
        sudo apt-get install helm


π΅ Helm search app 

    πΆ use cmd    π

        helm search hub  xxx   β  search in allavailable    repo 
        helm search repo xxx   β  search in installed ready repo 


    πΆ use website π

        https://artifacthub.io/packages/search?kind=0&sort=relevance&page=1
            website. tell you how install & config 




π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅ Helm app Strecture 

π΅ app importent files β

    chart.yaml        β  app info:          β app name / version / url / maintainer email 
    requirements.yaml β  app depends:       β if need mysql etc. 
    value.yaml        β  app default value  β docker port / username / 


π΅ global value  

    app is dependet. like docker. 
    but some time you need other app can visit something in out app.
    so you can use global value for that you need share out.









π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅π΅


π΅ available vaule - check how β

    app only allow you change some value. 
    depends on how the helm app is created.

    every app is different available value. 
    you have to check in helm app`s website.


        π» use web π 
            https://artifacthub.io/packages/helm/bitnami/nginx
            https://artifacthub.io/packages/helm/bitnami/nginx?modal=values

                you can check all info in web. no need cmd. 


        π» use cmd π
            -- use cmd must add repo first. then check 

            helm repo add bitnami https://charts.bitnami.com/bitnami
            helm show values bitnami/nginx

            helm repo add bitnami https://charts.bitnami.com/bitnami
            helm show values bitnami/mariadb



π΅ available vaule - check demo 

    https://artifacthub.io/packages/helm/bitnami/nginx

        Traffic Exposure parameters
        service.ports.http	80



π΅ custom value 

    two way to set your custom value.   
        -f    β use yaml file 
        --set β use cmd 

    πΆ set demo 

        helm install my-nginx bitnami/nginx --set xxx=yyy
        helm install my-nginx bitnami/nginx --set service.ports.http=880
        helm install my-nginx bitnami/nginx --set service.ports.http=880,service.ports.https=8443


    πΆ yaml demo 

        helm install my-nginx bitnami/nginx -f xxx.yaml 
        helm install my-nginx bitnami/nginx -f config-nginx.yaml 


            cat <<EOF>  config-nginx.yaml
            service.ports.http: 880,
            service.ports.https: 8443,
            EOF

            http:172.16.1.33:880
            https:172.16.1.33:8443







