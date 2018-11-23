---
layout  : wiki
title   : Async
summary : Spring 에서의 비동기 처리 방법
date    : 2018-11-19 14:58:51 +0900
updated : 2018-11-23 19:54:43 +0900
tags    : spring, async, task_executor
toc     : true
public  : true
parent  : Spring
latex   : false
---
* TOC
{:toc}

# 참고자료
* [동기 vs 비동기 / sync vs async](https://www.youtube.com/watch?v=HKlUvCv9hvA&t=552s)

# Spring 에서 Async 사용을 위한 설정 방법

## 주의사항!!! ThreadPoolExecutor 를 꼭 설정하고 사용해야 함.
* 그렇지 않을 경우 Async 용 thread poll 은 하나만 설정됨

```
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


