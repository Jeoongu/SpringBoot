# 스프링부트 QueryDSL
- DSL는 Domain Specific Language의 약자로, 특정 영역에 특화된 언어를 의미한다. 그런 의미에서 QueryDSL은 쿼리생성에 특화된 프레임워크를 의미한다.

## JPA와 JPQL
- QueryDSL을 알기 전에 JPA와 JPQL을 먼저 알아야 한다.

### JPA
- JPA는 객체지향어플리케이션과 관계형DB 사이의 패러다임 불일치를 해결하는 프레임워크이다. 
- 개발자는 객체지향 관점으로 개발하고 JPA프레임워크는 자동으로 SQL쿼리문을 생성한다. 
- SQL문이 자동으로 생성되니 개발자는 SQL 관점 프로그래밍을 하지 않아도 된다. **그러나 완전한 분리는 불가능하다**. 복잡한 쿼리 생성을 위해, JPA는 JPQL을 지원한다. 

### JPQL
- JPA도 한계가 있다. 모든 쿼리를 객체지향방식으로 표현할 수 없다. 특히나 SELECT문은 조회에 여러 조건이 붙는 경우도 많고 조건이 동적으로 변하기도 한다. 그러므로 이를 커버할 무언가가 필요하다. 그것이 JPQL(Java Persistence Query Language)이다.
- JPQL은 SQL문과 상당히 유사하다. 객체지향적 관점을 유지하고 있는 SQL문 정도로 보면된다.
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fn1Fhy%2FbtspfOB1wZE%2FSIp6HuatBOW3M4bFpAdf91%2Fimg.png" width=300px>

- 그러나 JPQL은 크게 2가지 문제가 있다.
1. 타입안정성이 떨어진다
    - JPQL은 문자열이기에 개발자는 JPQL문법이 틀려도 이를 알아차리지 못한다. 그래서 런타임 중에  메소드가 호출되어 JPQL이 파싱되어야 문법오류를 발견할 수 있다. @Query 어노테이션을 사용하면 프로그램이 올라가는 과정에 문법 오류를 발견 할수 있지만, 이것도 결국은 프로그램을 실행해야만 오류를 발견할 수 있다.  즉, JPQL을 파싱하는 프로세스가 동작해야만 문법오류를 발견할 수 있고 컴파일 과정에서는 어떤 오류도 발견할 수 없다.
2. 직관적인 동적쿼리 작성이 어렵다.
    - JPQL은 문자열이다. 동적쿼리를 작성하려면 문자열을 조작하는 방식으로 로직을 구성해야 한다. 그러다보니 문자열과 문자열 사이에 if-else문이나 for문 같은 코드가 들어가 가독성을 떨어트린다. 
    ```java
    String jpqlQuery = "select m from Member m";

    if(id != null){ //id가 NULL이 아니면
        jpql += "where id = :id"; // WHERE 조건문 추가
    }

    TypedQuery<Member> query = em.createQuery(jpqlQuery, Member.class);

    if(id != null){ //id가 NULL이 아니면
        query = query.setParameter("id", id); // 파라미터 추가
    }
    ```

> QueryDSL은 JPA가 지원하는 표준기술은 아니지만 타입안정성을 보장하고 직관적인 동적쿼리 작성이 가능하다. 이런 이유로 실무에서 JPA와 연계되어, 가장 많이 사용되는 프레임워크 중 하나이다.

## QueryDSL
<img src="https://velog.velcdn.com/images/chang626/post/19f3fa40-c7e3-4682-8c2b-c0f137fc7eb1/image.png" width=300px>
<img src="https://blog.kakaocdn.net/dn/GaaEz/btrE5TuxUdv/I1J43tpkjBLLrLsRlj4rK0/img.png" width=300px>

- 