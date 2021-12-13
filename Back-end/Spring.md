# Spring

## 스프링 핵심 원리 ( inflearn 김영한 강의 )

- IoC, DI, 컨테이너, SOLID(SRP, OCP, DIP)
- 객체 지향 프로그래밍


## 스프링이 란?

- **스프링 프레임워크** / **부트** / 데이터 / 세션 / 시큐리티 / RestDocs / 배치 / 클라우드 ...

### 스프링 프레임워크
- 핵심 기술 : 스프링 DI 컨테이너, AOP, 이벤트, 기타

### 스프링 부트
- 스프링을 편리하게 사용할 수 있도록 지원, 최근에는 기본으로 사용
- 장점 
    - 단독으로 실행할 수 있는 스프링 애플리케이션을 쉽게 생성(Tomcat 같은 웹서버 별도 설치 X)
    - starter 종속성 제공
    - 스프링과 3rd parth(외부) 라이브러리 자동 구성(상호 버전 관리)
    - 관례에 의한 간결한 설정
    - 모니터링 제공

### 스프링 단어?
- 스프링이라는 단어는 문맥에 따라 다르게 사용된다.
    - 스프링 DI 컨테이너 기술
    - 스프링 프레임워크
    - 스프링 부트, 스프링 프레임워크 등을 모두 포함한 스프링 생태계
    
## 만든 이유?
- 핵심 개념
    - 이 기술을 왜 만들었는가?
    - 이 기술의 핵심 컨셉은?

- 스프링의 진짜 핵심
    - 자바 언어 기반의 프레임워크 -> 객체 지향 언어 -> 좋은 객체 지향 애플리케이션을 개발할 수 있도록 도와주는 프레임워크
    
### 객체 지향 프로그래밍
- 객체들의 모임, 객체간 메시지를 주고 받는다, 유연, 변경 용이

#### 다형성 (유연, 변경 용이)
- 역할과 구현을 구분