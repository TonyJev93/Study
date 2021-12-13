# Spring Batch
## 목차

0. 개요
1. 시작

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

---

## 출처
- https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EB%B0%B0%EC%B9%98
