# RESTful API
- RESTful API는 REST(Representational State Transfer) 아키텍처 스타일의 설계 원칙을 준수하는 API이다.
- 단순하게 말하면 *"REST란 HTTP를 잘 사용하기 위한 아키텍처 스타일"*이다.
- 프로토콜이나 표준이 아닌 아키텍처 제약 조건이다. 개발자는 REST를 다양한 방식으로 구현할 수 있다.

1. HTTP URI(Uniform Resource Identifier)를 통해 자원(Resource)를 명시하고,
2. HTTP Method(POST, GET, PUT, DELETE, PATCH 등)을 통해
3. 해당 자원(URI)에 대한 CRUD Operation을 적용하는 것을 의미한다.

- REST에서의 CRUD Operation 동작 예시
```text
Create : 데이터 생성(POST)
Read : 데이터 조회(GET)
Update : 데이터 수정(PUT, PATCH)
Delete : 데이터 삭제(DELETE)
```

### RESTful의 목적
- REST의 특징을 기반으로 서비스 API를 구현하는 것

1. 이해하기 쉽고 사용하기 쉬운 REST API를 만드는 것
2. RESTful한 API를 구현하는 근본적인 목적이 성능 향상에 있는 것이 아니라 일관적인 컨벤션을 통한 API의 이해도 및 호환성을 높이는 것이 주 동기이니, 성능이 중요한 상황에서는 굳이 RESTful한 API를 구현할 필요는 없다.

### REST의 특징
1. Server-Client(서버-클라이언트 구조)
    - 자원이 있는 쪽이 서버, 요청하는 쪽이 클라이언트가 된다. 서버는 API를 제공하고 비즈니스 로직 처리 및 저장을 책임진다.
2. Stateless(무상태)
    - HTTP 프로토콜은 Stateless Protocol이므로 REST 역시 무상태성을 갖는다. Client의 context, 상태를 서버에 저장하지 않는다.
3. Cacheable
    - HTTP의 캐싱 기능을 적용할 수 있다.
4. Layered System(계층화)
    - 클라이언트는 REST API Server만 호출하고, REST Server는 다중 계층으로 구성될 수 있다.
5. Uniform Interface(인터페이스 일관성)
    - URI로 지정한 Resource에 대한 조작을 통일되고 한정적인 인터페이스로 수행한다.

### REST API 설계 규칙
1. URI는 동사보다는 명사를, 대문자보다는 소문자를 사용한다.
```text
Bad Example http://aledma.com/Running/
Good Example  http://aledma.com/run/  
```

2. 마지막에 슬래시 구분자(/)를 포함하지 않는다.
```text
Bad Example http://aledma.com/test/  
Good Example  http://aledma.com/test
```

3. 언더바(_) 대신 하이폰(-)을 사용한다.
```text
Bad Example http://aledma.com/test_blog
Good Example  http://aledma.com/test-blog  
```

4. 파일확장자는 URI에 포함하지 않는다.
```text
Bad Example http://aledma.com/photo.jpg  
Good Example  http://aledma.com/photo  
```

5. 행위는 포함하지 않는다.(CRUD같은 동사)
```text
Bad Example http://aledma.com/delete-post/1  
Good Example  http://aledma.com/post/1  
```

6. 슬래시 구분자(/)는 계층 관계를 나타내는데 사용한다.
```text
http://aledma.com/houses/apartments
```

7. 리소스 간에 연관 관계가 있는 경우, `/리소스명/리소스 ID/관계가 있는 다른 리소스명` 이런 식으로 표현한다.
```text
GET : /users/{userid}/devices (일반적으로 소유 ‘has’의 관계를 표현할 때)
```

### 장점 및 단점
- 장점
1. 기존 시스템과의 호환성 유지
2. 개발 시간 단축 및 비용 절감
3. 개발자간 협업 용이성
4. 비즈니스 확장 용이성

- 단점
1. API를 사용하기 위해 문사를 이해하고 구현해야하는 높은 기술적 요구사항
2. 보안 문제 존재 가능성
3. API가 변경될 경우 호환성 문제 발생 가능성
4. 제공되는 API의 제한적인 경우 제한적인 기능만 사용