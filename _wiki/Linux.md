---
layout  : wiki
title   : Linux 
summary : linux 활용에 관한 모든것
date    : 2018-08-21 18:28:36 +0900
updated : 2019-12-26 14:42:13 +0900
tags    : linux
toc     : true
public  : true
parent  : DevOps
latex   : false
adsense : true
---

## Linux 활용

### Linux Kernal Parameter

* 아래 설정에 하나하나 알아갈때마다 의미를 채워보자
 
```
vm.swappiness = 0	# swap memory 를 이용하지 않음. 
 
net.core.somaxconn = 1000
net.core.netdev_max_backlog = 5000
net.core.rmem_max = 16777216
net.core.wmem_max = 16777216
 
net.ipv4.tcp_wmem = 4096 12582912 16777216
net.ipv4.tcp_rmem = 4096 12582912 16777216
net.ipv4.tcp_max_syn_backlog = 8096
net.ipv4.tcp_slow_start_after_idle = 0
net.ipv4.tcp_tw_reuse = 1 # TIME_WAIT local socket 재사용
net.ipv4.ip_local_port_range = 10240 65535 # local port 범위 조정
```

* 주의사항
	* swap 메모리를 사용하지 않는다면, applicaiton 이 시스템 메모리를 80% 이상 점유하지 않도록 하자.
		* [[번역] Linux에서 메모리를 다 써버렸을 때 일어나는 일](https://medium.com/@EJSohn/%EB%B2%88%EC%97%AD-linux%EC%97%90%EC%84%9C-%EB%A9%94%EB%AA%A8%EB%A6%AC%EB%A5%BC-%EB%8B%A4-%EC%8D%A8%EB%B2%84%EB%A0%B8%EC%9D%84-%EB%95%8C-%EC%9D%BC%EC%96%B4%EB%82%98%EB%8A%94-%EC%9D%BC-9dadba29c89c?fbclid=IwAR1RSqZZReZdsexmbhRvf_bjMqyP8Sz-z7NhOd0Q9cXGqo1zvQf6Bf99mAY) 

### 자주쓰는 문법


```
grep -rn "찾고자 하는 문자열" *
grep -A라인수 -B 라인수 # A: 뒤 몇라인, B: 앞 몇라인
```

* while 문으로 주기적인 실행

```
while true ; do echo -n "$(date) " ; ss -s | grep timewait ; sleep 1 ; done
```

* sed
  * [sed 명령어](https://m.blog.naver.com/PostView.nhn?blogId=minki0127&logNo=220677180665&proxyReferer=https%3A%2F%2Fwww.google.com%2F)

```bash
$ sed '$d' fileName  # file의 마지막 라인 삭제
$ sed '1d' fileName  # file의 첫번째 라인만 삭제
$ sed '1,3d' fileName  # file의 1~3번째 라인만 삭제
```

### 시스템 모니터링

* top
  * shift + M: 메모리 사용량별, / P: CPU 사용량별
  * f 눌러서 추가로 보여질 필드를 고를 수 있다.

* free -g: 메모리 사용량을 G 단위로 보여줌 

### Linux Utils

* logrotate : log rolling 설정을 해주는 linux util
	* [logroate 설정방법](https://www.manualfactory.net/10547)
* supervisor : process 관리 해주는 도구 
	* [참고](https://jwkcp.github.io/2016/11/07/how-to-use-supervisor-in-one-minute/)

