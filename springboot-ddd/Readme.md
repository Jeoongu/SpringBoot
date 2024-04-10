# 스프링부트에 DDD 구조 적용하는 방법
## DDD란?
- Domain Driven Development의 준말로 도메인 주도 개발을 의미한다.
- 이에 대응되는 아키텍처로는 Layered Architecture이 있다.
- DDD의 목표는 "High cohesion" ,"Loosly coupling"으로 각각의 도메인은 서로 철저히 분리되고, 높은 응집력과 낮은 결합도로 변경과 확장에 용이한 설계를 얻게 된다.

## Layered Architecture
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdXLkOR%2FbtrvDQM9EfZ%2FMQ1xCqwjpXsNGQnS1jc0Gk%2Fimg.png" width=300px>

- 말 그대로 계층이 나뉘어져 있는 아키텍처를 의미한다.
- 주된 목표는 각각의 Layer가 하나의 관심사에만 집중할 수 있도록 하는 것이다.
- 중요한 규칙 2가지
    - 위의 계층에서 아래 계층으로는 접근이 가능하지만 아래에서 위로는 불가능한 것을 기본으로 한다.
    - 한 계층의 관심사와 관련된 어떤 것도 다른 계층에 배치되어서는 안된다.
- 계층 별 설명
    1. Presentatin Layer(표현 계층) (Controller)
        - 사용자 요청에 대해 해석하고 응답하는 일을 책임지는 계층이다
        - 사용자에게 UI를 제공하거나 클라이언트에 응답을 다시 보내는 역할을 하는 모든 클래스가 포함된다.
        - Client로부터 request를 받고 response를 return하는 API를 정의.
    2. Application Layer (응용 계층) (Service)
        - 비즈니스 로직을 정의하고 정상적으로 수행될 수 있도록 도메인 계층과 인프라 스트럭처 계층을 연결해주는 역할을 하는 계층이다.
        - 이 계층은 많은 정보를 가지고 있지 않게 유지하는 것이 중요하며, 실질적인 데이터의 상태 변화 등의 처리는 도메인 계층에서 진행할 수 있도록 위임하는 것이 주요하다.
        - Application Layer에 포함하는 기등들
            - 트랜잭션의 단위
            - DTO 변환
            - 엔티티 조회/저장
            - **단순하게 말하면 Entity를 찾고(Repository), 변경 내용을 저장하는 기능(Persistence)을 호출한다. (구현은 Infra layer)**
            - 사용자 인증/인가 : 사용자가 특정 URL에 대해 권한이 있는지 정도의 인가는 presentation layer에서 하지만, URL만으로 판단이 어렵거나, DB내의 데이터와 대조해봐야 알 수 있는 경우(데이터 존재 여부, 중복 여부 등)
            - 파라미터 검증 : presentation layer에서는 주로 요청 방식에 따라 달라지는 ‘형식’에 대한 검증을 하고, 여기서는 ‘논리적’인 오류를 검증한다.
    3. Domain Layer (도메인 계층) (Model)
        - 비즈니스 규칙, 정보에 대한 실질적인 도메인에 대한 정보를 가지고 있으며 이 모든 것을 책임지는 계층이다.
        - Entity를 활용하여 도메인 로직이 실행되며, 업무 상황을 반영하여 상태를 제어하는 역할에 집중하는 계층이다.
    4. **Infra structure Layer** (인프라 계층) (Repository)
        - 외부와의 통신(DB, 메시징 시스템)을 담당하는 계층이다.
        - 해당 계층에서 얻어온 정보를 응용 계층 또는 도메인 계층에 전달하는 것이 주 역할이다.
- 각각의 도메인들을 위와 같은 Layer로 철저히 분리해서 만드는 것이 DDD(Domain-Driven Design)의 핵심 설계 방식이다.

## Layered Achitecture의 장단점
장점
- 각 레이어를 loosely coupling 된 형태로 구축하면서, 각각 자신의 관심사에만 집중할 수 있다.
- 핵심 비즈니스 로직을 순수하게 유지함으로써 유지보수와 확장성 측면에서 이득을 얻을 수 있다.
- 각 레이어에서 서로 다른 추상화 수준을 가진 상태와 행동을 위치시킴으로써 코드 재사용성을 높일 수 있다.

단점
- 서비스가 커질 수록 복잡도가 증가하여 확장성이 떨어진다.
- 레이어로 분리된 관심사 외에 다른 관심사가 생길 경우 패키지 분리 및 코드 배치가 어렵다.

## Layered Architecture를 사용해야 하는 경우
- 작고 단순하며 확장성보다는 일관성을 가져가는 것이 목표인 어플리케이션이나 웹사이트에 적합하다.
- 또한 단순성 및 구현 용이성으로 인해 어플리케이션 시작점으로 사용하기에 좋다.

→ **application, domain, infra 3계층 구조와 더해서 global을 추가한 패키지 구조도 이에 해당한다.**
추론 객체 참조?
