# hackathoncontainers

## advisor container

### download project
git clone https://github.com/gujou/advisorcontainer.git

### build project
mvn clean package

### build docker image
sudo docker build . --tag advisor:[version]

### run container
sudo docker run -it --rm -p [connection_port]:8080 --name [container_name] advisor:[version]

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

### login docker repository hackathon-container.com
sudo docker login registry.hackathon-container.com

### push image to docker repository
sudo docker push registry.hackathon-container.com/packapp01/customer:[version]


