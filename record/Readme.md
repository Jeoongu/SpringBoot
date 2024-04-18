# Java Record
- record는 Java 14에서 처음 소개된 새로운 클래스 타입으로, 변경 불가(immutable) 또 다른 말로 불변 객체 데이터 객체를 쉽게 만들 수 있게 한다. 또한 record는 기존의 클래스와 비슷하지만, 더 간결하고 효율적으로 데이터 객체를 생성할 수 있도록 설계되었다.

**불변 객체를 사용하는 이유**
> 불변성(immutable)이란 객체가 생성된 후 내부 상태가 일정하게 유지되는 상태를 말한다. 불변 객체는 내부 상태가 변하지 않기 때문에 thread-safe하고 부작용이 발생하지 않는다.

### Boilerplate Code
- Boilerplate Code는 컴퓨터 프로그래밍에서 사용되는 용어로 최소한의 변경으로 여러 곳에서 재사용되며, 반복적으로 비슷한 형태를 띄는 코드를 의미한다.
- 쉽게 말해 프로그래밍에서 반복되는 작업이나 패턴에 대한 일종의 표준화된 코드를 뜻한다.

**장점**
1. 코드의 재사용성 향상
    - 공통적으로 사용되는 코드를 모아놓음으로써 다른 프로젝트에서도 재사용할 수 있다. 이를 통해 생산성을 높일 수 있다.
2. 코드의 유지보수 용이
    - 중복 코드를 제거하여 코드의 유지보수성을 향상시키고, 표준화된 코드를 사용함으로써 코드의 일관성이 유지된다. 이를 통해 개발자가 코드를 이해하고 수정하는 데 걸리는 시간을 단축하고, 코드의 안정성과 신뢰성이 높아질 수 있다.
3. 개발 시간 단축
    - 자주 사용되는 기능을 미리 구현해놓음으로서 개발자가 새로운 프로젝트를 시작할 때마다 해당 기능을 처음부터 구현하지 않아도 되므로 시간과 노력을 절약할 수 있다.
4. 코드 품질 개선
    - 이미 검증된 코드를 사용하기 때문에, 코딩에서 실수가 발생할 위험을 줄이고 소프트웨어 품질을 개선할 수 있다. 또한 팀이 소스 코드 전체에서 올바른 코딩 표준과 일관된 언어 스타일을 유지할 수 있다.

**단점**
1. 유연성 저하
    - 미리 정해진 구조와 기능을 사용하기 때문에 프로젝트의 요구사항에 맞게 유연하게 수정하기 어려울 수 있다.
2. 개발자의 창의성 제한
    - 표준화된 코드를 사용함으로써 개발자의 창의성이 제한 될 수 있다.

> 예시
1. java에서의 Logger 초기화 코드
```java
public class MyClass{
    
    private static final Logger logger = LoggerFactory.getLogger(MyClass.class)

    public void myMethod(){
        logger.info("MyMethod is called);
    }
}
```
2. java에서의 생성자, Setter, Getter

## Record의 특징
1. 간결성(Conciseness)
    - record는 코드를 간결하게 만들기 위한 목적으로 도입되었다. 필드를 정의하면 해당 필드를 기반으로 자동으로 메서드가 생성되어 코드의 양을 줄일 수 있고, 불필요한 boilerplate code를 줄여 가독성을 높여준다.
2. 메서드 자동 생성(Automatic Method Generation)
    - record는 필드를 기반으로 `equals()`, `hashCode()`, `toString()` 메서드를 자동으로 생성한다. 이로써 불필요한 반복적인 코드 작성을 피할 수 있다.
3. 생성자 자동 생성(Automatic Constructor Generation)
    - record는 필드를 기반으로 자동으로 생성자를 생성한다(필드 기반 AllArgsConstructor). 그러나 필요에 따라 명시적인 생성자를 정의할 수 있다.
    - 그러나 모든 필드를 초기화 할 수 있는 생성자 하나만을 생성하므로 다른 생성자는 따로 정의해야한다.
4. 불변성(Immutability)
    - record는 필드가 한 번 설정되면 값을 변경할 수 없다. 이는 불변성을 보장하며 데이터의 안정성을 높이는데 도움이 된다.
5. final 선언 생략
    - record의 필드를 final로 선언할 필요가 없다. 컴파일러는 필드를 불변으로 자동으로 취급하고 자동으로 final로 처리한다.
6. 패턴 매칭(Pattern Matching) 통합
    - record는 Java의 Pattern Matching과 함께 사용될 수 있어, 데이터 추출 및 패턴 일치 검사에 효과적으로 사용 가능하다.
7. 데이터 전달(Data Transfer)
    - record의 주된 목적은 객체 간에 불변 데이터를 전달하는 것이다. 따라서 DTO(Data Transfer Object)를 표현하는 것에 적합하다.

## Record의 코드 작성 시 특징
1. record는 불변 객체로 필드 앞에 암시적으로 private, final로 선언
2. 멤버변수 선언 불가
3. 타 클래스는 상속 불가, 인터페이스 구현 가능
4. 각 필드는 getter 메서드 자동 생성
5. 객체의 모든 필드를 포함한 기본 생성자(AllArgsConstructor) 자동 생성
6. hashCode 메서드 자동 생성
7. toString 메서드 자동 생성
8. static 변수와 메서드 생성 가능

> Record 사용 예시
as-is
```java
public final class User{

    private final Long id;
    private final String email;
    private final String name;

    public User(Long id, String email, String name){
        this.id = id;
        this.email = email;
        this.name = name;
    }
    // ... getter, toString 등등
}
```
to-be
```java
public record User(
     Long id,
    String email,
    String name
) {
   
}
```

> 정리
- Java의 record는 JSON이나 XML 데이터를 나타내는 데이터 전송 객체를 다룰 때 특히 유용하다. 이는 코드를 간결하고 읽기 쉽게 만들어주며, 유지 보수도 용이하게 해준다.
