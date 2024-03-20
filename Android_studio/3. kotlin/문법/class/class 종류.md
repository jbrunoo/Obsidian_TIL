enum class

sealed class [#1](https://kotlinlang.org/docs/sealed-classes.html#constructors)
- sealed interface [medium](https://jorgecastillo.dev/sealed-interfaces-kotlin?source=post_page-----db1fff634860--------------------------------)

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



