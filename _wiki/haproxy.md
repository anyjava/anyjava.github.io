---
layout  : wiki
title   : haproxy 
summary : haproxy
date    : 2020-06-18 00:52:42 +0900
updated : 2020-06-18 02:10:38 +0900
tags    : haproxy
toc     : true
public  : true
parent  : DevOps
latex   : false
adsense : true
---
* TOC
{:toc}

## Header 값으로 forwarding 할 backend 결정하기

```
frontend header_front
    bind *:80
    mode http
    option forwardfor if-none
    acl demo_host_version hdr(X-DEMO-HOST-VERSION) -i test
    use_backend test_backend if demo_host_version
    default_backend prod_backend

backend test_backend
    ...

backend prod_backend
    ...
```
 * [참고링크](https://stackoverflow.com/questions/58589331/haproxy-how-to-forward-traffic-to-backend-by-a-matching-header)

## 링크

* [Haproxy 설치해서 로드 밸런서로 활용하기](https://findstar.pe.kr/2018/07/27/install-haproxy/) 
* [L4/L7 스위치의 대안, 오픈 소스 로드 밸런서 HAProxy - naver D2](https://d2.naver.com/helloworld/284659)
* [Haproxy 의 Statistics 정보 모니터링 하기](https://findstar.pe.kr/2019/01/05/haproxy-stat-metric/)

