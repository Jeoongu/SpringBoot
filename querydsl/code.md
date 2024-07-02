# QueryDSL code 예시
- QueryDSL 사용으로 성능 향상이 가능하다.

### 의존성 주입
```java
dependencies {
	// queryDSL
	implementation 'com.querydsl:querydsl-jpa:5.0.0:jakarta'
    // querydsl  JPAAnnotationProcessor 사용 지정
    annotationProcessor "com.querydsl:querydsl-apt:${dependencyManagement.importedProperties['querydsl.version']}:jakarta" 
	// annotationProcessor "com.querydsl:querydsl-apt:5.0.0:jakarta"
	annotationProcessor "jakarta.annotation:jakarta.annotation-api" // java.lang.NoClassDefFoundError (javax.annotation.Generated) 대응 코드
	annotationProcessor "jakarta.persistence:jakarta.persistence-api" // java.lang.NoClassDefFoundError (javax.annotation.Entity) 대응 코드
}

// gradle clean 시에 QClass 디렉토리 삭제
clean {	
    delete file('src/main/generated')
} 

```

### 1. Q클래스 생성하기

<img src="https://velog.velcdn.com/images/kimsundae/post/fafdc270-5839-4bb9-9827-bdb2bbcbba21/image.png" width=300px>

- 인텔리제이의 오른쪽 Gradle 탭에서 clean 실행 후 build를 실행한다.
- 위와 같이 하면 프로젝트의 build -> generated에서 JPA entity class를 생성했던 디렉토리 명과 동일한 디렉토리에 Q클래스가 생성된다.

<img src="Q클래스.jpg" width=300px>

### 2. QueryDSL Configuration
- 프로젝트에서 QueryDSL을 사용하기 위해선 QueryDSL 설정이 필요하다.
- 따라서, 아래와 같이 JPAQueryFactory를 Bean으로 등록한다.
```java
@Configuration
@RequiredArgsConstructor
public class QueryDSLConfig {
    private final EntityManager entityManager;

    @Bean
    public JPAQueryFactory jpaQueryFactory(){
        return new JPAQueryFactory(entityManager);
    }
}
```

### 3. Custom Repository 생성 후 Spirng Data Jpa Repository와 연결하기
- 예시 Spring JPA Repository
```java
public interface MemberRepository extends JpaRepository<Member, Long> {
}
```
- QueryDSL을 사용하기 위한 별도의 사용자 정의 레포지토리를 인터페이스로 생성한다.
```java
public interface MemberQueryDSLRepositoryCustom {
 
    List<Member> findAllMember();
}
```
- 생성한 인터페이스를 구현하는 구현체 레포지토리를 만든다.
- 일종의 규칙이 존재한다. Spring Data Jpa 리포지토리 인터페이스명 뒤에 Impl을 붙여 구현한다.
```java
@RequiredArgsConstructor
public class MemberRepositoryImpl implements MemberQueryDSLRepositoryCustom {
    private final JPAQueryFactory jpaQueryFactory; // querydsl을 사용하기 위한 의존성 주입
 
    @Override
    public List<Member> findAllMember() {
        return jpaQueryFactory
                .selectFrom(member)
                .fetch();
    }
}
```
-  MemberRepository에 QueryDSL 사용자 정의 리포지토리 인터페이스를 추가로 implement한다.
```java
public interface MemberRepository extends JpaRepository<Member, Long>, MemberQueryDSLRepositoryCustom {
}
```
- 이제 MemberRepository만 주입받아서 서비스에서 QueryDSL문법을 사용한 메소드를 사용할 수 있게 된다.
