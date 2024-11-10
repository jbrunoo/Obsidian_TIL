abstract class vs interface
	클래스의 상속과 관련 있음
	kotlin에서 class를 상속받으려면 클래스와 함수에 open 키워드를 붙여야 함.
	abstract class, interface에서는 open 키워드 안 붙여도 됨.
	위 두 클래스를 상속받으면 하위 멤버들을 구현해야 함.
	공통점은 둘 다 하위 클래스의 구현을 위해 정의하는 클래스이며 객체를 생성할 수 없음.
	차이점은 abstract class만 생성자를 가질 수 있고 interface는 불가능.
	이어 추상 클래스만 [[백킹 필드]]를 가질 수 있음
	또한, 하위 클래스가 abstract class는 하나만 상속 가능하고 interface는 여러 개 상속 받을 수 있음
	[참고블로그1](https://interlude.tistory.com/15), [참고블로그2](https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4-vs-%EC%B6%94%EC%83%81%ED%81%B4%EB%9E%98%EC%8A%A4-%EC%B0%A8%EC%9D%B4%EC%A0%90-%EC%99%84%EB%B2%BD-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0)

	하지만 위의 차이가 두 종류를 나누는 큰 기준은 되지 못한다.
	사용의 목적을 적절히 분석하는 것이 더 중요하다.
	상속 즉 다형성을 위함인데, 추상 클래스는 목적, 인터페이스는 기능 위주다.
	쉽게 표현하면 상속하려는 기능이 부모-자식 관계인지 형제 관계인지 파악해야 한다.
	
	ex) 생명체 - (동물, 물고기) - (사람, 고양이), (금붕어, 고래)
		생명체, 동물, 물고기는 추상 클래스로 구현될 것이다.
		사람, 고양이, 고래, 상어는 상위 추상 클래스를 상속받은 서브 클래스이다.
		여기서 물 속에서 숨을 쉰다, 새끼를 낳는다와 같은 기능이 있다면,
		물속에서 숨을 쉬는 것은 물고기 추상 클래스에서 추상 메서드를 정의 가능하지만
		새끼를 낳는다는 사람, 고양이, 고래에 해당되므로 사람과 물고기에 추상 메서드를 정의한다면
		금붕어는 강제로 새끼를 낳는다라는 기능을 구현하게 된다.
		즉, 이는 인터페이스로 정의하는 것이 마땅하다.

- - - 
enum class
	상수 열거

sealed class [#1](https://kotlinlang.org/docs/sealed-classes.html#constructors)
	sealed interface [medium](https://jorgecastillo.dev/sealed-interfaces-kotlin?source=post_page-----db1fff634860--------------------------------)

data class
	auto-generate : equals / hashCode / toString / copy
```kotlin
data class MyClass(val a: Int, val b: String)                           

fun main() {
    val str1 = "Hello, Kotlin"
    val str2 = "Hello, Kotlin"
    val class1 = MyClass(10, "class1")
    val class2 = MyClass(10, "class1")
    val class3 = MyClass(20, "class2")

    println(str1 == str2) // true
    println(class1 == class2)  // true
    println(class1 == class3)  // false
    println(class1 === class2) // false
}
```


object
	클래스 정의 없이 바로 객체를 생성하는 방법.(싱글톤)
	- 싱글톤으로 만들 때
	- companion object 만들 때
	- anonymous object 만들 때
	멀티 스레드 환경에서 일반적으로 어떤 함수, 변수, 객체가 여러 스레드로부터 동시에 접근이 일어나도 프로그램 실행에 문제 없음 (thread safe)
	Object 키워드가 선언된 클래스는 외부에서 객체가 사용되는 시점에 초기화가 이루어짐.(lazy initialization)
	주/부 생성자 이용X, 객체 생성과 동시에 생성자 호출 없이 바로 만들어짐.
	중첩 object 선언 가능, 클래스나 인터페이스 상속 가능.


companion object
	클래스 내부의 객체 선언을 위한 object 키워드.
	클래스 내부에서 싱글톤 패턴 구현 위해.
```kotlin
```kotlin
class MyClass {
    companion object Factory {
        fun create(): MyClass = MyClass()
    }
}
# 생성자 없이 클래스의 이름을 사용해서 호출 가능
val instance = MyClass.create()
```

클래스 당 하나만 사용 가능, 생성자 가질 수 없음, static으로 선언이 아닌 런타임 시 실제 객체의 인스턴스 실행.



