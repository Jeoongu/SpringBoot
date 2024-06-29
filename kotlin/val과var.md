# kotlin : val과 var
- val : 불변 변수로, 값의 읽기만 허용되는 변수를 말한다. value의 약자이다.
- var : 가변 변수로 값의 읽기와 쓰기가 모두 허용되는 변수이다. variable의 약자이다.
- 공통적으로 초기화시에 값을 할당하지 않는다면 반드시 Type을 명시해야한다. 다만 컴파일러가 추론할 수 있는 경우에는 생략해도 된다.

### val
- val로 지정된 불변 타입 변수는 초기에 값을 할당하면 나중에 값을 변경할 수 없다. 강제로 변경하려고 하면 컴파일 에러가 발생한다.
- 즉, Java의 final과 같다.
- 하지만, 코드 문맥에서 한 번만 초기화 되는 것이 맞다면, 상황에 따라 여러 값으로 초기화가 가능하다.(flow에 따른 update)
- 변수의 참조가 가리키는 객체의 내부 값은 변경이 가능하다.
```java
// val 변수 선언하기
val CHANGE_NO = 100
println(CHANGE_NO)
//CHANGE_NO = 200 // 할당되지 않는다.

val SCHOOL_NAME = "대한민국 학교"
println(SCHOOL_NAME)
```
```java
fun main(args: Array<String>){
    // 변수의 참조가 가리키는 객체의 내부 값은 변경이 가능하다.
    val nameArray = arrayListOf("Kotlin")
    nameArray.add("Java")
    println("nameArray : $nameArray")
}
```

### var
- var로 지정된 가변 타입 변수는 초기화 후 값을 변경할 수 있다.
- 하지만, 다른 타입의 값을 넣을 수는 없다.
- 이미 정의된 타입을 변경하려면 형변환을 해야 한다.
```java
// var 변수 선언하기
var changeYes = 100
//var changeYes // 선언과 동시에 초기화를 해야 한다.
println(changeYes)

changeYes = 200
println(changeYes)
```