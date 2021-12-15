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
- Accenture가 가지고 있던 아키텍처 프레임워크를 Batch 프로젝트에 기증함

### 배치 핵심 패턴
- Read - DB/File/Que 에서 다량의 데이터를 조회
- Process - 특정 방법으로 데이터를 가공
- Write - 데이터를 수정된 양식으로 다시 저장


    -> 스프링 배치는 핵심 패턴을 잘 설계하였기 때문에 사용한다.
    -> DB에서의 ETL과 유사

### 배치 시나리오
- 배치 프로세스를 주기적으로 커밋 : 대용량의 데이터를 한번에 커밋하지 않고 나누어 커밋
- 동시 다발적인 Job의 배치 처리, 대용량 병렬 처리 가능 : Job간의 간섭 X, 멀티스레드를 통한 대용량 처리
- 실패 후 수동 또는 스케쥴링에 의해 재시작
- 의존관계가 있는 step 여러 개를 순차적으로 처리
- 조건적 Flow 구성을 통한 체계적이고 유연한 배치 모델 구성
- 반복, 재시도, Skip 처리

### 아키텍처

* 3개의 Layer
  - Application
      - 개발자는 업무 로직에만 집중, 공통 기반기술은 프레임웍이 담당.
  - Batch Core
      - Job을 실행, 모니터링, 관리하는 API로 구성
      - JobLauncher, Job, Step, Flow 등이 속함.
  - Batch Infrastructure 
      - Application, Core 모두 공통 Infrastructure 위에서 빌드 됨.
      - Job 실행의 흐름과 처리를 위한 틀 제공
      - Reader, Processor Writer, Skip, Retry 등이 속함.
  



## 1.시작

- 스프링 배치 활성화
  - @EnableBatchProcessing
  - 총 4개의 설정 클래스를 실행시키며 스프링 배치의 모든 초기화 및 실행 구성이 이루어짐.
  - 스프링 부트 배치의 자동 설정 클래스가 생성됨으로 빈으로 등록된 모든 Job을 검색해서 초기화와 동시에 Job을 수행하도록 구성됨.

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
    - 하나의 배치 Job을 정의하고 빈 설정
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
- Job, Step, JobPrameters .. 등의 도메인 정보들을 저장, 업데이트, 조회 할 수 있는 스키마 제공
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

### Job
- 기본 개념
    - 배치 계층 구조에서 가장 상위에 있는 개념
    - 배치작업을 어떻게 구성하고 실행할 것인지 전체적으로 설정하고 명세해 놓은 객체
    - 배치 Job을 구성하기 위한 최상위 인터페이스이며 기본 구현체를 스프링 배치가 제공
    - 여러 Step(1개 이상)을 포함하고 있는 컨테이너 역할
- 기본 구현체
    - SimpleJob
        - 순차적으로 Step 을 실행시키는 Job
    - FlowJob
        - 특정 조건과 흐름에 따라 Step 을 구성하여 실행시키는 Job
        - Flow 객체를 실행시켜 작업을 진행
    
- Job
- JobInstance
- JobParameters
- JobExecution


---

## 출처
- https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EB%B0%B0%EC%B9%98
