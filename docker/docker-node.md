# Node js in Docker


**create a Dockerfile in your react js project**
```sh
npx create-react-app sampleapp
```
 After Creating the project create the Dockerfile
 
 Dockerfile
 
 ```
FROM node:12.21.0-alpine3.11
WORKDIR /app
COPY package*.json ./
RUN npm install
RUN npm prune --production
COPY . .
ENTRYPOINT ["npm","start"]
```

i install node version 12.21.0 with alpine Linux OS version 3.11 **FROM node:12.21.0-alpine3.11**

then inside the OS we create a /app directory then perform all operation inside the /app directory **WORDIR /app**

then copy all json package **COPY package*.json ./** put it in a current directory

then run the npm install

i use npm prune --production version why because which use only important file and remove the unnessassary files and reduce size

then copy all file usine **COPY . .** command

i give the entrypoint as npm start **ENTRYPOINT ["npm","start"]**

### Execute the docker image and run the container


```sh
Build Docker image

docker build -t <Image-NAME> .

image is created using image create and run the container

docker run -p 8080:3000 <Image-Name>

```

docker allocate the port to run our npm 

**http://localhost:8080/**

[Reference website](https://itsopensource.com/how-to-reduce-node-docker-image-size-by-ten-times/#:~:text=In%20general%2C%20the%20node%20docker,GB%20most%20of%20the%20time.)
