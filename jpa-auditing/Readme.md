# JPA Auditing
## JPA Auditing 의미란 ?
- Auditing은 Audit(감시하다)의 뜻 그대로 Entity를 보고있다가 DB에 저장하거나 업데이트를 하는 경우 생성일자, 생성자, 수정일자, 수정자 등 중요한 메타데이터를 자동으로 데이터베이스에 반영해주는 기능.
- 간단하게 말하자면, 테이블 설계 시 공통으로 들어가는 생성일자, 수정일자, 생성자, 수정자 등을 자동으로 입력해주는 기능

## Auditing 기능 활성화
- import문
    ```java
    import org.springframework.data.jpa.repository.config.EnableJpaAuditing;
    ```
- configuration 분리
    ```java
    @EnableJpaAuditing
    @Configuration
    public class JpaAuditingConfiguration {
    }
    ```
- Application 클래스에 설정
    ```java
    @EnableJpaAuditing
    @SpringBootApplication
    public class FinalApplication {

        public static void main(String[] args) {
            SpringApplication.run(FinalApplication.class, args);
        }
    }
    ```

## BaseEntity(공통으로 사용하는 기본 엔티티 생성)
```java
@MappedSuperclass
@Getter
@NoArgsConstructor(access = lombok.AccessLevel.PROTECTED)
@EntityListeners(AuditingEntityListener.class)
// @ToString ?
public abstract class BaseEntity {
    @CreatedDate
    @Column(updatable = false)
    private LocalDateTime createdAt;

    @LastModifiedDate
    private LocalDateTime updatedAt;
}
```
- '@MapperSuperclass' : JPA Entity클래스가 해당 추상 클래스를 상속할 경우, 자식 클래스에게 매핑 정보 전달(부모 필드를 컬럼으로 인식)
- '@EntityListeners' : 엔티티를 DB에 적용하기 이전, 이후에 콜백 리스너를 요청
- '@CreatedDate', '@LastModifiedBy' : 생성자, 수정자
- '@Colume(updatable = false)' : : SQL update문에 해당 컬럼을 포함할지 여부(읽기 전용 필드라고 생각)

## BaseEntity를 통해 Auditing 기능이 적용된다.
- 적용 예시
    ```java
    @Builder
    @Entity
    @Getter
    @AllArgsConstructor
    @NoArgsConstructor
    @EqualsAndHashCode(callSuper = true)
    @ToString(callSuper = true)
    public class Post extends BaseEntity {

        @Id@GeneratedValue(strategy = GenerationType.IDENTITY)
        @Column(name = "post_id")
        private Long id;
        private String body;
        private String title;

        @ManyToOne(fetch = LAZY)
        @JoinColumn(name = "user_id")
        private User user;

    }
    ```
    > extends 해주면 Auditing 기능이 적용된다.

## ❗️주의
- 생성자와 수정자는 특정 테이블에서만 필요한 경우가 많다. 따라서 아래와 같이 Base 타입을 분리하여 원하는 타입을 상속받아도 된다.
```java
@Getter
@MappedSuperclass
@EntityListeners(AuditingEntityListener.class)
public class BaseTimeEntity {
        @CreatedDate
        @Column(updatable = false)
        private LocalDateTime createdDate;
        @LastModifiedDate
        private LocalDateTime lastModifiedDate;
}
```
```java
@Getter
@MappedSuperclass
@EntityListeners(AuditingEntityListener.class)
public class BaseEntity extends BaseEntity {
    @CreatedBy
    @Column(updatable = false)
    private String createdBy;
    @LastModifiedBy
    private String lastModifiedBy;
}
```