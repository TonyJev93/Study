# Testcontainers

## 소개

- Unit Test 중 컨테이너를 제어하는데 도움을 주는 라이브러리
- Maven으로 의존성 추가
    - @Testcontainers (Extension, Composite Annotation)
    - Testcontainers 사이트 > Modules > DB 선택 > 의존성 주입
    - ex) PostgreSQL
        - static PostgreSQLContainer postgreSQLContainer = new PostgreSQLContainer().withDatabaseName("schemaName"); // static으로 하지 않으면 모든 테스트별 컨테이너 발생
        - @BeforeAll <br>
          static void beforeAl() { postgreSQLContainer.start(); }<br>
        - @AfterAll <br>
          static void afterAl() { postgreSQLContainer.stop(); }
        - @Container = 위 @Before/AfterAll 의 기능 수행<br>
          static PostgreSQLContainer postgreSQLContainer = new PostgreSQLContainer().withDatabaseName("schemaName");
            - @BeforeEach() { xxxRepository.deleteAll(); } // 모든 테스트 실행 전 DB 데이터 삭제 수행(테스트간 의존성 제거)
- properties 설정 
    - spring.datasource.url=jdbc:tc:{database_name}:///{schema} <= Url, User Info 중요하지 않음. **tc**가 중요
    - spring.datasource.driver-class-name=org.testcontainers.jdbc.ContainerDatabaseDriver <= driver로 jdbc 드라이버를 사용하도록 설정 필요
    
---

### 단점
- 테스트 실행 시간이 느리다.
   
--- 

### 예제
-  https://github.com/keesun/inflearn-the-java-test (출처 : 백기선 git)
## 출처
- https://docs.google.com/document/d/1j6mU7Q5gng1mAJZUKUVya4Rs0Jvn5wn_bCUp3rq41nQ/edit#
