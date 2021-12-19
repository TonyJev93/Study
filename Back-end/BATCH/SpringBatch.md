# Spring Batch
## 목차

0. 개요
1. 시작
2. 도메인 이해

----

## 0.개요

### 탄생 배경
- 자바 기반 표준 배치 기술 부재(JSR)
- ex) IO, Network(TCP, UDP), Thread, JDBC ...
- SpringSource(현 Pivotal)와 Accenture(경영 컨설팅 기업)의 합작품
- Accenture 가 가지고 있던 아키텍처 프레임워크를 Batch 프로젝트에 기증함

### 배치 핵심 패턴
- Read - DB/File/Que 에서 다량의 데이터를 조회
- Process - 특정 방법으로 데이터를 가공
- Write - 데이터를 수정된 양식으로 다시 저장


    -> 스프링 배치는 핵심 패턴을 잘 설계하였기 때문에 사용한다.
    -> DB에서의 ETL과 유사

### 배치 시나리오
- 배치 프로세스를 주기적으로 커밋 : 대용량의 데이터를 한번에 커밋하지 않고 나누어 커밋
- 동시 다발적인 Job 의 배치 처리, 대용량 병렬 처리 가능 : Job 간의 간섭 X, 멀티스레드를 통한 대용량 처리
- 실패 후 수동 또는 스케쥴링에 의해 재시작
- 의존관계가 있는 step 여러 개를 순차적으로 처리
- 조건적 Flow 구성을 통한 체계적이고 유연한 배치 모델 구성
- 반복, 재시도, Skip 처리

### 아키텍처

* 3개의 Layer
  - Application
      - 개발자는 업무 로직에만 집중, 공통 기반기술은 프레임웍이 담당.
  - Batch Core
      - Job 을 실행, 모니터링, 관리하는 API 로 구성
      - JobLauncher, Job, Step, Flow 등이 속함.
  - Batch Infrastructure 
      - Application, Core 모두 공통 Infrastructure 위에서 빌드 됨.
      - Job 실행의 흐름과 처리를 위한 틀 제공
      - Reader, Processor Writer, Skip, Retry 등이 속함.
  



## 1.시작

- 스프링 배치 활성화
  - @EnableBatchProcessing
  - 총 4개의 설정 클래스를 실행시키며 스프링 배치의 모든 초기화 및 실행 구성이 이루어짐.
  - 스프링 부트 배치의 자동 설정 클래스가 생성됨으로 빈으로 등록된 모든 Job 을 검색해서 초기화와 동시에 Job 을 수행하도록 구성됨.

- 스프링 배치 초기화 설정 클래스
  1. BatchAutoConfiguration
      - 스프링 배치 초기화 시 자동으로 실행되는 설정 클래스(Auto Configuration)
      - Job 을 수행하는 JobLauncherApplicationRunner 빈을 생성
  1. SimpleBatchConfiguration
      - JobBuilderFactory 와 StepBuilderFactory 생성
      - 스프링 배치의 주요 구성 요소 생성 - 프록시 객체로 생성됨
  1. BatchConfigurerConfiguration
    - BasicBatchConfigurer
        - SimpleBatchConfiguration 에서 생성한 프록시 객체의 실제 대상 객체를 생성하는 설정 클래스
        - 빈으로 의존성 주입 받아서 주요 객체들을 참조해서 사용할 수 있다
    - JpaBatchConfigurer
      - JPA 관련 객체를 생성하는 설정 클래스
    - 사용자 정의 BatchConfigurer 인터페이스를 구현하여 사용할 수 있음
  

- 수행 순서
```
   @EnableBatchProcessing
-> SimpleBatchConfiguration
-> BatchConfigurerConfiguration
    - BasicBatchConfigurer
    - JpaBatchConfigurer
-> BatchAutoConfiguration
```

### 배치 시작하기
* @Configuration 선언
    - 하나의 배치 Job 을 정의하고 빈 설정
* JobBuilderFactory
    - Job 을 생성하는 빌더 팩토리
* StepBuilderFactory
    - Step 을 생성하는 빌더 팩토리
* Job
    - 일, 일감
* Step
    - 일의 항목, 단계
* tasklet
    - Step 안에서 단일 태스크로 수행되는 로직 구현
    - 작업 내용
    - Step 내에서 기본적으로 무한 반복 됨. (return null = 1회 수행 = RepeatStatus)
* Job 구동 -> Step 실행 -> Tasklet 실행


### DB 스키마 생성
#### 스프링 배치 메타 데이터
- 배치의 실행 및 관리를 위한 목적
- Job, Step, JobParameters .. 등의 도메인 정보들을 저장, 업데이트, 조회 할 수 있는 스키마 제공
- 과거, 현재의 실행에 대한 상세한 정보, 성공/실패 여부 등 관리
- DB 연동 시 필수적으로 메타 테이블 생성이 되어야 함

#### 스키마 생성 설정
- 수동 생성 : 쿼리 복사 후 직접 실행
- 자동 생성 : spring.batch.jdbc.initialize-schema
    - ALWAYS
        - 스크립트 항상 실행
        - RDBMS 설정이 되어 있을 경우 내장 DB 보다 우선적으로 실행
    - EMBEDDED(default)
        - 내장 DB일 때만 자동 실행 됨.
    - NEVER
        - 스크립트 항상 실행 안함.
        - 내장 DB 일 경우 스크립트가 생성이 되지 않아 오류 발생
        - **운영에서 수동으로 스크립트 생성 후 설정하는 것을 권장**

--- 

## 2. 도메인 이해
### 목록
1. Job
1. Step
1. ExecutionContext
1. JobRepository / JobLauncher

## Job
- 기본 개념
    - 배치 계층 구조에서 가장 상위에 있는 개념
    - 배치작업을 어떻게 구성하고 실행할 것인지 전체적으로 설정하고 명세해 놓은 객체
    - 배치 Job 을 구성하기 위한 최상위 인터페이스이며 기본 구현체를 스프링 배치가 제공
    - 여러 Step(1개 이상)을 포함하고 있는 컨테이너 역할
- 기본 구현체
    - SimpleJob
        - 순차적으로 Step 을 실행시키는 Job
    - FlowJob
        - 특정 조건과 흐름에 따라 Step 을 구성하여 실행시키는 Job
        - Flow 객체를 실행시켜 작업을 진행

### JobInstance
- 기본 개념
    - Job 이 실행될 때마다 생성되는 고유 식별 작업 실행을 나타냄
    - Job 의 설정과 구성은 동일하지만 Job 이 실행되는 시점에 처리하는 내용이 다르기 때문에 Job 의 실행을 구분해야 함
    - JobInstance 생성 및 실행
        - 처음 시작하는 Job + JobParameter 일 경우 새로운 JobInstance 생성
        - 이전과 동일한 Job + JobParameter 로 실행 시 기존 JobInstance 리턴
            - 내부적으로 JobName + JobKey(JobParameters 해시값)을 가지고 JobInstance 객체를 DB로 부터 얻음.
    - Job 과 1:M 관계 
- BATCH_JOB_INSTANCE 테이블과 매핑
    - JOB_NAME(Job) 과 JOB_KEY(JobParameter 해시값) 가 동일한 데이터는 중복해서 저장할 수 없음  

### JobParameters
- 기본 개념
    - Job 을 실행할 때 함께 포함되어 사용되는 파라미터를 가진 도메인 객체
    - 하나의 Job 에 존재할 수 있는 여러개의 JobInstance 를 구분하기 위한 용도
    - JobParameter 와 JobInstance 는 1:1 관계
- 생성 및 바인딩
    - 어플리케이션 실행 시 주입
        - java -jar LogBatch.jar requestDate=20210101
    - 코드로 생성
        - JobParameterBuilder, DefaultJobParametersConverter
    - SpEL 이용
        - @Value("#{jobParameter[requestDate]}")
        - @JobScope, @StepScope 선언 필수
- BATCH_JOB_EXECUTION_PARAM 테이블과 매핑
    - JOB_EXECUTION 과 1:M 의 관계
- Parameter 타입
    - STRING
    - DATE
    - LONG
    - DOUBLE

### JobExecution
- 기본 개념
    - JobInstance 에 대한 한번의 시도를 의미하는 객체
    - Job 실행 중에 발생한 정보들을 저장하고 있는 객체
        - 시작시간, 종료시간, 상태(시작됨, 완료, 실패), 종료상태
    - JobInstance 와의 관계
        - JobExecution 은 "FAILED" or "COMPLETED" 등의 실행상태를 가지고 있음.
        - JobExecution 실행상태가 "COMPLETED"인 경우 JobInstance 실행이 완료된 것으로 간주하여 재 실행 불가.
        - "FAILED"면 JobInstance 실행이 완료되지 않은 것으로 간주하여 재실행이 가능함.
        - "COMPLETED" 될 때까지 하나의 JobInstance 내에서 여러 번 시도 가능.
- BATCH_JOB_EXECUTION 테이블 매핑
    - JobInstance 와 JobExecution 는 1:M 관계
    - JobInstance 에 대한 성공/실패 내역을 가짐
    

## Step
- 기본 개념
    - Batch Job 을 구성하는 독립적인 하나의 단계(Step 간에 간섭 X)
    - 실제 배치 처리를 정의하고 컨트롤하는 데 필요한 모든 정보를 가지고 있는 도메인 객체
    - 단순한 단일 테스크 뿐 아니라 입력과 처리, 출력과 관련된 복잡한 비즈니스 로직을 포함하는 모든 설정들을 담고 있음.
    - 배치작업을 어떻게 구성할 것인지, Job 의 세부 작업을 Task 기반으로 설정하고 명세해 놓은 객체
    - 모든 Job 은 하나 이상의 Step 으로 구성됨
- 기본 구현체
    - TaskletStep
        - 가장 기본이 되는 클래스로서 Tasklet 타입의 구현체들을 제어
    - PartitionStep
        - 멀티 스레드 방식으로 Step 을 여러 개로 분리해서 실행.
    - JobStep
        - Step 내에서 Job 을 실행하도록 한다.
    - FlowStep
        - Step 내에서 Flow 를 실행하도록 한다.
    
### StepExecution
- 기본 개념
    - Step 에 대한 한번의 시도를 의미하는 객체
    - Step 실행 중 발생한 정보들을 저장하고 있는 객체
        - 시작시간, 종료시간, 상태(시작됨, 완료, 실패), commit count, rollback count 등
    - Step 이 매번 시도될 때마다 생성되며 각 Step 별로 생성된다.
    - Job 이 재시작 하더라도 이미 성공적으로 완료된 Step 은 재 실행되지 않고 실패한 Step 만 실행된다.(옵션으로 바꿀수는 있음)
    - 이전 단계 Step 이 실패한 경우 현재 단계 Step 에 대한 StepExecution 은 생성 생성되지 않는다.
    - JobExecution 과의 관계
        - Step 의 StepExecution 이 모두 정상적으로 완료 되어야 JobExecution 이 정상적으로 완료된다.
        - StepExecution 중 하나라도 실패 시 JobExecution 도 실패
- BATCH_STEP_EXECUTION 테이블과 매핑
    - JobExecution 와 StepExecution 는 1:M 의 관계
    - 하나의 Job 에 여러 개의 Step 으로 구성했을 경우 각 StepExecution 은 하나의 JobExecution 을 부모로 가진다.
    
### StepContribution
- 기본 개념
    - 청크 프로세스의 변경 사항을 버퍼링 한 후 StepExecution 상태를 업데이트하는 도메인 객체
    - 청크 커밋 직전에 StepExecution 의 apply 메서드를 호출하여 상태를 업데이트 함
    - ExitStatus 의 기본 종료코드 외 사용자 정의 종료코드를 생성해서 적용 할 수 있음
    
## ExecutionContext
- 기본 개념
    - 프레임워크에서 유지 및 관리하는 키/값으로 된 컬렉션으로 StepExecution 또는 JobExecution 객체의 상태(state)를 저장하는 공유 객체
    - DB 에 직렬화 한 값으로 저장됨 - {"key" : "value:"}
    - 공유 범위
        - Step 범위 - 각 Step 의 StepExecution 에 저장되며 Step 간 서로 공유 안됨
        - Job 범위 - 각 Job 의 JobExecution 에 저장되며 Job 간 서로 공유 안되며 해당 Job 의 Step 간 서로 공유됨
    - Job 재 시작시 이미 처리한 Row 데이터는 건너뛰고 이 후로 수행하도록 할 때 상태 정보를 활용한다.
- 구조
    - Map<String, Object> map = new ConcurrentHashMap
    - 유지, 관리에 필요한 키값 설정
- BATCH_JOB_EXECUTION_CONTEXT
    - job_execution_id
    - short_context : 공유 정보 Map
    - serialized_context
- BATCH_STEP_EXECUTION_CONTEXT
    - step_execution_id
    - short_context : 공유 정보 Map
    - serialized_context
    
### JobRepository
- 기본 개념
    - 배치 작업 중의 정보를 저장하는 저장소 역할
    - Job 이 언제 수행되었고, 언제 끝났으며, 몇 번이 실행되었고 실행에 대한 결과 등의 배치 작업의 수행과 관련된 모든 meta data 를 저장함
        - JobLauncher, Job, Step 구현체 내부에서 CRUD 기능을 처리함
 - JobRepository 설정
    - @EnableBatchProcessing 어노테이션만 선언하면 JobRepository 가 자동으로 빈으로 생성됨
    - BatchConfigurer 인터페이스를 구현하거나 BasicBatchConfigurer 를 상속해서 JobRepository 설정을 커스터마이징 할 수 있다.
        - JDBC 방식으로 설정 - JobRepositoryFactoryBean
            - 내부적으로 AOP 기술을 통해 트랜잭션 처리를 해주고 있음
            - 트랜잭션 isolation 의 기본값은 SERIALIZABLE 로 최고 수준, 다른 레벨(READ_COMMITTED, REPEATABLE_READ)로 지정 가능
              - [참고](https://joont92.github.io/db/%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%98-%EA%B2%A9%EB%A6%AC-%EC%88%98%EC%A4%80-isolation-level/)
            - 메타테이블의 Table Prefix 를 변경할 수 있음(기본 값 : "BATCH_")
    - In Memory 방식으로 설정 - MapJobRepositoryFactoryBean
        - 성능 등의 이유로 도메인 오브젝트를 굳이 데이터베이스에 저장하고 싶지 않을 경우
        - 보통 Test 나 프로토타입의 빠른 개발이 필요할 때 사용

### JobLauncher
- 기본 개념
    - 배치 Job 을 실행시키는 역할
    - Job 과 Job Parameters 를 인자로 받으며 요청된 배치 작업을 수행한 후 최종 client 에게 JobExecution 을 반환함
    - 스프링 부트 배치가 구동되면 JobLauncher 빈이 자동 생성 됨
    - Job 실행
        - JobLauncher.run(Job, JobParameters)
        - 스프링 부트 배치에서는 JobLauncherApplicationRunner 가 자동적으로 JobLauncher 을 실행
        - 동기적 실행
            - taskExecutor 를 SyncTaskExecutor 로 설정할 경우 (기본값 : SyncTaskExecutor)
            - JobExecution 을 획득하고 배치 처리를 **최종 완료한 이후** Client 에게 JobExecution 을 반환
            - 스케줄러에 의한 배치처리에 적합 - 배치처리시간이 길어도 상관없는 경우
        - 비동기적 실행
            - taskExecutor 가 SimpleAsyncTaskExecutor 로 설정할 경우
            - JobExecution 을 획득한 후 Client 에게 **바로** JobExecution 을 반환하고 배치처리를 완료한다
            - HTTP 요청에 의한 배치처리에 적합 - 배치처리 시간이 길 경우 응답이 늦어지지 않도록 함
- 구조
    - JobLauncher
        - JobExecution run(Job, JobParameters)
    

---

## 출처
- 인프런 : [스프링 배치 - Spring Boot 기반으로 개발하는 Spring Batch](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EB%B0%B0%EC%B9%98)
