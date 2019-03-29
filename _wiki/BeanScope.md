---
layout  : wiki
title   : BeanScope
summary : Bean scope 에 대한 설명
date    : 2018-09-18 20:05:34 +0900
updated : 2019-03-29 23:57:50 +0900
tags    : spring, bean_scope
toc     : true
public  : true
parent  : Spring
latex   : false
adsense : true
---
* TOC
{:toc}

# Bean Scope

* Spring Bean 의 생성 주기를 설정할 수 있다.
	* org.springframework.beans.factory.config.ConfigurableBeanFactory.SCOPE_SINGLETON
	* org.springframework.beans.factory.config.ConfigurableBeanFactory.SCOPE_PROTOTYPE
	* SessionScope
	* RequestScope
	* JobScope
	* StepScope

## 주의사항

* org.springframework.beans.factory.config.ConfigurableBeanFactory.SCOPE_PROTOTYPE
	* 실제 `@Autowired` 해서 주입된 bean 을 getter 로 조회 한다면 동일한 객체에 접근이 된다.
	* 따라서 아래와 같은 방법으로 사용하자.
		* Provider<Object> 로 주입 받아아 한다.(javax)
		* ObjectFactory<Object> 로 주입 받는다. (spring)

## `SmartLifecycle` 를 통한 BeanLifecycle 관리

* `org.springframework.context.SmartLifecycle` interface 를 implements 하여서 Bean 의 Lifecycle 을 관리 할 수 있음.
* 어떤 곳에서 사용할까?
	* 어떤 빈을 가장 먼저 혹시 가장 후순위로 순서를 보장하면서 shutdown 시켜야 할때, 아주 유용 하다.

```java

@RequiredArgsConstructor
public class GracefulShutdownHandler implements SmartLifecycle {

		private final ThreadPoolTaskExecutor taskExecutor;

    @Override
    public boolean isAutoStartup() {
        return true;
    }

		/**
		 *
		 */
    @Override
    public void stop(Runnable callback) {
        stop();
        callback.run();
    }

    @Override
    public void start() {
        this.isRunning = true;
    }

    /*
        1. ThreadPoolTaskExecutor 종료
        2. Spring life cycle 을 따름.
     */
    @Override
    public void stop() {
        log.info("[ThreadPoolTaskExecutor 종료 시작]");
        this.taskExecutor.destroy();
        log.info("[ThreadPoolTaskExecutor 종료 완료]");

        this.isRunning = false;
        log.info("[GracefulShutdown 완료]");
    }


    @Override
    public boolean isRunning() {
        return this.isRunning;
    }

    @Override
    public int getPhase() {
        /*
            제일 마지막 start()
            제일 먼저 stop()
        */
        return Integer.MAX_VALUE;
    }
}
```
### 참고자료

* [https://blog.outsider.ne.kr/766](https://blog.outsider.ne.kr/766)


