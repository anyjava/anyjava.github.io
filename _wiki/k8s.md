---
layout  : wiki
title   : k8s
summary : Kubernetes
date    : 2020-02-18 14:45:23 +0900
updated : 2023-01-19 11:08:01 +0900
tags    : k8s, kubernetes
toc     : true
public  : true
parent  : Infra
latex   : false
adsense : true
---
* TOC
{:toc}

## k8s 에서 JVM 메모리 설정

* Pod 의 Request / Limit 은 동일하게 설정한다.
  * jvm 의 heap memory 는 `-XX:InitialRAMPercentage=50.0 -XX:MaxRAMPercentage=50.0`로 설정하자.
  * 이유는 Pod 에서 jvm 이 사용하는 부분도 필요하지만, OS가 사용하는 file 캐싱 영역도 필요하기때문에 50% 설정하는게 적당.
	* 자세한건 다음글 참고 -> [컨테이너 환경에서의 java 애플리케이션의 리소스와 메모리 설정](https://findstar.pe.kr/2022/07/10/java-application-memory-size-on-container/)

## Ingress

* [Kubernetes Ingress Controllers](https://docs.google.com/spreadsheets/d/191WWNpjJ2za6-nbG4ZoUMXMpUK8KlCIosvQB0f-oq3k/edit#gid=907731238) 
  * k8s igress type 별로 기능들을 비교해둔 표

* [Probe default value](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/#configure-probes)


## 참고자료 

* [카카오톡 적용 사례를 통해 살펴보는 카카오 클라우드의 Kubernetes as a Service](https://if.kakao.com/2019/program?sessionId=eebbe5ae-0c77-4f52-83af-5818f9fd6c26)
* [Kubernetes deployment strategies](https://github.com/ContainerSolutions/k8s-deployment-strategies)
* [blue-green-deploy](https://codefresh.io/kubernetes-tutorial/blue-green-deploy/) 
* [Container부터 다시 살펴보는 Kubernetes Pod 동작 원리](https://hyojun.me/~k8s-pod-internal) 
* [Kubernetes Failure Stories](https://k8s.af/)
