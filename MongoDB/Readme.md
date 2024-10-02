# MongoDB
- 문서 지향적인(Document) NoSQL 데이터베이스로, 대량의 비정형 데이터를 저장하고 처리하는 것에 사용되는 데이터베이스 중 하나이다.
<img src="https://velog.velcdn.com/images/zihooy/post/f1e8204f-b532-49d3-b587-d1d60d575209/image.png" width=700px>

```json
[
  {
    "_id": ObjectId("61e0d934a4b2d43e3aef3e6f"),
    "name": "John",
    "gender": "남",
    "age": 30,
    "address": "New York"
  },
  {
    "_id": ObjectId("61e0d934a4b2d43e3aef3e70"),
    "name": "Alice",
    "gender": "여",
    "age": 25,
    "address": "London"
  },
  {
    "_id": ObjectId("61e0d934a4b2d43e3aef3e71"),
    "name": "Michael",
    "gender": "남",
    "age": 35,
    "address": "Los Angeles"
  }
]
```
- 위의 사진은 RDBMS의 데이블에 저장할 때의 저장 형식이고, 같은 데이터를 하나의 문서안에 json형식으로 저장하는 것이 NoSQL인 MongoDB의 방식이다.

### 특징
- 도큐먼트 지향
    - MongoDB는 도큐먼트 지향적인 데이터베이스이다. JSON형태로 데이터를 관리하며, 객체 지향적으로 DB를 저장할 수 있다는 장점이 있다
- 스키마less
    - 테이블과 같은 정형적인 구조에 데이터를 저장하는 것이 아니라 비정형 데이터를 저장할 때 주로 사용된다. 테이블 역할을 하는 컬렉션이 존재하는데 저장 규칙이 따로 존재하지 않는다.
- 비관계형 DB
    - MongoDB에는 관계의 개념이 없다. 따라서 조인 연산을 지원하지 않는다.
- 트랜잭션 지원 X
    - 관계형 DB와 다르게 트랜잭션을 지원하지 않는다. 따라서 Commit, Rollback의 개념이 존재하지 않는다.

### 의존성 및 application.yml
```java
implementation ('org.springframework.boot:spring-boot-starter-data-mongodb');
```

```java
spring:
  data:
    mongodb:
      uri: mongodb+srv://user:<password>@pushpin.lgan0wm.mongodb.net/<database이름>?retryWrites=true&w=majority
```
- 이런 식으로 yml파일에 uri를 입력해주어야 한다.

### Application 클래스 및 컬렉션 클래스
- application 클래스 어노테이션
```java
@EnableMongoRepositories // 꼭 넣어야하는지 확인 필요
```

- 엔티티와 유사한 컬렉션 클래스
```java
@Document(collection = "interpark") // 실제 몽고 DB 컬렉션 이름 
@Getter
@NoArgsConstructor
@AllArgsConstructor
public class CrawlingInfo {
    @Id
    private String id;
    private String name;
    private Long age;
}
```

### Repository
```java
@Repository
public interface CrawlingRepository extends MongoRepository<CrawlingInfo, String>{
	    CrawlingInfo findCrawlingInfoByName(String name);

}
```
- JPA와 비슷하게 레포지토리에 이런식으로 extends 하여 사용한다.
- Spring Data MongoDB를 사용하기 위해서는 Repository를 쓰는 방법과 MongoTemplate를 쓰는 방법 2가지가 있다.
    - MongoTemplate
    - MongoRepository를 상속받은 Repository 사용

1. MongoTemplate
    - Criteria(쿼리를 만들어주는 클래스)와 함께 사용할 수도 있고, 기본적으로 제공한느 메소드를 제공하여 기본적인 CRUD기능을 수행할 수 있다
2. MongoRepository를 상속받은 Repository 사용
    - 복잡한 쿼리문일 시 가독성이 좋다.
    - JPA와 비슷한 형식으로 빠르게 이해할 수 있다.
    - 쿼리 메소드로 개발자 편의성 증대
    - Querydsl 도입을 고려한다면 이 방법을 사용하는 것이 좋다.






