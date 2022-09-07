# Devops with Docker

## 1.1: Getting started
![alt text](https://github.com/Mhooes/docker-course/blob/917ba89bcc3ad7ea27993efbb2591b1848e84f10/part-1/1.1/Exercise%201-1.png)

## 1.2: Cleanup
![alt text](https://github.com/Mhooes/docker-course/blob/917ba89bcc3ad7ea27993efbb2591b1848e84f10/part-1/1.2/Exercise%201-2.png)

## 1.3: Secret message

```sh
Secret message is: 'You can find the source code here: https://github.com/docker-hy'

1) sudo docker run -d --name simple devopsdockeruh/simple-web-service:ubuntu
2) sudo docker exec simple tail -f ./text.log
```
## 1.4: Missing dependencies
```
1)sudo docker run -d -it --name web ubuntu
2)sudo docker exec web apt update && sudo docker exec web apt install -y curl
3)sudo docker exec -it web sh -c 'echo "Input website:"; read website; echo "Searching.."; sleep 1; curl http://$website;'
4)helsinki.fi
```

## 1.5: Sizes of images
![alt text](https://github.com/Mhooes/docker-course/blob/917ba89bcc3ad7ea27993efbb2591b1848e84f10/part-1/1.5/Exercise%201-5.png)
![alt text](https://github.com/Mhooes/docker-course/blob/917ba89bcc3ad7ea27993efbb2591b1848e84f10/part-1/1.5/Exercise%201-5-1.png)

## 1.6: Hello Docker Hub
**Password:**
```
basics
```
**Message:**
```
Give me the password: basics
You found the correct password. Secret message is:
"This is the secret message"
```

## 1.7: Two line Dockerfile
```Dockerfile
####Exercise 1.7
### Start from the alpine image
FROM devopsdockeruh/simple-web-service:alpine
##
### When running docker run the command
CMD server
```

## Image for script
**Script (input.sh):**
```sh
#!/bin/bash

echo "Input website:"; read website; echo "Searching.."; sleep 1; curl http://$website;
```
**Dockerfile:**
```Dockerfile
####Exercise 1.8
FROM ubuntu:20.04
RUN apt update && apt install -y curl
WORKDIR /usr/src/app
COPY input.sh .
CMD ./input.sh
```

## 1.9: Volumes
**Create file for logs on host**
```
touch logs
```
```
sudo docker run -v "$(pwd)"/logs:/usr/src/app/text.log devopsdockeruh/simple-web-service
```

## 1.10: Ports open
```
sudo docker run -p 127.0.0.1:80:8080 devopsdockeruh/simple-web-service server
```

## 1.11: Spring
```Dockerfile
FROM openjdk:9-b130-jdk
WORKDIR /usr/src/app
EXPOSE 8080
COPY . .
RUN ./mvnw package
CMD ["java", "-jar", "./target/docker-example-1.1.3.jar"]
```

## 1.12: Hello, frontend!
**Dockerfile:**
```Dockerfile
FROM ubuntu
WORKDIR /usr/src/app
COPY . .
RUN apt update && apt install -y curl && curl -sL https://deb.nodesource.com/setup_16.x | bash
RUN apt install -y nodejs
RUN npm install && npm install -g serve
RUN npm run build
CMD ["serve", "-s", "-l", "5000", "build"]
EXPOSE 5000
```

## 1.13: Hello, backend!
**Dockerfile:**
```Dockerfile
FROM ubuntu
WORKDIR /usr/src/app
COPY . .
ENV PATH /usr/local/go/bin:$PATH
RUN apt update && apt install -y wget gcc && wget https://dl.google.com/go/go1.17.7.linux-amd64.tar.gz && tar -xvf go1.17.7.linux-amd64.tar.gz && mv go /usr/local 
RUN go build 
RUN go test
CMD ./server
EXPOSE 8080
```
**Command:**
```bash
 sudo docker build . -t backend && sudo docker run -d -p 127.0.0.1:8080:8080 backend
 ```
 
 ## 1.14: Environment
**Dockerfile frontend:**
```Dockerfile
FROM ubuntu
WORKDIR /usr/src/app
COPY . .
ENV REACT_APP_BACKEND_URL=http://localhost:8080
RUN apt update && apt install -y curl && curl -sL https://deb.nodesource.com/setup_16.x | bash
RUN apt install -y nodejs
RUN npm install && npm install -g serve
RUN npm run build
CMD ["serve", "-s", "-l", "5000", "build"]
EXPOSE 5000
```
**Dockerfile Backend:**
```Dockerfile
FROM ubuntu
WORKDIR /usr/src/app
COPY . .
ENV PATH /usr/local/go/bin:$PATH
ENV REQUEST_ORIGIN=http://localhost:5000
RUN apt update && apt install -y wget gcc && wget https://dl.google.com/go/go1.17.7.linux-amd64.tar.gz && tar -xvf go1.17.7.linux-amd64.tar.gz && mv go /usr/local 
RUN go build 
RUN go test
CMD ./server
EXPOSE 8080
```
**Commands:**
```bash
sudo docker build . -t frontend && sudo docker run -d -p 127.0.0.1:5000:5000 frontend 
sudo docker build . -t backend && sudo docker run -d -p 127.0.0.1:8080:8080 backend
```


 
