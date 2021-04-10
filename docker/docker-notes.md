## Building Docker Image from Scratch

```
FROM alpine:3.11
RUN apk add openjdk8
ENV TOMCAT_MAJOR=8 \
    TOMCAT_VERSION=8.5.3 \
    TOMCAT_HOME=/opt/tomcat \
    CATALINA_HOME=/opt/tomcat \
    CATALINA_OUT=/dev/null
RUN apk upgrade --update && \
    apk add --update curl && \
    curl -jksSL -o /tmp/apache-tomcat.tar.gz http://archive.apache.org/dist/tomcat/tomcat-${TOMCAT_MAJOR}/v${TOMCAT_VERSION}/bin/apache-tomcat-${TOMCAT_VERSION}.tar.gz && \
    gunzip /tmp/apache-tomcat.tar.gz && \
    tar -C /opt -xf /tmp/apache-tomcat.tar && \
    ln -s /opt/apache-tomcat-${TOMCAT_VERSION} ${TOMCAT_HOME} && \
    rm -rf ${TOMCAT_HOME}/webapps/* && \
    apk del curl && \
    rm -rf /tmp/* /var/cache/apk/*
COPY target/spring-h2-demo.war /opt/tomcat/webapps
ENTRYPOINT ["/opt/tomcat/bin/catalina.sh", "run"]
```
This file install Java + Tomcat + Alpine (Linux Distribution)

### Building Docker Image from existing templates

For existing Docker images, refer the [Docker Hub Official Website](https://hub.docker.com/)

```
FROM tomcat:jdk8-openjdk-slim
COPY /target/spring-h2-demo.war /usr/local/tomcat/webapps
ENTRYPOINT ["/usr/local/tomcat/catalina.sh", "run"]
```

### Command Syntax
```
// Build Docker Image
docker build -t <IMAGE_NAME> .

// Run Docker Image aka Container
docker run -p <HOST_PORT>:<DOCKER_IMAGE_OS_PORT> <DOCKER_IMAGE_NAME>

// Get Logs of Docker Container
docker logs <CONTAINER_ID>

// Get running container details
docker ps

// Get all container details (including stopped ones)
docker ps -a

// Get id of all stopped conatiners
docker ps -aq

// Stop running container
docker stop <CONTAINER_ID>

// Remove container forcibly
docker rm -f <CONTAINER_ID>

// Remove all containr ids return by docker ps -aq command
docker rm -f $(docker ps -aq)

// Remove Docker image
docker image rm -f <IMAGE_ID>

//copy file current to host
docker cp <path of src> <containerID>:<path of the server>
```

### Command Samples

```sh
docker build -t mari-artifact .

docker run -p 8080:8080 mari-artifact

docker run -p 8080:8080 -d mari-artifact 
```



Docker basic Commands

1.Pulling image from docker hub:

--> docker pull <ImageName>

2.Create a Custom Image from Dockerfile

--> docker build -t <ImageName(U can give any name)> [Path of the Dockerfile]

3.Create and run a Container from image

--> docker run -p 8080:<PortNo of that image> <ContainerID>

4.Check what are the images we have

--> docker images

5.Check what are the container we have

--> docker ps

6.Check what are the container we have (inactive container)
 
--> docker ps -a

7.Stop container

--> docker stop <ContainerID>

8.Delete container

--> docker rm -f <containerID>

9. Delete Image

--> docker rm -f <ImageID> (or) docker rmi <ImageID>

Delete image only the container of that image is not in active

10. Check version of Docker

--> docker --version

11. Copy file from local to host directory

--> docker cp [path of local file] <ContainerID>:[Path location of that Container]
