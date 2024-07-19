# N+1 조회
- 연관 관계에서 발생하는 이슈로 연관 관계가 설정된 엔티티를 조회할 경우에 조회된 데이터 갯수(n) 만큼 연관관계의 조회 쿼리가 추가로 발생하여 데이터를 읽어오게 된다. 
- 즉, 1번의 쿼리를 날렸을 때 의도하지 않은 N번의 쿼리가 추가적으로 실행되는 것이다. 이를 N+1 문제라고 한다.

### When
- JPA Repository를 활용해 인터페이스 메소드를 호출할 때(Read 시) 발생할 수 있다.

### Who
- 1:N 또는 N:1 관계를 가진 엔티티를 조회할 때 발생한다.

### How : 어떤 상황에서 발생하는가?
1. JPA Fetch 전략이 EAGER로 데이터를 조회하는 경우
2. JPA Fetch 전력이 LAZY 전략으로 데이터를 가져온 이후에 연관 관계인 하위 엔티티를 다시 조회하는 경우

### Why
- JPA Repository로 find 시 실행하는 첫 쿼리에서 하위 엔티티까지 한 번에 가져오지 않고, 하위 엔티티를 사용할 때 추가로 조회하기 때문에 발생한다.
- JPQL 사용시 JPQL은 기본적으로 글로벌 Fetch 전략을 무시하고 JPQL만 가지고 SQL을 생성하기 때문에 발생한다.

1. EAGER(즉시 로딩)인 경우
    1. JPQL에서 만든 SQL을 통해 데이터를 조회
    2. 이후 JPA에서 Fetch 전략을 가지고 해당 데이터의 연관 관계인 하위 엔티티들을 추가 조회
    3. 2번 과정에서 N+1 문제가 발생
2. LAZY(지연 로딩)인 경우
    1. JPQL에서 만든 SQL을 통해 데이터를 조회
    2. JPA에서 Fetch 전략을 가지지만, 지연 로딩이기 때문에 추가 조회는 하지 않음
    3. 하지만, 하위 엔티티를 가지고 작업하게 되면 추가 조회가 발생하기 때문에 결국 N+1 문제 발생
- 즉, 지연 로딩에서는 N+1 문제가 발생하지 않는 것처럼 보이지만, 막상 객체를 탐색하려고 하면 N+1문제가 발생한다.
- N+1 문제가 발생되는 시점만 즉시 로딩과 다를 뿐이다.

### 해결 방법
1. Fetch Join
- N+1 자체가 발생하는 이유는 한쪽 테이블만 조회하고 연결된 다른 테이블을 **따로 조회**하기 때문이다.
    - 즉, 미리 두 테이블을 Joig하여 한 번에 모든 데이터를 가져올 수 있다면 애초에 N+1 문제가 발생하지 않을 것이다.
- Fetch Join의 단점
    - 우리가 연관관계 설정해놓은 FetchType을 사용할 수 없다. Fetch Join을 사용하게 되면 데이터 호출 시점에 모든 연관 관계의 데이터를 가져오기 때문에 FetchType을 LAZY로 해놓은 것이 무의미하다.
    - 페이징 쿼리를 사용할 수 없다. 하나의 쿼리문으로 가져오다 보니 페이징 단위로 데이터를 가져오는 것이 불가능하다.
```java
public interface TeamRepository extends JpaRepository<Team, Long> {
    @Query("select t from Team t join fetch t.users")
    List<Team> findAllFetchJoin();
}
```

2. `@EntityGraph`
- `@EntityGraph`의 attributPaths에 쿼리 수행시 바로 가져올 필드명을 지정하면 Lazy가 아닌 Eager 조회로 가져오게 된다.
- 그러나 Fetch Join과 다르게 Join문이 outer Join으로 실행된다.
    - 기본적으로 outer Join보다 inner Join이 성능 최적화에 더 유리하다.
```java
@EntityGraph(attributePaths = "cats")
@Query("select o from Owner o")
List<Owner> findAllEntityGraph();
```

3. Batch Size
- 이 옵션은 N+1 문제를 안 일어나게 하는 방법은 아니다.
- N+1 문제가 발생하더라도 아래와 같은 방식으로 발생하게 하는 방법이다.
```text
select * from user where team_id = ? 이 아닌 
select * from user where team_id in (?, ?, ? )
```
- 이렇게 하면 100번 일어날 N+1 문제를 1번만 더 조회하는 방식으로 최적화할 수 있다.
```yml
spring:
  jpa:
    properties:
      hibernate:
        default_batch_fetch_size: 1000
```

### 실무에서 이를 방지하기 위한 방법?
- 연관관계에 대한 설정이 필요하다면 FetchType을 성능 최적화를 하기 어려운 즉시 로딩(EAGER)을 사용하는 게 아니라 지연 로딩 (LAZY) 모드로 사용을 하고 성능 최적화가 필요한 부분에서는 Fetch 조인을 사용한다.
- 또한 기본적으로 Batch Size의 값을 1000 이하로 설정한다. (대부분의 DB에서 IN절의 최대 개수 값 : 1000)
- 아니면, 아예 연관관계를 끊는다.