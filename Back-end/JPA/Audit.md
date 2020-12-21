# Audit

## Audit 적용

- 모든 엔티티에 공통으로 들어가는 필드(컬럼) 관리를 위한 기능

## 적용하기

1. @EnableJpaAuditing 추가 (in SpringApplication.java)
    ```
    @SpringBootApplication
    @EnableJpaAuditing
    public class Application {
        // 생략
    }
    ```
2. 공통 엔터티 생성
    - 등록일시, 수정일시
    ```$xslt
    @MappedSuperclass
    @EntityListeners({AuditingEntityListener.class})
    @Getter
    public class BaseEntity {
    
        @CreatedDate
        @Column(updatable = false)
        private LocalDateTime created_at;
    
        @LastModifiedDate
        private LocalDateTime updated_at;
    }
    ```
3. 등록자, 수정자 적용을 위한 AuditorAware 스프링 빈 등록
    ```$xslt
    @Bean
      public AuditorAware<String> auditorProvider() {
        return () -> Optional.of("사용자이름.. 실무에서는 세션이나 스프링 시큐리티 로그인 정보 이용"); }
    ```
4. 엔터티 적용
    - 적용하고자 하는 엔터티에 extends
5. 실무 TIP
    - 실무에서 등록시간/수정시간만 사용하고, 등록자/수정자는 사용하지 않을 경우
    - Base 타입을 분리하여 원하는 타입 상속하여 적용
    - ex)
    ```$xslt
    public class BaseTimeEntity {
        @CreatedDate
        @Column(updatable = false)
        private LocalDateTime createdDate;
        @LastModifiedDate
        private LocalDateTime lastModifiedDate;
    }
    public class BaseEntity extends BaseTimeEntity {
        @CreatedBy
        @Column(updatable = false)
        private String createdBy;
        @LastModifiedBy
        private String lastModifiedBy;
    }
    ```


## 참고

- 적용 방법 : [jpa.audit.velog](https://velog.io/@aidenshin/%EC%97%94%ED%8B%B0%ED%8B%B0-%EC%9E%91%EC%84%B1-JPA-Auddit-%EC%A0%81%EC%9A%A9)
