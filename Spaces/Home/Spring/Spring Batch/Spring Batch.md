# **Spring Batch**

Spring Batch는 로깅/추적, 트랜잭션 관리, 작업 처리 통계, 작업 재시작, 건너뛰기, 리소스 관리 등 대용량 레코드 처리에 필수적인 기능을 제공합니다. 또한 최적화 및 파티셔닝 기술을 통해 대용량 및 고성능 배치 작업을 가능하게 하는 고급 기술 서비스 및 기능을 제공합니다.

Spring Batch에서 배치가 실패하여 작업 재시작을 하게 된다면 처음부터가 아닌 실패한 지점부터 실행을 하게 됩니다.

또한 중복 실행을 막기 위해 성공한 이력이 있는 Batch는 동일한 Parameters로 실행 시 Exception이 발생하게 됩니다.

## **Spring Batch vs Quartz? Scheduler?**

> **Spring Batch는 Scheduler가 아니기에 비교 대상이 아닙니다.**

Spring Batch는 Batch Job을 관리하지만 Job을 구동하거나 실행시키는 기능은 지원하고 있지 않습니다. Spring에서 Batch Job을 실행시키기 위해서는 Quartz, Scheduler, Jenkins등 전용 Scheduler를 사용하여야 합니다.



# **Spring Batch 용어**

## **Job**

Job은 배치처리 과정을 하나의 단위로 만들어 놓은 객체입니다. 또한 배치처리 과정에 있어 전체 계층 최상단에 위치하고 있습니다.

## **JobInstance**

JobInstance는 Job의 실행의 단위를 나타냅니다. Job을 실행시키게 되면 하나의 JobInstance가 생성되게 됩니다. 예를들어 1월 1일 실행, 1월 2일 실행을 하게 되면 각각의 JobInstance가 생성되며 1월 1일 실행한 JobInstance가 실패하여 다시 실행을 시키더라도 이 JobInstance는 1월 1일에 대한 데이터만 처리하게 됩니다.

## **JobParameters**

JobInstance는 Job의 실행 단위라고 했습니다. 그렇다면 JonInstance는 어떻게구별 할까요? 이는 바로 JobParameters 객체로 구분하게 됩니다. JobParameters는 JobInstance 구별 외에도 개발자 JobInstacne에 전달되는 매개변수 역할도 하고 있습니다.

또한 JobParameters는 String, Double, Long, Date 4가지 형식만을 지원하고 있습니다.

## **JobExecution**

JobExecution은 JobInstance에 대한 실행 시도에 대한 객체입니다. 1월 1일에 실행한 JobInstacne가 실패하여 재실행을 하여도 동일한 JobInstance를 실행시키지만 이 2번에 실행에 대한 JobExecution은 개별로 생기게 됩니다. JobExecution는 이러한 JobInstance 실행에 대한 상태,시작시간, 종료시간, 생성시간 등의 정보를 담고 있습니다.

## **Step**

Step은 Job의 배치처리를 정의하고 순차적인 단계를 캡슐화 합니다. Job은 최소한 1개 이상의 Step을 가져야 하며 Job의 실제 일괄 처리를 제어하는 모든 정보가 들어있습니다.

## **StepExecution**

StepExecution은 JobExecution과 동일하게 Step 실행 시도에 대한 객체를 나타냅니다. 하지만 Job이 여러개의 Step으로 구성되어 있을 경우 이전 단계의 Step이 실패하게 되면 다음 단계가 실행되지 않음으로 실패 이후 StepExecution은 생성되지 않습니다. StepExecution 또한 JobExecution과 동일하게 실제 시작이 될 때만 생성됩니다. StepExecution에는 JobExecution에 저장되는 정보 외에 read 수, write 수, commit 수, skip 수 등의 정보들도 저장이 됩니다.

## **ExecutionContext**

ExecutionContext란 Job에서 데이터를 공유 할 수 있는 데이터 저장소입니다. Spring Batch에서 제공하느 ExecutionContext는 JobExecutionContext, StepExecutionContext 2가지 종류가 있으나 이 두가지는 지정되는 범위가 다릅니다. JobExecutionContext의 경우 Commit 시점에 저장되는 반면 StepExecutionContext는 실행 사이에 저장이 되게 됩니다. ExecutionContext를 통해 Step간 Data 공유가 가능하며 Job 실패시 ExecutionContext를 통한 마지막 실행 값을 재구성 할 수 있습니다.

## **JobRepository**

JobRepository는 위에서 말한 모든 배치 처리 정보를 담고있는 매커니즘입니다. Job이 실행되게 되면 JobRepository에 JobExecution과 StepExecution을 생성하게 되며 JobRepository에서 Execution 정보들을 저장하고 조회하며 사용하게 됩니다.

## **JobLauncher**

JobLauncher는 Job과 JobParameters를 사용하여 Job을 실행하는 객체입니다.

## **ItemReader**

ItemReader는 Step에서 Item을 읽어오는 인터페이스입니다. ItemReader에 대한 다양한 인터페이스가 존재하며 다양한 방법으로 Item을 읽어 올 수 있습니다.

## **ItemWriter**

ItemWriter는 처리 된 Data를 Writer 할 때 사용한다. Writer는 처리 결과물에 따라 Insert가 될 수도 Update가 될 수도 Queue를 사용한다면 Send가 될 수도 있다. Writer 또한 Read와 동일하게 다양한 인터페이스가 존재한다. Writer는 기본적으로 Item을 Chunk로 묶어 처리하고 있습니다.

## **ItemProcessor**

Item Processor는 Reader에서 읽어온 Item을 데이터를 처리하는 역할을 하고 있다. Processor는 배치를 처리하는데 필수 요소는 아니며 Reader, Writer, Processor 처리를 분리하여 각각의 역할을 명확하게 구분하고 있습니다.



# **Spring Batch 사용하기**

Spring Batch에서의 Job은 여러가지 Step의 모음으로 구성되어 있으며 Job은 순차적인 Step을 수행하며 Batch를 수행하게 됩니다. Step은 Tasklet 처리 방식과 Chunk 지향 처리 방식을 지원하고 있습니다.

![[Spring Batch1.png]]


### **Job Example 1 - 단일 Step 구성하기**



```java
package com.finnq.batch.job.config;

import lombok.extern.slf4j.Slf4j;
import org.springframework.batch.core.Job;
import org.springframework.batch.core.Step;
import org.springframework.batch.core.configuration.annotation.EnableBatchProcessing;
import org.springframework.batch.core.configuration.annotation.JobBuilderFactory;
import org.springframework.batch.core.configuration.annotation.StepBuilderFactory;
import org.springframework.batch.repeat.RepeatStatus;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Slf4j
@Configuration
@EnableBatchProcessing
public class ExampleJobConfig {

    @Autowired public JobBuilderFactory jobBuilderFactory;
    @Autowired public StepBuilderFactory stepBuilderFactory;

    @Bean
    public Job ExampleJob(){

        Job exampleJob = jobBuilderFactory.get("exampleJob")
                .start(Step())
                .build();

        return exampleJob;
    }

    @Bean
    public Step Step() {
        return stepBuilderFactory.get("step")
                .tasklet((contribution, chunkContext) -> {
                    log.info("Step!");
                    return RepeatStatus.FINISHED;
                })
                .build();
    }


}
```



### **Job Example 2 - 다중 Step 구성하기**



```java
package com.finnq.batch.job.config;

import lombok.extern.slf4j.Slf4j;
import org.springframework.batch.core.Job;
import org.springframework.batch.core.Step;
import org.springframework.batch.core.configuration.annotation.EnableBatchProcessing;
import org.springframework.batch.core.configuration.annotation.JobBuilderFactory;
import org.springframework.batch.core.configuration.annotation.StepBuilderFactory;
import org.springframework.batch.repeat.RepeatStatus;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Slf4j
@Configuration
@EnableBatchProcessing
public class ExampleJobConfig {

    @Autowired public JobBuilderFactory jobBuilderFactory;
    @Autowired public StepBuilderFactory stepBuilderFactory;

    @Bean
    public Job ExampleJob(){

        Job exampleJob = jobBuilderFactory.get("exampleJob")
                .start(startStep())
                .next(nextStep())
                .next(lastStep())
                .build();

        return exampleJob;
    }

    @Bean
    public Step startStep() {
        return stepBuilderFactory.get("startStep")
                .tasklet((contribution, chunkContext) -> {
                    log.info("Start Step!");
                    return RepeatStatus.FINISHED;
                })
                .build();
    }

    @Bean
    public Step nextStep(){
        return stepBuilderFactory.get("nextStep")
                .tasklet((contribution, chunkContext) -> {
                    log.info("Next Step!");
                    return RepeatStatus.FINISHED;
                })
                .build();
    }

    @Bean
    public Step lastStep(){
        return stepBuilderFactory.get("lastStep")
                .tasklet((contribution, chunkContext) -> {
                    log.info("Last Step!!");
                    return RepeatStatus.FINISHED;
                })
                .build();
    }

}
```


# **다양한 Step 설정**

## **Step에서 startlimit사용하기**



```java
   @Bean
    @JobScope
    public Step Step() throws Exception {
        return stepBuilderFactory.get("Step")
                .startLimit(3)
                .<Member,Member>chunk(10)
                .reader(reader(null))
                .processor(processor(null))
                .writer(writer(null))
                .build();
    }
```


## **Step에서 Skip 사용하기**


```java
   @Bean
    @JobScope
    public Step Step() throws Exception {
        return stepBuilderFactory.get("Step")
                .<Member,Member>chunk(10)
                .reader(reader(null))
                .processor(processor(null))
                .writer(writer(null))
                .faultTolerant()
                .skipLimit(1) // skip 허용 횟수, 해당 횟수 초과시 Error 발생, Skip 사용시 필수 설정
                .skip(NullPointerException.class)// NullPointerException에 대해선 Skip
                .noSkip(SQLException.class) // SQLException에 대해선 noSkip
                //.skipPolicy(new CustomSkipPolilcy) // 사용자가 커스텀하며 Skip Policy 설정 가능
                .build();
    }
```


## **Step에서 Retry 사용하기**


```java
    @Bean
    @JobScope
    public Step Step() throws Exception {
        return stepBuilderFactory.get("Step")
                .<Member,Member>chunk(10)
                .reader(reader(null))
                .processor(processor(null))
                .writer(writer(null))
                .faultTolerant()
                .retryLimit(1) //retry 횟수, retry 사용시 필수 설정, 해당 Retry 이후 Exception시 Fail 처리
                .retry(SQLException.class) // SQLException에 대해선 Retry 수행
                .noRetry(NullPointerException.class) // NullPointerException에 no Retry
                //.retryPolicy(new CustomRetryPolilcy) // 사용자가 커스텀하며 Retry Policy 설정 가능
                .build();
    }
```


## **Step에서 noRollback 사용하기**


```java
    @Bean
    @JobScope
    public Step Step() throws Exception {
        return stepBuilderFactory.get("Step")
                .<Member,Member>chunk(10)
                .reader(reader(null))
                .processor(processor(null))
                .writer(writer(null))
                .faultTolerant()
                .noRollback(NullPointerException.class) // NullPointerException 발생  rollback이 되지 않게 설정
                .build();
    }
```


# **STEP을 구성하는 Tasklet과 Chunk 지향 처리**

## **Tasklet**

Tasklet은 하나의 메서드로 구성 되어있는 간단한 인터페이스입니다. 이 메서드 는 실패를 알리기 위해 예외를 반환 하거나 throw할 때까지 execute를 반복적으로 호출하게 됩니다 .


![[Spring Batch2.png]]

### **Tasklet Example 1 - Job Class 안에 Tasklet 구현하기(Lambda)**


```java
package com.finnq.batch.job.cofig;

import lombok.extern.slf4j.Slf4j;
import org.springframework.batch.core.Job;
import org.springframework.batch.core.Step;
import org.springframework.batch.core.configuration.annotation.EnableBatchProcessing;
import org.springframework.batch.core.configuration.annotation.JobBuilderFactory;
import org.springframework.batch.core.configuration.annotation.StepBuilderFactory;
import org.springframework.batch.repeat.RepeatStatus;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Slf4j
@EnableBatchProcessing
@Configuration
public class TaskletJobConfig{

    @Autowired public JobBuilderFactory jobBuilderFactory;
    @Autowired public StepBuilderFactory stepBuilderFactory;

    @Bean
    public Job TaskletJob(){

        Job customJob = jobBuilderFactory.get("taskletJob")
                .start(TaskStep())
                .build();

        return customJob;
    }

    @Bean
    public Step TaskStep(){
        return stepBuilderFactory.get("taskletStep")
                .tasklet((contribution, chunkContext) ->{

                    //비즈니스 로직
                    for(int idx = 0; idx < 10; idx ++){
                        log.info("[idx] = " + idx);
                    }

                   return RepeatStatus.FINISHED;
                }).build();
    }

}
```


### **Taskle Example 2 - MethodInvokingAdapter를 사용하여 구현하기**



```java
package com.finnq.batch.job.serivce;

import lombok.extern.slf4j.Slf4j;

@Slf4j
public class CustomService {

    public void businessLogic() {
        //비즈니스 로직
        for(int idx = 0; idx < 10; idx ++){
            log.info("[idx] = " + idx);
        }
    }
}
```




```java
package com.finnq.batch.job.config;

import com.finnq.batch.job.serivce.CustomService;
import lombok.extern.slf4j.Slf4j;
import org.springframework.batch.core.Job;
import org.springframework.batch.core.Step;
import org.springframework.batch.core.configuration.annotation.EnableBatchProcessing;
import org.springframework.batch.core.configuration.annotation.JobBuilderFactory;
import org.springframework.batch.core.configuration.annotation.StepBuilderFactory;
import org.springframework.batch.core.step.tasklet.MethodInvokingTaskletAdapter;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Slf4j
@Configuration
@EnableBatchProcessing
public class TaskletJobConfig{

    @Autowired public JobBuilderFactory jobBuilderFactory;
    @Autowired public StepBuilderFactory stepBuilderFactory;

    @Bean
    public Job TaskletJob(){

        Job customJob = jobBuilderFactory.get("taskletJob")
                .start(TaskStep())
                .build();

        return customJob;
    }

    @Bean
    public Step TaskStep(){
        return stepBuilderFactory.get("taskletStep")
                .tasklet(myTasklet()).build();
    }

    @Bean
    public CustomService service() {
        return new CustomService ();
    }

    @Bean
    public MethodInvokingTaskletAdapter myTasklet() {
        MethodInvokingTaskletAdapter adapter = new MethodInvokingTaskletAdapter();

        adapter.setTargetObject(service());
        adapter.setTargetMethod("businessLogic");

        return adapter;
    }

}
```



### **Taskle Example 3 - 외부 클래스를 사용하여 Tasklet 구현하기**



```java
package com.finnq.batch.job.serivce;

import lombok.extern.slf4j.Slf4j;
import org.springframework.batch.core.ExitStatus;
import org.springframework.batch.core.StepContribution;
import org.springframework.batch.core.StepExecution;
import org.springframework.batch.core.StepExecutionListener;
import org.springframework.batch.core.annotation.AfterStep;
import org.springframework.batch.core.annotation.BeforeStep;
import org.springframework.batch.core.scope.context.ChunkContext;
import org.springframework.batch.core.step.tasklet.Tasklet;
import org.springframework.batch.repeat.RepeatStatus;


@Slf4j
public class BusinessTasklet implements Tasklet, StepExecutionListener {

    @Override
    @BeforeStep
    public void beforeStep(StepExecution stepExecution) {
        log.info("Before Step Start!");
    }

    @Override
    @AfterStep
    public ExitStatus afterStep(StepExecution stepExecution) {

        log.info("After Step Start!");

        return ExitStatus.COMPLETED;
    }

    @Override
    public RepeatStatus execute(StepContribution stepContribution, ChunkContext chunkContext) throws Exception {

        //비즈니스 로직
        for (int idx = 0; idx < 10; idx++) {
            log.info("[idx] = " + idx);
        }

        return RepeatStatus.FINISHED;
    }

}
```

```java
package com.finnq.batch.job.config;

import com.finnq.batch.job.serivce.BusinessTasklet;
import lombok.extern.slf4j.Slf4j;
import org.springframework.batch.core.Job;
import org.springframework.batch.core.Step;
import org.springframework.batch.core.configuration.annotation.EnableBatchProcessing;
import org.springframework.batch.core.configuration.annotation.JobBuilderFactory;
import org.springframework.batch.core.configuration.annotation.StepBuilderFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Slf4j
@Configuration
@EnableBatchProcessing
public class TaskletJobConfig{

    @Autowired public JobBuilderFactory jobBuilderFactory;
    @Autowired public StepBuilderFactory stepBuilderFactory;

    @Bean
    public Job TaskletJob(){

        Job customJob = jobBuilderFactory.get("taskletJob")
                .start(TaskStep())
                .build();

        return customJob;
    }

    @Bean
    public Step TaskStep(){
        return stepBuilderFactory.get("taskletStep")
                .tasklet(new BusinessTasklet())
                .build();
    }
    
}
```


Taslket에서는 @BeforeStep, @AfterStep을 통해 execute 배치 실행 전 후에 Event를 등록하여 실행 시킬 수 있습니다.

## **Chunk**

Spring Batch에서의 Chunk란 처리 되는 커밋 row 수를 의미합니다. Batch 처리에서 커밋 되는 row 수라는건 chunk 단위로 Transaction을 수행하기 때문에 실패시 Chunk 단위 만큼 rollback이 되게 됩니다.

Chunk 지향 처리에서는 다음과 같은 3가지 시나리오로 실행 됩니다

- 읽기(Read) — Database에서 배치처리를 할 Data를 읽어온다
- 처리(Processing) — 읽어온 Data를 가공,처리를 한다 (필수사항X)
- 쓰기(Write) — 가공,처리한 데이터를 Database에 저장한다.


![[Pasted image 20240208095323.png]]


하기 그림은 Chunk 지향 처리에서 배치가 수행되는 그림입니다.


![[Pasted image 20240208095329.png]]



하기 코드는 위의 그림과 동일한 처리를 보여줍니다.


```java
List items = new Arraylist();
for(int i = 0; i < commitInterval; i++){
    Object item = itemReader.read();
    if (item != null) {
        items.add(item);
    }
}

List processedItems = new Arraylist();
for(Object item: items){
    Object processedItem = itemProcessor.process(item);
    if (processedItem != null) {
        processedItems.add(processedItem);
    }
}

itemWriter.write(processedItems);
```


## **Chunk Example에 들어가기 앞서..**

Spring Batch에는 다양한 ItemReader와 ItemWriter가 존재합니다. 대용량 배치 처리를 하게 되면 Item을 읽어 올 때 Paging 처리를 하는게 효과적입니다. Spring Batch Reader에서는 이러한 Paging 처리를 지원하고 있습니다. 또한 적절한 Paging처리와 Chunk Size(한번에 처리 될 트랜잭션)를 설정하여 더욱 효과적인 배치 처리를 할 수 있습니다.

## **적절한 Paging Size와 Chunk Size에 관하여..**

Paging Size와 Chunk Size의 관계는 다음과 같이 이루어 집니다.

Paging Size가 5이며 Chunk Size가 10일 경우 2번의 Read가 이루어진 후에 1번의 Transaction이 수행됩니다. 이는 한번의 Transaction을 위해 2번의 쿼리 수행이 발생하게 됩니다.

이에 따른 적절한 Paging Size와 Chunk Size에 대해 Spring Batch에는 다음과 같이 적혀 있습니다.

> **Setting a fairly large page size and using a commit interval that matches the page size should provide better performance.**
> 
> **페이지 크기를 상당히 크게 설정하고 페이지 크기와 일치하는 커밋 간격을 사용하면 성능이 향상됩니다.**

이와 같이 한번의 Read 쿼리 수행시 1번의 Transaction을 위해 두 설정의 값을 일치를 시키는게 가장 좋은 성능 향상 방법이며 특별한 이유가 없는 한 Paging Size 와 Chunk Size를 동일하게 설정하는 것을 추천합니다.

## **PagingReader 사용 시 주의사항**

페이징 처리 시 각 쿼리에 Offset 과 , Limit를 지정해 주어야 하는데 이는 PageSize를 지정하면 Batch에서 Offset과 Limit를 지정해줍니다. 하지만 페이징 처리를 할 때 마다 새로운 쿼리를 실행하기 때문에 데이터 순서가 보장 될 수 있도록 반드시 Order By를 사용하여야 합니다.

### **Chunk Example 1 - Jdbc 기반의 Batch Job 구현하기**


```java
package com.finnq.batch.job.config;

import com.finnq.batch.model.Member;
import lombok.extern.slf4j.Slf4j;
import org.springframework.batch.core.Job;
import org.springframework.batch.core.Step;
import org.springframework.batch.core.configuration.annotation.*;
import org.springframework.batch.item.ItemProcessor;
import org.springframework.batch.item.database.JdbcBatchItemWriter;
import org.springframework.batch.item.database.JdbcPagingItemReader;
import org.springframework.batch.item.database.Order;
import org.springframework.batch.item.database.PagingQueryProvider;
import org.springframework.batch.item.database.builder.JdbcBatchItemWriterBuilder;
import org.springframework.batch.item.database.builder.JdbcPagingItemReaderBuilder;
import org.springframework.batch.item.database.support.SqlPagingQueryProviderFactoryBean;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.jdbc.core.BeanPropertyRowMapper;

import javax.sql.DataSource;
import java.util.HashMap;
import java.util.Map;

/**
 * 전체 금액이 10,000원 이상인 회원들에게 1,000원 캐시백을 주는 배치
 */

@Slf4j
@Configuration
@EnableBatchProcessing
public class ExampleJobConfig {

    @Autowired public JobBuilderFactory jobBuilderFactory;
    @Autowired public StepBuilderFactory stepBuilderFactory;
    @Autowired public DataSource dataSource;

    @Bean
    public Job ExampleJob() throws Exception {


        Job exampleJob = jobBuilderFactory.get("exampleJob")
                .start(Step())
                .build();

        return exampleJob;
    }

    @Bean
    @JobScope
    public Step Step() throws Exception {
        return stepBuilderFactory.get("Step")
                .<Member,Member>chunk(10)
                .reader(reader())
                .processor(processor())
                .writer(writer())
                .build();
    }

    @Bean
    @StepScope
    public JdbcPagingItemReader<Member> reader() throws Exception {

        Map<String,Object> parameterValues = new HashMap<>();
        parameterValues.put("amount", "10000");

        //pageSize와 fethSize는 동일하게 설정
        return new JdbcPagingItemReaderBuilder<Member>()
                .pageSize(10)
                .fetchSize(10)
                .dataSource(dataSource)
                .rowMapper(new BeanPropertyRowMapper<>(Member.class))
                .queryProvider(customQueryProvider())
                .parameterValues(parameterValues)
                .name("JdbcPagingItemReader")
                .build();
    }

    @Bean
    @StepScope
    public ItemProcessor<Member, Member> processor(){

        return new ItemProcessor<Member, Member>() {
            @Override
            public Member process(Member member) throws Exception {

                //1000원 추가 적립
                member.setAmount(member.getAmount() + 1000);

                return member;
            }
        };
    }

    @Bean
    @StepScope
    public JdbcBatchItemWriter<Member> writer(){
        return new JdbcBatchItemWriterBuilder<Member>()
                .dataSource(dataSource)
                .sql("UPDATE MEMBER SET AMOUNT = :amount WHERE ID = :id")
                .beanMapped()
                .build();

    }

    public PagingQueryProvider customQueryProvider() throws Exception {
        SqlPagingQueryProviderFactoryBean queryProviderFactoryBean = new SqlPagingQueryProviderFactoryBean();

        queryProviderFactoryBean.setDataSource(dataSource);

        queryProviderFactoryBean.setSelectClause("SELECT ID, NAME, EMAIL, NICK_NAME, STATUS, AMOUNT ");
        queryProviderFactoryBean.setFromClause("FROM MEMBER ");
        queryProviderFactoryBean.setWhereClause("WHERE AMOUNT >= :amount");

        Map<String,Order> sortKey = new HashMap<>();
        sortKey.put("id", Order.ASCENDING);

        queryProviderFactoryBean.setSortKeys(sortKey);

        return queryProviderFactoryBean.getObject();

    }

}
```


### **Chunk Example 2 - JPA 기반의 Batch Job 구현하기**


```java
package com.finnq.batch.job.config;

import com.finnq.batch.model.Member;
import lombok.extern.slf4j.Slf4j;
import org.springframework.batch.core.Job;
import org.springframework.batch.core.Step;
import org.springframework.batch.core.configuration.annotation.*;
import org.springframework.batch.item.ItemProcessor;
import org.springframework.batch.item.database.JpaItemWriter;
import org.springframework.batch.item.database.JpaPagingItemReader;
import org.springframework.batch.item.database.builder.JpaItemWriterBuilder;
import org.springframework.batch.item.database.builder.JpaPagingItemReaderBuilder;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import javax.persistence.EntityManagerFactory;
import java.util.HashMap;
import java.util.Map;

/**
 * 전체 금액이 10,000원 이상인 회원들에게 1,000원 캐시백을 주는 배치
 */

@Slf4j
@Configuration
@EnableBatchProcessing
public class ExampleJobConfig {

    @Autowired public JobBuilderFactory jobBuilderFactory;
    @Autowired public StepBuilderFactory stepBuilderFactory;
    @Autowired public EntityManagerFactory entityManagerFactory;

    @Bean
    public Job ExampleJob() throws Exception {

        Job exampleJob = jobBuilderFactory.get("exampleJob")
                .start(Step())
                .build();

        return exampleJob;
    }

    @Bean
    @JobScope
    public Step Step() throws Exception {
        return stepBuilderFactory.get("Step")
                .<Member,Member>chunk(10)
                .reader(reader())
                .processor(processor())
                .writer(writer())
                .build();
    }

    @Bean
    @StepScope
    public JpaPagingItemReader<Member> reader() throws Exception {

        Map<String,Object> parameterValues = new HashMap<>();
        parameterValues.put("amount", 10000);

        return new JpaPagingItemReaderBuilder<Member>()
                .pageSize(10)
                .parameterValues(parameterValues)
                .queryString("SELECT p FROM Member p WHERE p.amount >= :amount ORDER BY id ASC")
                .entityManagerFactory(entityManagerFactory)
                .name("JpaPagingItemReader")
                .build();
    }

    @Bean
    @StepScope
    public ItemProcessor<Member, Member> processor(){

        return new ItemProcessor<Member, Member>() {
            @Override
            public Member process(Member member) throws Exception {

                //1000원 추가 적립
                member.setAmount(member.getAmount() + 1000);

                return member;
            }
        };
    }

    @Bean
    @StepScope
    public JpaItemWriter<Member> writer(){
        return new JpaItemWriterBuilder<Member>()
                .entityManagerFactory(entityManagerFactory)
                .build();
    }
}
```



### **Chunk Example 3 - Mybatis 기반의 Batch Job 구현하기**

```java
package com.finnq.batch.job.config;

import com.finnq.batch.model.Member;
import lombok.extern.slf4j.Slf4j;
import org.apache.ibatis.session.SqlSessionFactory;
import org.mybatis.spring.batch.MyBatisBatchItemWriter;
import org.mybatis.spring.batch.MyBatisPagingItemReader;
import org.mybatis.spring.batch.builder.MyBatisBatchItemWriterBuilder;
import org.mybatis.spring.batch.builder.MyBatisPagingItemReaderBuilder;
import org.springframework.batch.core.Job;
import org.springframework.batch.core.Step;
import org.springframework.batch.core.configuration.annotation.*;
import org.springframework.batch.item.ItemProcessor;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import java.util.HashMap;
import java.util.Map;

/**
 * 전체 금액이 10,000원 이상인 회원들에게 1,000원 캐시백을 주는 배치
 */

@Slf4j
@Configuration
@EnableBatchProcessing
public class ExampleJobConfig {

    @Autowired public JobBuilderFactory jobBuilderFactory;
    @Autowired public StepBuilderFactory stepBuilderFactory;
    @Autowired public SqlSessionFactory sqlSessionFactory;

    @Bean
    public Job ExampleJob() throws Exception {
        
        Job exampleJob = jobBuilderFactory.get("exampleJob")
                .start(Step())
                .build();

        return exampleJob;
    }

    @Bean
    @JobScope
    public Step Step() throws Exception {
        return stepBuilderFactory.get("Step")
                .<Member,Member>chunk(10)
                .reader(reader())
                .processor(processor())
                .writer(writer())
                .build();
    }

    @Bean
    @StepScope
    public MyBatisPagingItemReader<Member> reader() throws Exception {

        Map<String,Object> parameterValues = new HashMap<>();
        parameterValues.put("amount", "10000");

        return new MyBatisPagingItemReaderBuilder<Member>()
                .pageSize(10)
                .sqlSessionFactory(sqlSessionFactory)
                //Mapper안에서도 Paging 처리 시 OrderBy는 필수!
                .queryId("com.finnq.batch.db.mapper.memberMapper.selectMemberInfo")
                .parameterValues(parameterValues)
                .build();
    }

    @Bean
    @StepScope
    public ItemProcessor<Member, Member> processor(){

        return new ItemProcessor<Member, Member>() {
            @Override
            public Member process(Member member) throws Exception {

                //1000원 추가 적립
                member.setAmount(member.getAmount() + 1000);

                return member;
            }
        };
    }

    @Bean
    @StepScope
    public MyBatisBatchItemWriter<Member> writer(){
        return new MyBatisBatchItemWriterBuilder<Member>()
                //.assertUpdates(false)
                .sqlSessionFactory(sqlSessionFactory)
                .statementId("com.finnq.batch.db.mapper.memberMapper.insertMember")
                .build();
    }

}
```


MyBatisBatchItemWriter Class에 write에서는 result Size 1이 아닌경우 Exception을 Throws 합니다.


저는 Processor 안에서 배치 처리를 하며 write와 별개로 DB Insert를 수행하고 있었는데 해당 Insert로 인해서 result Size가 2가 되면서 다음과 같은 Error가 발생 했었습니다.

해당 문제 해결 방법은 MyBatisBatchItemWriterBuilder에서 assertUpdates(false)로 간단하게 해결 할 수 있습니다.

## **@JobScope, @StepScope?**

Chunk 지향 처리 Example을 확인하면 @JobScope와 @StepScope Annotation을 확인할 수 있습니다.

@JobScope는 Step 선언문에 사용 가능하며 @StepScope는 Step을 구성하는 ItemReader, ItemProcessor, ItemWriter에 사용이 가능합니다.


@JobScope와 @StepScope는 Singleton 패턴이 아닌 Annotation이 명시된 메소드의 실행 시점에 Bean이 생성되게 됩니다. 또한 @JobScope와 @StepScope Bean이 생성 될 때 JobParameter가 생성되기 때문에 JobParameter 사용하기 위해선 반드시 Scope를 지정해주어야 합니다. 이는 LateBinding을 하여 JobParameter를 비즈니스 로직 단계에서 할당하여 보다 유연한 설계를 가능하게 하고 서로 다른 Step이 서로를 침범하지 않고 병렬로 실행되게 하기 위함입니다.

## **JobLauncher로 Job 실행 시키기**


```java
package com.finnq.batch.scheduler;

import lombok.extern.slf4j.Slf4j;
import org.springframework.batch.core.*;
import org.springframework.batch.core.launch.JobLauncher;
import org.springframework.batch.core.repository.JobExecutionAlreadyRunningException;
import org.springframework.batch.core.repository.JobInstanceAlreadyCompleteException;
import org.springframework.batch.core.repository.JobRestartException;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Configuration;
import org.springframework.scheduling.annotation.Scheduled;

import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.HashMap;
import java.util.Map;

@Configuration
@Slf4j
public class JobScheduler {

	@Autowired
	private JobLauncher jobLauncher;

	@Autowired
	private Job ExampleJob;

	@Scheduled(cron = "1 * * * * *")
	public void jobSchduled() throws JobParametersInvalidException, JobExecutionAlreadyRunningException,
			JobRestartException, JobInstanceAlreadyCompleteException {

		Map<String, JobParameter> jobParametersMap = new HashMap<>();
		
		SimpleDateFormat format1 = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss:SSS");
		Date time = new Date();

		String time1 = format1.format(time);

		jobParametersMap.put("date",new JobParameter(time1));

		JobParameters parameters = new JobParameters(jobParametersMap);

		JobExecution jobExecution = jobLauncher.run(ExampleJob, parameters);

		while (jobExecution.isRunning()) {
			log.info("...");
		}

		log.info("Job Execution: " + jobExecution.getStatus());
		log.info("Job getJobConfigurationName: " + jobExecution.getJobConfigurationName());
		log.info("Job getJobId: " + jobExecution.getJobId());
		log.info("Job getExitStatus: " + jobExecution.getExitStatus());
		log.info("Job getJobInstance: " + jobExecution.getJobInstance());
		log.info("Job getStepExecutions: " + jobExecution.getStepExecutions());
		log.info("Job getLastUpdated: " + jobExecution.getLastUpdated());
		log.info("Job getFailureExceptions: " + jobExecution.getFailureExceptions());
		
	}
}
```


## **JobParameters 사용하기**


```java
package com.finnq.batch.job.config;

import com.finnq.batch.model.Member;
import lombok.extern.slf4j.Slf4j;
import org.springframework.batch.core.Job;
import org.springframework.batch.core.Step;
import org.springframework.batch.core.configuration.annotation.*;
import org.springframework.batch.item.ItemProcessor;
import org.springframework.batch.item.database.JpaItemWriter;
import org.springframework.batch.item.database.JpaPagingItemReader;
import org.springframework.batch.item.database.builder.JpaItemWriterBuilder;
import org.springframework.batch.item.database.builder.JpaPagingItemReaderBuilder;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import javax.persistence.EntityManagerFactory;
import java.util.HashMap;
import java.util.Map;

/**
 * 전체 금액이 10,000원 이상인 회원들에게 1,000원 캐시백을 주는 배치
 */

@Slf4j
@Configuration
@EnableBatchProcessing
public class ExampleJobConfig {

    @Autowired public JobBuilderFactory jobBuilderFactory;
    @Autowired public StepBuilderFactory stepBuilderFactory;
    @Autowired public EntityManagerFactory entityManagerFactory;

    @Bean
    public Job ExampleJob() throws Exception {

        Job exampleJob = jobBuilderFactory.get("exampleJob")
                .start(Step())
                .build();

        return exampleJob;
    }

    @Bean
    @JobScope
    public Step Step() throws Exception {
        return stepBuilderFactory.get("Step")
                .<Member,Member>chunk(10)
                .reader(reader(null))
                .processor(processor(null))
                .writer(writer(null))
                .build();
    }

    @Bean
    @StepScope
    public JpaPagingItemReader<Member> reader(@Value("#{jobParameters[date]}")  String date) throws Exception {

        log.info("jobParameters value : " + date);

        Map<String,Object> parameterValues = new HashMap<>();
        parameterValues.put("amount", 10000);

        return new JpaPagingItemReaderBuilder<Member>()
                .pageSize(10)
                .parameterValues(parameterValues)
                .queryString("SELECT p FROM Member p WHERE p.amount >= :amount ORDER BY id ASC")
                .entityManagerFactory(entityManagerFactory)
                .name("JpaPagingItemReader")
                .build();
    }

    @Bean
    @StepScope
    public ItemProcessor<Member, Member> processor(@Value("#{jobParameters[date]}")  String date){

        return new ItemProcessor<Member, Member>() {
            @Override
            public Member process(Member member) throws Exception {

                log.info("jobParameters value : " + date);

                //1000원 추가 적립
                member.setAmount(member.getAmount() + 1000);

                return member;
            }
        };
    }

    @Bean
    @StepScope
    public JpaItemWriter<Member> writer(@Value("#{jobParameters[date]}")  String date){

        log.info("jobParameters value : " + date);

        return new JpaItemWriterBuilder<Member>()
                .entityManagerFactory(entityManagerFactory)
                .build();
    }
}

```


# **Spring Batch 환경 구성**

## **Application**


```java
package com.finnq.batch;

import org.springframework.batch.core.configuration.annotation.EnableBatchProcessing;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;


@SpringBootApplication
@EnableBatchProcessing
public class FinnqBatchApplication {

    public static void main(String args[]){
        SpringApplication.run(FinnqBatchApplication.class, args);
    }

}
```


## **dependencies**

```java
  
dependencies {

    implementation group: 'org.springframework.boot', name: 'spring-boot-starter-batch', version: '2.3.9.RELEASE'
    implementation group: 'org.springframework.integration', name: 'spring-integration-http', version: '5.2.11.RELEASE'
    implementation group: 'org.hibernate', name: 'hibernate-entitymanager', version: '5.5.6'

    //JDBC
    implementation('org.springframework.boot:spring-boot-starter-jdbc')
    //mybatis
    implementation 'org.mybatis.spring.boot:mybatis-spring-boot-starter:2.0.0'
    //jpa
    implementation group: 'org.springframework.data', name: 'spring-data-jpa', version: '2.4.11'

    runtime('com.h2database:h2')
    runtimeOnly 'org.mariadb.jdbc:mariadb-java-client'
    compileOnly('org.projectlombok:lombok')
    annotationProcessor('org.projectlombok:lombok')
    testCompile('org.springframework.boot:spring-boot-starter-test')
    testImplementation group: 'org.springframework.batch', name: 'spring-batch-test', version: '4.2.2.RELEASE'

}
```


## **실행 시 배치 자동 실행 끄기 옵션**


```java
spring:
  batch:
    job:
      enabled: false
```


# **Spring Meta Table**

Spring Batch에는 6개의 Meta Table과 3개의 Sequence Table이 존재합니다. 이는 Spring BatchJob이 실행 될 때마다 실행된 Job에 대한 다양한 정보들이 저장되게 됩니다.

일반적으로는 해당 Meta Table이 없이는 Spring Batch Framework를 실행시킬 수 없으나 이는 필요에 따라 커스터마이징을 통해 Meta Table이 없이도 실행되게 만들 수 있습니다.


(하지만 Spirng Batch에서 해당 Table이 없이 실행되지 않게 했다는 건 그만큼 중요한 정보들이 저장 된다는 것이겠죠?)


![[Spring  Batch6.png]]


# **SEQUENCE**

BATCH_JOB_INSTANCE, BATCH_JOB_EXECUTION및 BATCH_STEP_EXECUTION의 Primary Key는 시퀀스에 의해 생성됩니다. 다음은 Sequence를 지원하는 Database Create 식입니다.


```sql
CREATE SEQUENCE BATCH_STEP_EXECUTION_SEQ;
CREATE SEQUENCE BATCH_JOB_EXECUTION_SEQ;
CREATE SEQUENCE BATCH_JOB_SEQ;
```


Database에서 Sequence를 지원하지 않을 수도 있습니다. Mysql에선 하기 쿼리문을 사용하시면 됩니다.


```sql
CREATE TABLE BATCH_JOB_INSTANCE (
JOB_INSTANCE_ID BIGINT PRIMARY KEY ,
VERSION BIGINT,
JOB_NAME VARCHAR(100) NOT NULL ,
JOB_KEY VARCHAR(2500)
```

# **BATCH_JOB_INSTANCE**

BATCH_JOB_INSTANCE 테이블에는 JobInstance에 관련된 모든 정보가 포함되어 있습니다. 또한 해당 Table은 전체 계층 구조의 최상위 역할을 합니다


```sql
CREATE TABLE BATCH_JOB_INSTANCE (
JOB_INSTANCE_ID BIGINT PRIMARY KEY ,
VERSION BIGINT,
JOB_NAME VARCHAR(100) NOT NULL ,
JOB_KEY VARCHAR(2500)
);
```

# **BATCH_JOB_EXECUTION_PARAMS**

BATCH_JOB_EXECUTION_PARAMS 테이블에는 Job을 실행 시킬 때 사용했던 JobParameters에 대한 정보를 저장하고 있습니다.

```sql
CREATE TABLE BATCH_JOB_EXECUTION_PARAMS (
JOB_EXECUTION_ID BIGINT NOT NULL ,
TYPE_CD VARCHAR(6) NOT NULL ,
KEY_NAME VARCHAR(100) NOT NULL ,
STRING_VAL VARCHAR(250) ,
DATE_VAL DATETIME DEFAULT NULL ,
LONG_VAL BIGINT ,
DOUBLE_VAL DOUBLE PRECISION ,
IDENTIFYING CHAR(1) NOT NULL ,
constraint JOB_EXEC_PARAMS_FK foreign key (JOB_EXECUTION_ID)
references BATCH_JOB_EXECUTION(JOB_EXECUTION_ID)
);
```


# **BATCH_JOB_EXECUTION**

BATCH_JOB_EXECUTION테이블에는 JobExcution에 관련된 모든 정보를 저장하고 있습니다. JobExcution은 JobInstance가 실행 될 때마다 시작시간, 종료시간, 종료코드 등 다양한 정보를 가지고 있습니다.

```sql
CREATE TABLE BATCH_JOB_EXECUTION (
JOB_EXECUTION_ID BIGINT PRIMARY KEY ,
VERSION BIGINT,
JOB_INSTANCE_ID BIGINT NOT NULL,
CREATE_TIME TIMESTAMP NOT NULL,
START_TIME TIMESTAMP DEFAULT NULL,
END_TIME TIMESTAMP DEFAULT NULL,
STATUS VARCHAR(10),
EXIT_CODE VARCHAR(20),
EXIT_MESSAGE VARCHAR(2500),
LAST_UPDATED TIMESTAMP,
JOB_CONFIGURATION_LOCATION VARCHAR(2500) NULL,
constraint JOB_INSTANCE_EXECUTION_FK foreign key (JOB_INSTANCE_ID)
references BATCH_JOB_INSTANCE(JOB_INSTANCE_ID)
);
```

# **BATCH_STEP_EXECUTION**

BATCH_JOB_EXECUTION테이블에는 StepExecution에 대한 정보를 저장하고 있습니다. BATCH_JOB_EXECUTION 테이블과 여러 면에서 유사하며 STEP을 EXECUTION 정보인 읽은 수, 커밋 수, 스킵 수 등 다양한 정보를 추가로 담고 있습니다.



```sql
CREATE TABLE BATCH_STEP_EXECUTION (
STEP_EXECUTION_ID BIGINT PRIMARY KEY ,
VERSION BIGINT NOT NULL,
STEP_NAME VARCHAR(100) NOT NULL,
JOB_EXECUTION_ID BIGINT NOT NULL,
START_TIME TIMESTAMP NOT NULL ,
END_TIME TIMESTAMP DEFAULT NULL,
STATUS VARCHAR(10),
COMMIT_COUNT BIGINT ,
READ_COUNT BIGINT ,
FILTER_COUNT BIGINT ,
WRITE_COUNT BIGINT ,
READ_SKIP_COUNT BIGINT ,
WRITE_SKIP_COUNT BIGINT ,
PROCESS_SKIP_COUNT BIGINT ,
ROLLBACK_COUNT BIGINT ,
EXIT_CODE VARCHAR(20) ,
EXIT_MESSAGE VARCHAR(2500) ,
LAST_UPDATED TIMESTAMP,
constraint JOB_EXECUTION_STEP_FK foreign key (JOB_EXECUTION_ID)
references BATCH_JOB_EXECUTION(JOB_EXECUTION_ID)
);
```



# **BATCH_JOB_EXECUTION_CONTEXT**

BATCH_JOB_EXECUTION_CONTEXT테이블에는 JobExecution의ExecutionContext 정보가 들어있습니다.이 ExecutionContext 데이터는 일반적으로 JobInstance가 실패 시 중단된 위치에서 다시 시작할 수 있는 정보를 저장하고 있습니다.

```sql
CREATE TABLE BATCH_JOB_EXECUTION_CONTEXT (
JOB_EXECUTION_ID BIGINT PRIMARY KEY,
SHORT_CONTEXT VARCHAR(2500) NOT NULL,
SERIALIZED_CONTEXT CLOB,
constraint JOB_EXEC_CTX_FK foreign key (JOB_EXECUTION_ID)
references BATCH_JOB_EXECUTION(JOB_EXECUTION_ID)
);
```


# **BATCH_STEP_EXECUTION_CONTEXT**

BATCH_STEP_EXECUTION_CONTEXT테이블에는 StepExecution의 ExecutionContext 정보가 들어있습니다. 이 ExecutionContext 데이터는 일반적으로 JobInstance가 실패 시 중단된 위치에서 다시 시작할 수 있는 정보를 저장하고 있습니다.


```sql
CREATE TABLE BATCH_STEP_EXECUTION_CONTEXT (
STEP_EXECUTION_ID BIGINT PRIMARY KEY,
SHORT_CONTEXT VARCHAR(2500) NOT NULL,
SERIALIZED_CONTEXT CLOB,
constraint STEP_EXEC_CTX_FK foreign key (STEP_EXECUTION_ID)
references BATCH_STEP_EXECUTION(STEP_EXECUTION_ID)
);
```









---
출처-  https://khj93.tistory.com/entry/Spring-Batch%EB%9E%80-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B3%A0-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0

