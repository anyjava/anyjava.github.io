---
layout  : wiki
title   : Docker
summary : Docker 에 대한 모든것 
date    : 2018-09-06 02:01:37 +0900
updated : 2018-09-06 02:07:10 +0900
tags    : docker
toc     : true
public  : true
parent  : DevOps
latex   : false
---
* TOC
{:toc}

# Docker

## Docker 주요 명령어 정리

* `docker run` 은 image 파일로 부터 container 를 생성하는 작업을 한다.
* `docker start` 는 container 를 시작하는 기능
* `docker -it exec imageId /bin/bash` docker 의 shell 을 수행함, -i interactive mode -t tty 
* `docker ps -a` run 되고 있지 않은 모든 container 를 보여줌
* `docker images ls` docker image 조회
* `docker rmi imageId` docker image 삭제
* `docker rm containerid` docker container 삭제
* `docker logs -f` docker log 조회 `-f`는 `tail -f` 와 동일함
