---
layout  : wiki
title   : Async
summary : Spring 에서의 비동기 처리 방법
date    : 2018-11-19 14:58:51 +0900
updated : 2019-03-27 09:19:37 +0900
tags    : spring, async, task_executor
toc     : true
public  : true
parent  : Spring
latex   : false
adsense : true
---
* TOC
{:toc}

# 참고자료
* [동기 vs 비동기 / sync vs async](https://www.youtube.com/watch?v=HKlUvCv9hvA&t=552s)

# Spring 에서 Async 사용을 위한 설정 방법

## 주의사항!!! ThreadPoolExecutor 를 꼭 설정하고 사용해야 함.
* 그렇지 않을 경우 Async 용 `SimpleAsyncTaskExecutor` 를 사용하도록 default 가 되어 있음.

## Srping Boot 2.1 에서의 ThreadPoolExecutor

* auto-configuration 으로 `ThreadPoolTaskExecutor` 를 생성해준다. [https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-2.1-Release-Notes](https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-2.1-Release-Notes) 참고


```

    /**
     * 1. CORE_POOL_SIZE 를 먼저 소비 한다.
     * 2. CORE_POOL_SIZE 를 모두 점유 했다면, QUEUE_CAPACITY 값만큼 대기한다.
     * 3. QUEUE_CAPACITY 도 다 찬다면, MAX_POOL_SIZE 만큼 THREAD 를 생성해서 처리한다.
     * 주의!! 3가지의 설정을 적절히 해주어야 한다.
     */
    private static final int CORE_POOL_SIZE = 100;
    private static final int KEEP_ALIVE_SECONDS = 60;
    private static final int MAX_POOL_SIZE = Integer.MAX_VALUE;
    private static final int QUEUE_CAPACITY = Integer.MAX_VALUE;
    private static final boolean WAIT_FOR_JOBS_TO_COMPLETE_ON_SHUTDOWN = true;
    private static final int AWAIT_TERMINATION_SECONDS = 15;
		
    @Bean
    @Primary
    public ThreadPoolTaskExecutor adCenterTaskExecutor() {

        ThreadPoolTaskExecutor threadPoolTaskExecutor = new ThreadPoolTaskExecutor();

        threadPoolTaskExecutor.setThreadNamePrefix("AdCenter-");
        threadPoolTaskExecutor.setCorePoolSize(CORE_POOL_SIZE);
        threadPoolTaskExecutor.setMaxPoolSize(MAX_POOL_SIZE);
        threadPoolTaskExecutor.setQueueCapacity(QUEUE_CAPACITY);
        threadPoolTaskExecutor.setKeepAliveSeconds(KEEP_ALIVE_SECONDS);
        threadPoolTaskExecutor.setWaitForTasksToCompleteOnShutdown(WAIT_FOR_JOBS_TO_COMPLETE_ON_SHUTDOWN);
        threadPoolTaskExecutor.setAwaitTerminationSeconds(AWAIT_TERMINATION_SECONDS);

        return threadPoolTaskExecutor;
    }
```


