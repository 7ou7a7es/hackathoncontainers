# hackathoncontainers

## project architecture
<img src="https://github.com/gujou/hackathoncontainers/blob/master/images/archi_last.png"
     alt="Architecture overview"
     width="70%" />


## project target logic architecture
<img src="https://github.com/gujou/hackathoncontainers/blob/master/images/archi_logique_cible.png"
     alt="Architecture overview"
     width="70%" />
     
## project target deployment architecture
<img src="https://github.com/gujou/hackathoncontainers/blob/master/images/archi_deploy_cible.png"
     alt="Architecture overview"
     width="70%" /> 

## projet url
https://monapp01.hackathon-container.com/containerbank/

## advisor container

### download project
git clone https://github.com/gujou/advisorcontainer.git

### build project
mvn clean package -P=MySQL

### local test on tomcat
mvn tomcat7:run-war -P=MySQL

### build docker image
sudo docker build . --tag advisor:[version]

### run container
sudo docker run -it --rm -p [connection_port]:8080 --name [container_name] advisor:[version]

### tag docker image
sudo docker tag advisor:[version] registry.hackathon-container.com/packapp01/advisor:[version]

### login docker repository hackathon-container.com
sudo docker login registry.hackathon-container.com

### push image to docker repository
sudo docker push registry.hackathon-container.com/packapp01/advisor:[version]

## customer container

### download project
git clone https://github.com/gujou/customercontainer.git

### build project
mvn clean package -P=MySQL

### local test on tomcat
mvn tomcat7:run-war -P=MySQL

### build docker image
sudo docker build . --tag customer:[version]

### run container
sudo docker run -it --rm -p [connection_port]:8080 --name [container_name] customer:[version]

### tag docker image
sudo docker tag customer:[version] registry.hackathon-container.com/packapp01/customer:[version]

### login docker repository hackathon-container.com
sudo docker login registry.hackathon-container.com

### push image to docker repository
sudo docker push registry.hackathon-container.com/packapp01/customer:[version]

## Deploy to k8s

### ssh connection
ssh -i [hackathon_certificate_path]/hackathon-ec2-tp.pem ec2-user@ec2-52-210-144-207.eu-west-1.compute.amazonaws.com

### deploy customers pods & replicaset
kubectl create -f customer-deployment.yaml

### deploy advisors pods & replicaset
kubectl create -f advisor-deployment.yaml

### deploy customers service
kubectl create -f customer-service.yaml

### deploy advisors service
kubectl create -f advisor-service.yaml

### deploy ingress
kubectl create -f main-ingress.yaml

### control pods deployment
kubectl get pods

### see pod logs
kubectl logs [pod_id]

### connect bash pod to debug
kubectl exec -it [pod_id] -- /bin/bash

## k8s config


### Advisor : 
deployment  : Advisor-deployment.yaml  (replica 2 / port 8080) 
service : advisor-service.yaml (VIP cluster IP port 82 -> 8080)

### Custormer:
deployment : customer-deployment.yaml (replica 3/ port 8080)
service : customer-service.yaml    (cluster IP port 81 -> 8080)

### Ingress:
main-ingress.yaml
redirect / -> customer service
redirect /containerbank/advisor -> advisor service

### config files
https://github.com/gujou/hackathoncontainers/tree/master/k8s_config

## show logs customers services calls (logs end with time in ms)
https://kibana.hackathon-container.com/app/kibana#/discover?_g=()&_a=(columns:!(message),index:f3af7350-cd36-11e8-b77b-a7199e083105,interval:auto,query:(language:lucene,query:'packapp01%20%26%26%20customercontainer'),sort:!('@timestamp',desc))

## show logs customers services calls (logs end with time in ms && last 4 hours)
https://kibana.hackathon-container.com/app/kibana#/discover?_g=(refreshInterval:(pause:!t,value:0),time:(from:now-4h,mode:quick,to:now))&_a=(columns:!(message),index:f3af7350-cd36-11e8-b77b-a7199e083105,interval:auto,query:(language:lucene,query:'packapp01%20%26%26%20customercontainer'),sort:!('@timestamp',desc))
