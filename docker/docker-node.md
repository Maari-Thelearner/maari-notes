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

we install node version 12.21.0 with alpine Linux OS version 3.11 **FROM node:12.21.0-alpine3.11**

then inside the OS we create a /app directory then perform all operation inside the /app directory **WORDIR /app**

