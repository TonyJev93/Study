


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
## 테스트 인스턴스
- 테스트 마다 클래스 인스턴스가 새로 생김
	- 이유 : 테스트 간 **의존성을 제거하기 위함.** 실행 순서에 영항 없도록
		-  테스트는 선언 순서대로 수행된다는 보장 없음. 
		- JUnit5는 선언된 순서로 수행되는게 일반적.
	- 해결방법
		- **@TestInstance(TestInstance.Lifecycle.PER_CLASS)** : 클래스마다 인스턴스 하나 생성 > 클래스 위에 선언하면 클래스 내에서 하나의 인스턴스를 공유하여 사용
		- @BeforeAll / @AfterAll = static method 선언 필요, 그러나 @TestInstance 선언 시 static일 필요가 없음.

---
## 테스트 순서
- 특정 정해진 수행 순서가 있음 -> 그러나 순서에 의존하면 안 됨. 언제나 바뀔 수 있도록 해야 함.
(단위 테스트는 다른 단위테스트와 독립적으로 수행 가능해야 함.)

- 때로는 원하는 순서대로 테스트가 필요할 때가 있음.
	- @TestInstance 활용하여 흐름 테스트 가능(상태공유)
	- **@TestMethodOrder(MethodOrderer class 구현체)**
		- ex) @TestMethodOrder(MethodOrderer.OrderAnnotation.class)
				**@Order**(1) <- Spring 제공하는 Annotation말고 JUnit 사용
				**@Order**(2) ... : 테스트 위에 수행 순서 명시

---
## JUnit 5 설정파일
- 경로 : test > resources > junit-platform.properties
- inteliJ 에서 테스트 클래스페스 설정 > Modules > resources = Test Resources 설정
- 활용
	- junit.jupiter.**testinstance.lifecycle.default** = per_class (전체 적용)
	- junit.jupiter.**extensions.autodetection.enabled** = true : 자동 extension 등록, 확장팩 자동 감지
	-  junit.jupiter.**conditions.deactivate** = org.junit.*DisabledCondition : @Disabled 무시하고 실행
	- junit.jupiter.**displayname.generator.default** = \\ <-줄바꿈
	  org.junit.jupiter.api.DisplayNameGenerator$ReplaceUndersources
		- 참고로 DisplayName이 우선순위 더 높음
---
## 확장 모델
- JUnit 4의 확장모델 : @RunWith(Runner), TestRule, MethodRule
- JUnit 5의 확장모델 : Extension 단 하나
- ex) FindSlowTestExtension : 실행이 Slow인 테스트 찾는 Extension
	- **implements BeforeTestExecutionCallback, AfterTestExecutionCallback** 
	- @Override **beforeTestExecution / afterTestExeucution** **(ExtensionContext context)**
		- SlowTest annotation = **context.getRequiredTestMethod().getAnnotation**(SlowTest.class)
		- String testClassName = **context.getRequiredTestClass().getName**()
		- String testMethodName = **context.getRequiredTestMethod().getName**()
		- **ExtensionContext.Store** store = **context.getStore(ExtensionContext.Namespace.create**(testClassName, testMethodName))
		- Before : start_time = store.put("START_TIME", System.currentTimeMillis())
		- After : start_time = store.remove("START_TIME", long.class)
			- duration = curr - start_time
			- if(duration > THRESHOLD && annotation === null) : 시간 초과 + SlowTest annotation이 없는 경우 출력하도록
- **등록 방법**
	 1. **선언적 등록** : @ExtendWith(FinSlowTestExtension.class)
	 2. **프로그래밍 등록** : @RegisterExtension > static XxxExtension oo = new XxxExtension( .. );
	 3. **자동등록 자바 ServiceLoader 이용** (설정파일 등록) : junit.jupiter.extensions.autodetection.enabled= true 

---

### TIP
- lambda 표현식 = 호출 할 때만 수행
- InteliJ 단축키
	- ctrl + shift + T : 해당 클래스 테스트 생성
	- ctlrl + P : 파라미터 정보 확인 가능
- Spring Boot 2.x -> AssertJ, Hamcrest 포함 (JUnit 변형 된 라이브러리)
- JUnit 4 마이그래이션 
	- Vintage 의존성 추가 필요, Jupiter와 Vintage를 구분하여 테스트에 보여줌.
	- @Rule은 기본적으로 지원하지 않음.(@EnableRuleMigrationSupport를 사용하면 ExtenalResource, Verifier, ExpectedExeption 지원) <- **junit-jupiter-migrationsupport 모듈**이 제공
	
	
### 예제
-  https://github.com/keesun/inflearn-the-java-test (출처 : 백기선 git)
## 출처
- https://junit.org/junit5/docs/current/user-guide/
- https://docs.google.com/document/d/1j6mU7Q5gng1mAJZUKUVya4Rs0Jvn5wn_bCUp3rq41nQ/edit#
