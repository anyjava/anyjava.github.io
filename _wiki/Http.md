---
layout  : wiki
title   : Http
summary : Http 에 대한 정보를 정리
date    : 2019-05-20 01:49:12 +0900
updated : 2019-05-20 02:13:27 +0900
tags    : http
toc     : true
public  : true
parent  : Web
latex   : false
adsense : true
---
* TOC
{:toc}

## HTTP 연결 이해를 돕기 위한 내용들

* 성능 관련 중요 요소
	* TCP 커텍션의 핸드세이크 설정
	* 인터넷의 혼자을 제어하기 위한 TCP의 느린시작(slow-start)
	* 데이터를 한데 모아 한 번에 전송하기 위한 네이블(nagle) 알고리즘
	* TCP 의 편승(piggyback) 확인응답(acknowledgment)을 위한 확인응답 지연 알고리즘
	* TIME_WAIT 지연과 포트 고갈

### TCP Connection HandShake

* 3 WAY Handshake	
	* Client 는 `SYN` 라는 작은 패킷을 서버에게 보낸다.
	* 서버는 그에 대한 응답으로 `SYN` + `ACK` 패킷을 보낸다.
	* Client 는 최종적으로 연결되었음을 알리는 `ACK` 를 보내는데 이때 데이터를 같이 보낼수도 있다.


### Keep-Alive

* 해당 optioan 을 끈다면, client 에서 매번 연결하는 연결 비용이 발생해서 성능에 좋지 않는 영향을 미칠 수 있다.
* 보통은 키고 있는데 성능에 좋다.
* API 서버의 경우, 보통 출발지가 고정되어 있기 때문에 설정하는게 초고 timeout 설정은 10s 정도가 적당함


### 참고자료

* [CLOSE_WAIT & TIME_WAIT 최종 분석](http://tech.kakao.com/2016/04/21/closewait-timewait/)
* [HTTP 완벽 가이드](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9788966261208&orderClick=LEA&Kc=)
* [권남님의 wiki - web performance][http://kwonnam.pe.kr/wiki/web/performance]
