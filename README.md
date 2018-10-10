# hackathoncontainers

## project architecture
<img src="https://github.com/gujou/hackathoncontainers/blob/master/images/archi.png"
     alt="Architecture overview"
     width="70%" />

## projet url
https://monapp01.hackathon-container.com/containerbank/

## advisor container

### download project
git clone https://github.com/gujou/advisorcontainer.git

### build project
mvn clean package

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
mvn clean package

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
