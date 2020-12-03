# JUnit
## JUnit5

### 특징
- JAVA 8+ ver. 필요 (JUnit4 = JAVA 5+ ver.)
- Spring Boot 2.2+ ver. 기본 의존성 추가
- JUnit5 = JUnit Platform + Jupiter + Vintage
	- Platform : JVM에서 Testing Framework를 launching 해주는 기반, TestEngine API 가 정의되어있음( 테스트 실행, 발견 및 보고이 있는 모든 확장 프레임 워크 포괄)
	- Jupiter : TestEngin API 구현체, JUnit5 제공
	- Vintage : TestEngin API 구현체, JUnit3, 4 제공 ( 역 호환성 허용 )

### JUnit4 VS JUnit5
- JUnit5는 여러 테스트 러너를 동시에 작업 가능
- JUnit5는 Java8 기능 활용 (JUnit4는 Java 8을 넘어선 고급 기능 없음)
- JUnit5는 전체 프레임워크(단일 jar)가 아닌 세분화된 필요한 것만(모듈 3개) 가져올 수 있음.(JUnit4는 All in One)
- 달라진 문법 (참고 : https://pureainu.tistory.com/190 )


## 출처
- https://junit.org/junit5/docs/current/user-guide/
- https://docs.google.com/document/d/1j6mU7Q5gng1mAJZUKUVya4Rs0Jvn5wn_bCUp3rq41nQ/edit#
