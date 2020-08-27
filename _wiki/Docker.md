---
layout  : wiki
title   : Docker
summary : Docker 에 대한 모든것 
date    : 2018-09-06 02:01:37 +0900
updated : 2020-08-28 01:18:29 +0900
toc     : true
public  : true
parent  : DevOps
latex   : false
adsense : true
---
* TOC
{:toc}

# Docker

## Docker 주요 명령어 정리

* `docker run` 은 image 파일로 부터 container 를 생성하는 작업을 한다.
* `docker stop` container 를 중지하는 기능
* `docker start` 는 container 를 시작하는 기능
* `docker exec -it imageId /bin/bash` docker 의 shell 을 수행함, -i interactive mode -t tty 
* `docker ps -a` run 되고 있지 않은 모든 container 를 보여줌
* `docker images ls` docker image 조회
* `docker rmi imageId` docker image 삭제
* `docker rm containerid` docker container 삭제
* `docker logs -f` docker log 조회 `-f`는 `tail -f` 와 동일함


## Docker For Java/Spring

* `FROM:openjdk:8-alpine` : [Alpine Linux](https://alpinelinux.org/) 기반
	* Docker 용 리눅스로 곽광받고 있다. 특징 아주 작은 용량

## Docker For IntegrationTest

* MySQL
  * H2 DB 보다 실제 MySQL 을 띄워서 local test 를 하는 방향을 추천 한다.

```
docker run --name mysql57 \
    -p 3306:3306 \
    -e MYSQL_ROOT_PASSWORD=root \
    -e MYSQL_ROOT_HOST='%' \
    --restart=unless-stopped \
    -d \
    mysql/mysql-server:5.7 \
    --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
```

## 참고
* [개발자가 처음 Docker 접할때 오는 멘붕 몇가지](https://www.popit.kr/%EA%B0%9C%EB%B0%9C%EC%9E%90%EA%B0%80-%EC%B2%98%EC%9D%8C-docker-%EC%A0%91%ED%95%A0%EB%95%8C-%EC%98%A4%EB%8A%94-%EB%A9%98%EB%B6%95-%EB%AA%87%EA%B0%80%EC%A7%80/)
* [도커 베스트 프랙티스 (번역)](https://changhoi.github.io/posts/docker/Docker-best-practices/?fbclid=IwAR0FSzgUIa28tg2vVEYnHIrROAsiagnMrxAjatTODtfNtazMN9N_FHEMpx8)
* [Docker 가 왜 좋은지 5분안에 설명해줌](https://www.youtube.com/watch?v=chnCcGCTyBg)
