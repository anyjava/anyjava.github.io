---
layout  : wiki
title   : Spring Batch
summary : Spring Batch 에 대한 사용방법 및 팁들을 정리 한다.
date    : 2018-12-11 21:08:18 +0900
updated : 2018-12-11 21:25:56 +0900
tags    : spring, batch
toc     : true
public  : true
parent  : Spring
latex   : false
adsense : true
---
* TOC
{:toc}

## Spring Batch 의 소소한 Tip 들

### JobInstace 와 JobExecution 의 관계

* Spring Batch 수행을 하면서, 파라미터를 변경 했는데 반영이 되지 않은적이 있지 않은가?
	* 원인은 Spring Batch 의 기본설정은 이전 batch job 이 실패 했을 경우, 이전 batch job 의 실행 파라미터로 재수행 되게 된다.
	* why? chuck 단위로 수행되고, 중간에 실패가 났을 경우, 실패난 위치부터 재수행 해야 하기 때문에
	* 따라서 우리는 이렇게 사용해보자.
		* 아주 대용량의 데이터를 다루는 배치가 아니라면, chunk 단위 수행 중 실패시, 무조건 clean 하고 처음부터 재수행 하도록 작성하자. (심플한 코드구조)
		* 다음은 실행할때마다 새로운 파라미터로 재수행 되도록 하는 방법
		* 주의 점, RunIncrementer() 를 사용하면, 이전 수행했던 파라미터를 다시 가져와서 수행한다!! 

```
    @Bean(name = "testJob")
    public Job testJob() {
        return jobBuilderFactory.get("testJob")
                .incrementer(new RunIdIncrementer())
                .preventRestart() // 항상 처음부터 재시작
                .start(step(null))
                .build();
           
```


### Spring Boot Application 을 수행하면, 특정 배치를 수행하고 싶을때,

* Spring Boot Job Lancher
 
```
/**
 * Spring Boot Batch Job Launcher
 * <p>
 * JVM System Property 로 --spring.batch.job.names=잡이름 지정(쉼표로 여러 job 지정 가능)
 * spring.active.profiles 등을 지정할 수 있음.
 * Program parameter param1=value1 param2=value2 형태로 Job Parameter 지정가능. parameter value 는 무조건 문자열로 된다.
 */
@Slf4j
@EnableBatchProcessing
@SpringBootApplication
public class BatchApplication extends DefaultBatchConfigurer {

    public static void main(String[] args) {
        int exit = SpringApplication.exit(SpringApplication.run(BatchApplication.class, args));
        log.info("exitCode={}", exit);
        System.exit(exit);
    }

    @Value("${spring.batch.job.names:NONE}")
    private String jobNames;

    /**
     * 혹시나 spring.batch.job.names 가 지정 안 될 경우 모든 batch job을 다 실행해버리기 때문에
     * 해당 값이 올바로 설정 돼 있는지를 검사해서 올바르지 않으면 오류를 발생시키도록 한다.
     */
    @PostConstruct
    public void validateJobNames() {
        log.info("jobNames : {}", jobNames);
        if (jobNames.isEmpty() || jobNames.equals("NONE")) {
            throw new IllegalStateException("spring.batch.job.names=job1,job2 형태로 실행을 원하는 Job을 명시해야만 합니다!");
        }
    }
}
```

### Tasklet 으로만 스프링 배치를 구성할 때,

* 어떨떄 사용할까? 
	* CASE 1. 레거시 java application 이나 통쿼리로 된걸 spring batch 로 옮겨야 할때, 우선 tasklet 으로으로만 구성된 배치를 만들자.
	* CASE 2. 아주 단순한 배치를 만들때,
	* 예제 코드: 아래 예저 코드중, `ResourcelessTransactionManager` 사용하는 것은 Transaction 관리를 Spring Batch 에서 하지 않겠다 는 뜻이다.

```
    @Bean
    @JobScope
    public Step step(@Value("#{jobParameters[param1]}") String param1) {
        return stepBuilderFactory.get("taskletStep").tasklet(
                (StepContribution contribution, ChunkContext chunkContext) -> {
                    log.info("==== Tasklet called....");
                    return RepeatStatus.FINISHED;
                })
                .transactionManager(new ResourcelessTransactionManager())   // Transaction 는 Service Layer 에 맡긴다.
                .build();
    }
```

### Step 간의 데이터 공유 방법

* 방법1. @JobScope 의 Bean 을 활용 하여 상태를 저장 한다.
	* 단순한 배치에서 사용해라. 재수행할때, clean 하고 처음부터 재수행하는 전략을 취할 때 적합하다.
* 방법2. Step_Execution_Context 를 이용한다.
	* 각 Step 마다의 상태를 DB 에 저장할 수 있음, 예정코드 추가 예정
