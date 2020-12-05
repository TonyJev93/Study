
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

### Annotation

* **@Test** : Test Class에 선언, Main함수 없이도 테스트 러너 동작
* **@BeforeAll** : 테스트 시작 시 최초 한 번 수행
* **@AfterAll** : 테스트 끝나기 전 한 번 수행
* **@BeforeEach** : 모든 테스트의 시작 전 한 번 수행
* **@AfterEach** : 모든 테스트의 끝에 한 번 수행
* **@Disabled** : 해당 테스트 비활성화


> 테스트 이름표시


* **@DisplayNameGeneration** : Method와 Class 레퍼런스를 사용하여 테스트 이름 표기하는 방법 설정
(기본 구현체 :  ReplaceUnderscores 제공 = 이름에 밑줄을 공백으로 변경)
* **@DisplayName** : 테스트 이름 쉽게 표현, 이모지 추가 가능 (@DisplayNameGeneration 보다 우선순위 높음)
* **참고** : **[https://junit.org/junit5/docs/current/user-guide/#writing-tests-display-names](https://junit.org/junit5/docs/current/user-guide/#writing-tests-display-names)**

### Assertion
- **assertEqulas(expected, actual)** : 실제값 == 기대값
- **assertNotNull(actual)** : 값이 null이 아닌지 확인
- **assertTrue(boolean)** : 다음 조건이 참(true)인지 확인
- **assertAll(executables...)** : 모든 확인 구문 확인
- **assertThrows(expectedType, executable)** : 예외 발생 확인
- **assertTimeout(duration, executable)** : 특정 시간 안에 실행이 완료되는지 확인


### 예제
-  https://github.com/keesun/inflearn-the-java-test (출처 : 백기선 git)
## 출처
- https://junit.org/junit5/docs/current/user-guide/
- https://docs.google.com/document/d/1j6mU7Q5gng1mAJZUKUVya4Rs0Jvn5wn_bCUp3rq41nQ/edit#
