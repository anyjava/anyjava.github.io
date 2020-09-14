---
layout  : wiki
title   : MSA
summary : Microservice Architecture
date    : 2019-10-21 15:30:38 +0900
updated : 2020-09-15 03:04:13 +0900
tags    : msa, architecture
toc     : true
public  : true
parent  : index
latex   : false
adsense : true
---
* TOC
{:toc}

# Microservice Architecture

## Armeria Framework

* 여러가지 remote 프로토콜을 지원하는 Framwork 이다. 비동기, Reactive 함.
* [Git Repo](https://github.com/line/armeria)
* [DEVIEW 2019 발표자료](https://deview.kr/data/deview/2019/presentation/[236]2019.10.%20Armeria%20-%20A%20Microservice%20Framework%20Well-suited%20Everywhere.pdf)

## MSA 대한 토론

* [DB 호출이 API 호출보다 빠른데 MSA 전환하는 이유](https://www.facebook.com/groups/TeAnE/permalink/1949109658558069/)
  * MSA는 ms 성능이 아주 중요하다면 올바른 선택지가 못될듯,
  * 개인의견
    * 비동기적으로 트랜잭션을 처리하고 최종적 일관성(Eventual consistency) 으로 여러 시스템이 데이터 소스간의 sync 를 맞추게 된다.
    * 경험상 최초 프로덕을 설계할때는 한 트랜잭션에서 모든 작업을 수행했지만, 많은 트랙픽을 받아내기위해서는 중요한 트랜잭션과 그렇지 않은 트랜잭션을 분리해서 적용해서 시스템을 확장해 나가고는 했다.
    * 읽기에 대한 성능은 DB 보다 고가용성은 NoSQL 등을 이용하요 Query Model을 구현하는 시스템을 설계하게 된다.
    * 위의 모든 행위는 대용량의 트래픽을 처리하기 위함이였으며, 그에 따른 장애 회피를 위한 아키텍쳐로 MSA 를 선택하게 되었었다.
  * 답변중 일부를 발췌함. 
```
애플리케이션에서 가장 비싼 부분은 I/O 쪽인데요, 그 중에서 DB I/O와 네트워크 I/O를 비교하자면 네트워크 I/O가 훨씬 더 비쌉니다. 그만큼 레이턴시가 크고 예측이 불가능하다는 점 때문에 그런 건데요,
그 성능이라는 관점을 레이턴시 수준에서 볼 것인가, 아니면 엔터프라이즈 레벨에서 볼 것인가를 먼저 결정해야 할 것 같습니다. 엔터프라이즈 레벨에서는 I/O 뿐만 아니라 고가용성, 비동기성 등 다른 요소들과 함께 성능을 측정합니다.
다른 말로, 조직내 구성원 모두가 "성능"이라는 표현의 정의를 공유하지 않으면 대규모 서비스들이 MSA를 도입함으로 갖는 성능과 내가 지금 DB 연결시 보여주는 성능과 같은 단어이지만 전혀 다른 의미로 받아들여지는 상황이 다가올 거예욥
``` 
  * [latency 기준](https://gist.github.com/jboner/2841832?fbclid=IwAR0i0XuhSNfLPgN2DhSPvqJspZldJKCsaaF0AVHl4SvK3WITCzI4ANZqk60)



## 참고링크

* [Delta: A Data Synchronization and Enrichment Platform](https://medium.com/netflix-techblog/delta-a-data-synchronization-and-enrichment-platform-e82c36a79aee)
  * MSA 환경에서 Data Sync 에 대한 이야기. Delta 가 open 될것 같은데 유심히 봐야할듯
