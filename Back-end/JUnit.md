
# JUnit
## JUnit5

### 특징
- JAVA 8+ ver. 필요 (JUnit4 = JAVA 5+ ver.)
- Spring Boot 2.2+ ver. 기본 의존성 추가
- JUnit5 = JUnit Platform + Jupiter + Vintage
	- Platform : JVM에서 Testing Framework를 launching 해주는 기반, TestEngine API 가 정의되어있음( 테스트 실행, 발견 및 보고이 있는 모든 확장 프레임 워크 포괄)
	- Jupiter : TestEngin API 구현체, JUnit5 제공
	- Vintage : TestEngin API 구현체, JUnit3, 4 제공 ( 역 호환성 허용 )

### JUnit4 vs JUnit5
- JUnit5는 여러 테스트 러너를 동시에 작업 가능
- JUnit5는 Java8 기능 활용 (JUnit4는 Java 8을 넘어선 고급 기능 없음)
- JUnit5는 전체 프레임워크(단일 jar)가 아닌 세분화된 필요한 것만(모듈 3개) 가져올 수 있음.(JUnit4는 All in One)
- 달라진 문법 (참고 : https://pureainu.tistory.com/190 )
---
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
- **assertEqulas(expected, actual, {error msg})** : 실제값 == 기대값 ( + 오류 메시지 = lambda 표현식 사용 추천_에러 시에만 호출됨.)
- **assertNotNull(actual)** : 값이 null이 아닌지 확인
- **assertTrue(boolean)** : 다음 조건이 참(true)인지 확인
- **assertAll(executables...)** :  괄호 안 모든 테스트 구문 수행
- **assertThrows(expectedType, executable)** : 예외 발생 확인, exception을 Return - 내부 오류 메시지와 기대되는 메시지 비교 가능
- **assertTimeout(duration, executable)** : 특정 시간 안에 실행이 완료되는지 확인, 설정한 Timeout과 관계없이 executable이 끝날 때 까지 수행됨. 
->  **assertTimeoutPreemptively** : 설정한 Timeout 까지만 수행 후 종료 (ThreadLocal 사용 시 주의, assertTimeout 사용이 더 안정적임)

---
## 조건문
### Assume
- **assumeTrue(expected, actual)**  : 해당 조건을 통과해야지 다음 수행. ( 환경변수에 따른 테스트문 수행 여부 선택 시 이용. 환경 변수 = System.getenv("TEST_ENV") )
- **assumingThat( 조건문, () -> { } )** : 조건 만족 시 함수 수행

### Annotation 활용
- **@EnabledOnOs/@DisabledOnOs({OS.MAC, OS.LINUX .. })** : OS별 조건문
- **@EnabledOnJre/@DisabledOnJre({JRE.JAVA_8, JRE.JAVA_9, JRE.OTHER...})** : JRE 버전 체크
- **@EnabledIfEnvironmentVariable(named = "TEST_ENV", matches = "local")** : 환경변수 조건 체크

---
### Tag
- @Tag("Msg") : 테그 별 테스트 수행 가능 ( @Tag("fast"), @Tag("slow") )
	- InteliJ > Edit Configuration > Test kind > Tags > Tag expression
	- 빌드 설정 > pom.xml > profiles  > build > plugins >  configuration : groups = fast | slow 
	(표현식 참고 : https://junit.org/junit5/docs/current/user-guide/#running-tests-tag-expressions)
		- ./mvnw test(default)로 실행 ( ./mvnw test -P ci 전체 테스트 수행)
### Custom Tag
- Tag Annotation을 사용자 설정하여 사용
- Tag를 클래스화 시킨다.
	- ex) FastTest 클래서 생성(@FastTest Annotation)
		@Target(ElementType.METHOD) : 메서드를 대상으로 Annotation 사용
		@ Retention(RetentionPolicy.RUNTIME) : 런타임에 유효
		@ Test
		@ Tag("fast")
		public @interface FastTest {	}
	- TEST Source > @FastTest(클래스명)으로 대체하여 사용
- 장점 : 오타를 줄일 수 있음. 중복 제거.
---

## 테스트 반복
- **@RepeatedTest(value = 10, name = "{displayName}, {currentRepetition}/{totalRepetitions} )** : 반복 횟수, 테스트 이름, 현재 반복 수, 전체 반복 수
- void repeatTest(**RepetitionInfo** repetitionInfo) 
	- repetitionInfo.getCurrentRepitition() : 현재 반복된 횟수

- **@ParameterizedTest** (name = "{index} {displayName} message={0}" : 파라미터를 통한 테스트(테스트마다 값 변경 테스트)
**@ValueSource**(strings = {"날씨가", "많이", "추워지고", "있네요."})
**@EmptySource** : 비어있는 문자열 추가로 전달
**@NullSource** : null 값 추가로 전달 (@NullAndEmptySource : null&empty)
void parameterizedTest(String message) { print(message) }

- **SimpleArgumentConverter** : 인자값 커스텀하게 변환
	- @Override **function convert** 필요
	- @ConvertWith 로 인자값 앞에 선언하여 형변환 (하나의 인자값 받을 때 사용) 



- **@CvsSource({"10, '자바 스터디'", "20, 스프링"})** : 여러 인자 전달
	- **ArgumentsAccessor** argumentsAccessor : 인자값 접근 클래스
		- argumentsAccessor.getInteger(0), argumentsAccessor.getString(1)
	- **ArgumentsAggregator(Interface)** : 여러 인자값 형변환
		- @Override **aggregateArguments**( ) : 함수 구현 필요
		- **@AggregateWith**(xxxAggregator.class) 로 여러 인자값 변형 가능.
---
### TIP
- lambda 표현식 = 호출 할 때만 수행
- InteliJ 단축키
	- ctrl + shift + T : 해당 클래스 테스트 생성
	- ctlrl + P : 파라미터 정보 확인 가능
- Spring Boot 2.x -> AssertJ, Hamcrest 포함 (JUnit 변형 된 라이브러리)

### 예제
-  https://github.com/keesun/inflearn-the-java-test (출처 : 백기선 git)
## 출처
- https://junit.org/junit5/docs/current/user-guide/
- https://docs.google.com/document/d/1j6mU7Q5gng1mAJZUKUVya4Rs0Jvn5wn_bCUp3rq41nQ/edit#
