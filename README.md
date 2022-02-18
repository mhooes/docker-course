# Devops with Docker

## 1.1: Getting started
![alt text](https://github.com/Mhooes/devops-with-docker/blob/adc093244669ca0cdcf5dab966865b2c107509ac/part-1/1.1/Exercise%201-1.png)

## 1.2: Cleanup
![alt text](https://github.com/Mhooes/devops-with-docker/blob/adc093244669ca0cdcf5dab966865b2c107509ac/part-1/1.2/Exercise%201-2.png)

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
![alt text](https://github.com/Mhooes/devops-with-docker/blob/cdc44832b5a469d5c6bd14c7a1f6ff2b177e809b/part-1/1.5/Exercise%201-5.png)
![alt text](https://github.com/Mhooes/devops-with-docker/blob/cdc44832b5a469d5c6bd14c7a1f6ff2b177e809b/part-1/1.5/Exercise%201-5-1.png)

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
CMD ./  input.sh
```

## 1.9: Volumes
**Create file for logs on host**
```
touch logs
```
```
sudo docker run -v "$(pwd)"/logs:/usr/src/app/text.log devopsdockeruh/simple-web-service
```
