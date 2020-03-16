---
layout  : wiki
title   : Rabbit MQ
summary : Rabbit MQ
date    : 2020-03-16 19:12:33 +0900
updated : 2020-03-16 22:53:55 +0900
tags    : mq, message, queue, mq, rabbit, amqp
toc     : true
public  : true
parent  : Infra
latex   : false
adsense : true
---
* TOC
{:toc}

## Rabbit MQ 사용하기 

* Rabbit MQ 의 특성

  * code level 에서 Queue exchange 들을 선언해서 관리하는게 좋다.
  * RabbitMQ 관리자는 사용하지 않는 Queue 는 자동삭제 되게 하는걸 추천함.


* Consuemr ACK 정보

  * NACK -> 줄 경우 dead letter queue 로 들어감. dead letter queue 가 없다면 requeue 됨
  * ACK -> 정상 처리 되면서 queque 에서 삭제

  *  queue 설정에 autoAck 라는 설정이 있는데, 가져가면 바로 메시지가 삭제 됨

```
    spring amqp 에서의 설정
        org.springframework.amqp.core.AcknowledgeMode 참고
            NONE : autoack true 입니다.
            MANUAL : 코드에서 직접 명시적으로 ack 함
            AUTO : exception 이 발생한다면 NACK 함 (default)
```

* Spring Boot amqp 의 default ack 전략

  * org.springframework.amqp.rabbit.listener.ConditionalRejectingErrorHandler 에 따름
  * [공식문서의 exception-handling](https://docs.spring.io/spring-amqp/reference/html/#exception-handling) 참고



## 참고링크

* [RabbitMQ 이해하기](https://github.com/gjchoi/gjchoi.github.io/blob/master/_posts/2016-02-27-rabbit-mq-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0.md)
* [RabbitMQ 에 대한 이해](https://nesoy.github.io/articles/2019-02/RabbitMQ)
* [RabbitMQ Publisher Confirm - 메시지 전송도 안 잃어버리기](https://blog.leocat.kr/notes/2018/06/23/rabbitmq-publisher-confirm)
* [nack/reject로 다른 큐로 보내기 (dead-letter)](https://blog.leocat.kr/notes/2018/06/20/rabbitmq-dead-lettering-with-reject-or-nack) 
* [spring amqp example](https://github.com/michaellihs/spring-amqp-by-example/blob/master/src/test/java/ch/lihsmi/spring/amqp/byexample/connections/PublisherConfirmTest.java)
* [RabbitMQ getStarted](https://www.rabbitmq.com/getstarted.html)
* [13 Common RabbitMQ Mistakes and How to Avoid Them](https://www.cloudamqp.com/blog/2018-01-19-part4-rabbitmq-13-common-errors.html)
* [10 Things Every Developer Using RabbitMQ Should Know](https://www.brighttalk.com/webcast/14893/340552?autoclick=true&utm_source=brighttalk-recommend&utm_campaign=network_weekly_email&utm_medium=email&utm_content=collab&utm_term=042019)
