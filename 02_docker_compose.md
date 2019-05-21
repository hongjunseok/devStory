## Docker-Compose


#### 각 프로젝트에 Dockerfile 생성 
```
$ touch Dockerfile
```


#### 각 프로젝트에 docker-compose.yml 생성 
```
$ touch docker-compose.yml
```

#### Nginx Proxy + Node

```
version: '3.6'
services:
  front:
    image: [ECR URL or Docker Hub URL]
    restart: always
    environment:
      - VIRTUAL_HOST= [도메인 or Host]
      - VIRTUAL_PORT=3000
      - NODE_ENV=development
      - LETSENCRYPT_HOST=[도메인 or Host]
      - LETSENCRYPT_EMAIL=steven@wepla.net
    networks:
      - nginx-proxy

  nginx-proxy:
    image: jwilder/nginx-proxy
    ports:
      - 80:80
      - 443:443
    restart: always
    volumes:
      - /nginx/certs:/etc/nginx/certs
      - /nginx/vhost.d:/etc/nginx/vhost.d
      - /nginx/html:/usr/share/nginx/html
      - /var/run/docker.sock:/tmp/docker.sock:ro
    labels:
      - com.github.jrcs.letsencrypt_nginx_proxy_companion.nginx_proxy
    networks:
      - nginx-proxy

  letsencrypt-nginx-proxy:
    image: jrcs/letsencrypt-nginx-proxy-companion
    restart: always
    volumes:
      - /nginx/certs:/etc/nginx/certs
      - /nginx/vhost.d:/etc/nginx/vhost.d
      - /nginx/html:/usr/share/nginx/html
      - /var/run/docker.sock:/var/run/docker.sock:ro
    networks:
      - nginx-proxy

networks:
  nginx-proxy:
    name: nginx-proxy


```

#### Deploy.sh 작성
```
#!/bin/bash
AWS_PROFILE=$1
IMAGE_NAME=ck-friends-front
REGISTRY_URL=[REGISTRY_URL]:latest
HOST=[remote host]
PEM_FILE=~/.ssh/[인증서].pem
APP_PATH=/home/ubuntu/${IMAGE_NAME}
DOCKER_COMPOSE=docker-compose.yml

set -e

function printUsage() {
    echo "Usage: deploy.sh {profileName}"
    echo ""
}

function errorCheck() {
    if [[ $? -ne 0 ]]; then
    exit 1
fi
}

if [[ -z ${AWS_PROFILE} ]]; then
  printUsage
  exit 1
fi

docker build -t ${IMAGE_NAME} .
errorCheck

docker tag ${IMAGE_NAME}:latest ${REGISTRY_URL}

errorCheck


ECR_LOGIN=$(aws ecr get-login --no-include-email --region ap-northeast-2 --profile ${AWS_PROFILE})
eval ${ECR_LOGIN}

docker push ${REGISTRY_URL}
errorCheck

ssh ${HOST} -i ${PEM_FILE} "sudo "${ECR_LOGIN}" && sudo docker pull "${REGISTRY_URL}""
errorCheck

ssh ${HOST} -i ${PEM_FILE} "sudo mkdir -p ${APP_PATH}"
errorCheck
scp -i ${PEM_FILE} -d ./docker-compose.yml ${HOST}:${APP_PATH}
errorCheck
ssh ${HOST} -i ${PEM_FILE} "sudo docker-compose -f "${APP_PATH}"/docker-compose.yml -p ${IMAGE_NAME} up -d --remove-orphans"
errorCheck

echo 'start ecr get-login'
ssh ${HOST} -i ${PEM_FILE} "sudo docker image prune -f"

echo 'end ecr get-login'

docker image prune -f

echo "Deploy Success!"

```

#### 실행
```
$ bash script/deploy.sh
```
