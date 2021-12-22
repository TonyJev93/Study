# DDD (Domain Driven Design)

## 용어 정리
- Domain : 현실세계
- Model : 현실세계를 추상화
- Entity : 식별자를 가지며 영속성이 필요한 객체
- Value Object
    - 식별자가 없고, 영속성이 필요 없는 객체 
    - 수정할 필요없고, 필요한 만큼 복제하여 전달하여 사용
- Service 
    - 특정 엔터티나 값 객체에 속할 수 없는 도메인의 개념 표현 
    - 주로 여러 객체에 걸쳐서 일어나는 행위를 담당, 상태정보를 관리하지 않음
- Aggregate
    - 객체의 소유권과 경계를 정의
    - 데이터를 변경할 때 하나의 단위로 간주되는 관련된 객체들의 집합
    - 외부에서 접근 가능하도록 하는 창구인 root 를 가짐
- Factory : 복잡한 객체 생성의 절차를 캡슐화
- Repository : 객체의 저장 담당

----
- Bounded Context
    - 모델이 적용되는 컨텍스트를 명시적으로 정의
    - 각 모델을 명확하게 구분하기 위해 가상의 경계선을 둘러 명확하게 분리
    - 분할된 컨텍스트의 특징은 특정한 문맥에 따라 도메인의 요소를 해석
- Context Map
    - 분할된 컨텍스트 간의 관계 표현
- 공유 커널
    - 양쪽 컨텍스트에서 명확하게 공유되는 부분 정의
    - 양쪽 팀이 공동으로 수정에 대한 책임 및 테스트에 대한 책임을 지도록 하는 패턴


## 계층형 아키텍처
- User Interface (Presentation Layer)
    - 사용자에게 정보를 보여주는 책임 
    - 사용자의 명령을 해석하는 책임
- Application Layer
    - 어플리케이션 활동을 조율하는 레이어
    - 업무 로직을 포함하지 않음
    - 비즈니스 객체의 상태를 보관하지 않지만 어플리케이션 작업의 처리 상태 보관
- Domain Layer
    - 도메인 정보 포함
    - 비즈니스 객체의 상태 포함
    - 비즈니스 객체와 이 객체의 상태 정보 중 가능한 부분의 영속성에 대한 책임은 인프라스트럭처 레이어로 위임
    - 도메인 모델(엔티티, 값 객체), 도메인 서비스 등 리포지토리 인터페이스도 여기에 속함
- Infrastructure Layer
    - 다른 레이어 모두를 지원하는 라이브러리
    - 레이어간의 통신을 제공하고 비즈니스 객체의 영속성 구현



## 장점
- Easy Communication
- Improves Flexibility
- Emphasizes Domain Over Interface

# 적용

## Event Storming
- 정의 : 도메인 지식을 공유함으로써 전체 비즈니스가 어떻게 돌아가는지 한눈에 볼 수 있는 지도(타임라인)를 만드는 workshop 형태의 행위
- 방법 : 포스트잇과 화이트보드를 사용하여 진행하며, 모든 사람이 적극적으로 참여

### 순서
1. Big Picture
    - 메인 Event
    - 외부 시스템
2. Process Level
    - Command
    - Policy
    - Read Model
3. Design Level
    - Aggregate

## Event Sourcing & CQRS
### Event Sourcing 
- 어플리케이션의 모든 상태변화를 순서대로 이벤트로 보관하여 처리하는 개념
- 이벤트는 시간 순으로 기록 되며, 각 서비스는 자신이 필요한 이벤트만 최신의 상태로 유지하면 됨

### CQRS(Command Query Responsibility Segregation)
- Command 와 Query 분리
    - Command : CUD
    - Query : R

## Event Sourcing <-> CQRS
1. Client -> Command API : Command 요청, Event Store 에 결과 저장
2. Event Exchange : Event Store -> View Store 이관
3. Client -> Query API : Query 요청, View Store 에서 결과 조회


## 참고
- 강연 내용 : [DDD(Domain-Driven Design) 시작하기](https://www.youtube.com/watch?v=td5VRmxntmw&t=2160s)