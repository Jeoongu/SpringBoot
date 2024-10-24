# Spring MVC
- Spring MVC패턴은 **Model, View, Controller** 아키텍처 패턴을 기반으로 한다.
- Controller와 View라는 영역으로 서로 역할을 나누고, Model을 통해서 데이러를 전달하는 방식이다. 웹 어플리케이션은 보통 이 MVC패턴을 사용한다.

## MVC(Moder View Controller)
- MVC패턴 이전 사진
<img src="https://hongchangsub.com/content/images/2022/05/Screen-Shot-2022-05-26-at-6.05.26-PM.png" width=500px>
    - MVC패턴을 도입하기 전에는, 아래의 그림처럼 비즈니스 로직과 뷰 로직이 함께 존재했다. 

- MVC패턴 이후 사진
<img src="https://blog.kakaocdn.net/dn/bPcTdO/btq2ckRDKc2/Jgne5Rhthx6z7ZNclAOX71/img.png" width=500px>

- 컨트롤러 
    - 사용자의 요청을 처리하는 역할, 요청을 받고, 요청에 맞는 모델 데이터를 생성하거나 수정하고, 적절한 뷰를 선택하여 데이터를 반환한다.
    - HTTP 요청을 받아 파라미터를 검증하고, 비즈니스 로직을 담당한다. 그 후, 뷰에 전달할 결과를 조회해서 모델에 담는다.
- 모델
    - 뷰에서 필요한 데이터가 모두 담겨 전달된다. 이 덕분에 뷰는 비즈니스 로직을 몰라도 되고, 화면 렌더링에 집중할 수 있다.
- 뷰
    - 뷰는 UI를 담당한다. 즉, 데이터를 어떻게 표시할지를 담당하는 부분이다.
        - 주로 HTML, JSP, Thymeleaf와 같은 템플릿 엔진을 사용하여 데이터를 시각적으로 표시한다.
    - 모델에 담겨있는 데이터를 사용해서 화면을 구성하는 일에 집중한다. 즉, 순수하게 표현의 역할만 수행하고, 비즈니스 로직을 포함하지 않는다.

- 다들 알다시피, 컨트롤러가 아닌 서비스/레포지토리에서 비즈니스 로직과 데이터 접근을 진행한다.

## Spring MVC 구조 이해
- 스프링 MVC 구조 사진, 두 사진 모두 괜찮은데, 아래 사진이 좀 더 자세히 설명되어 있다!
<img src="https://oopy.lazyrockets.com/api/v2/notion/image?src=https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F183bb42c-2998-4362-ade1-b7d75f75a851%2FUntitled.png&blockId=209ecc2e-d659-4a44-a519-675c16309d89" width=500px>
<img src="https://i.imgur.com/blr7x6q.png" width=500px>

### Spring MVC 동작 구조
1. 클라이언트가 서버에 요청을 하면, Front Controller인 DispatcherServlet 클래스가 요청을 받는다.
2. DispatcherServlet은 프로젝트 파일 내의 servlet-context.xml 파일의 @Controller 인자를 통해 등록한 요청 위임 컨트롤러를 찾아 매핑(mapping)된 컨트롤러가 존재하면 @RequestMapping을 통해 요청을 처리할 메소드로 이동한다.
3. 컨트롤러는 해당 요청을 처리한 Service(서비스)를 받아 비즈니스 로직을 서비스에게 위임한다.
4. Service는 요청에 필요한 작업을 수행하고, 요청에 대해 DB에 접근해야 한다면 DAO에 요청하여 처리를 위임한다.
5. DAO는 DB 정보를 DTO를 통해 받아 서비스에게 전달한다.
6. 서비스는 전달받은 데이터를 컨트롤러에게 전달한다.
7. 컨트롤러는 Model(모델) 객체에게 요청에 맞는 View 정보를 담아 DispatcherServlet에게 전송한다.
8. DispatcherServlet은 ViewResolver에게 전달받은 View 정보를 전달한다.
9. ViewResolver는 응답할 View에 대한 JSP를 찾아 DispatcherServlet에게 전달한다.
10. DispatcherServlet은 응답할 뷰의 Render를 지시하고 뷰는 로직을 처리한다.
11. DispatcherServlet은 클라이언트에게 Rending된 뷰를 응답하며 요청을 마친다.


### 1. DispatcherServlet
- 우선 Dispatcher Servlet를 설명하기 위해서, Front Controller개념을 먼저 알아야 한다.

#### Front Controller
- 프론트 컨트롤러 도입 전
<img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQ5k7hIGt1A9dchCU4_JG915ciKS9eldD7iBKjTb9tSulDnyfuuLTmG2I2-0Z57pd0Pq20&usqp=CAU" width=500px>

- 프론트 컨트롤러 도입 후
<img src="https://blog.kakaocdn.net/dn/b9iAbR/btshiEtgguj/lSoUMb1urhSa2vTjG6pBWK/img.png" width=500px>

- Front Controller가 서블릿 하나로 클라이언트의 요청을 받고, 요청에 맞는 컨트롤러를 찾아서 호출하면 된다.
- 스프링 웹 MVC의 DispatcherServlet이 바로 이 Front Controller 패턴으로 구현되어 있다.
- DispatcherServlet에서 공통적인 내용을 처리한 후, 어떤 컨트롤러로 보내야 할지 판단해야 한다.



