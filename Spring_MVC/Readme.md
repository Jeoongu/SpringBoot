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


