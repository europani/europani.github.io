---
layout: post
title: '[JPA] BaseTimeEntity - 생성/수정 시간 자동 설정'
categories: Spring
tags: [Spring, JPA]
---
모든 Entity의 상위 클래스에서 createdDate, updateDate를 자동으로 관리해주는 역할

- `Date` 자료형을 지양하고 8버전에 추가된 `LocalDate`, `LocalDateTime`을 사용하는 것을 추천한다
- `BaseTimeEntity` 추상클래스를 구현하고 `Entity` 클래스들에게 상속 시켜 사용한다

```java
@Getter
@MappedSuperclass
@EntityListeners(AuditingEntityListener.class)  // Auditing 기능 포함
public abstract class BaseTimeEntity {

    @CreatedDate
    @Column(updatable = false)
    private LocalDateTime createdDate;

    @LastModifiedDate
    private LocalDateTime modifiedDate;
}

```
- `@MappedSuperclass` : JPA Entity 클래스들이 `BaseTimeEntity`를 상속 할 경우 createdDate, modifiedDate 두 필드도 컬럼으로 인식하도록 설정
- `@CreatedDate` : 생성시 날짜 자동 생성
- `@LastModifiedDate` : 수정시 날짜 자동 갱신

```java
public class Members extends BaseTimeEntity {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(length = 20, nullable = false)
    private String username;

    @Column(nullable = false)
    private String password;

    @Column(length = 10)
    private String name;

    @Enumerated(EnumType.STRING)
    private MembersRole role;
}
```

- 메인클래스에서 `JPA Auditing`을 활성화 시켜야 한다

```java
@EnableJpaAuditing // JPA Auditing 활성화
@SpringBootApplication
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

◼︎ `@WebMvcTest`를 통해 Controller 테스트를 할 경우
- 메인클래스가 실행되자마자 `@EnableJpaAuditing`이 작동한다. 이를 사용하기 위해서는 `@Entity`가 1개 이상이여야 한다
- 하지만, `@WebMvcTest`는 Controller 관련 클래스만 스캔하기 때문에 JPA 관련 클래스를 읽을 수 없다
- 그 결과 `Caused by: java.lang.IllegalArgumentException: JPA metamodel must not be empty!`이 발생한다

- 해결방안 : `@EnableJpaAuditing`를 `@SpringBootApplication`와 분리

```java
@SpringBootApplication
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}

@Configuration
@EnableJpaAuditing // JPA Auditing 활성화
public class JpaConfig {
}
```
