## Docker Command
**Example Dockerfile**
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

### Dockerfile with Tomcat
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
```

### Command Samples

```sh
docker build -t mari-artifact .

docker run -p 8080:8080 mari-artifact

docker run -p 8080:8080 -d mari-artifact 
```
