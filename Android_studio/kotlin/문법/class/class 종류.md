enum class

sealed class

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
클래스 정의 없이 바로 객체를 생성하는 방법.
- 싱글톤으로 만들 때
- companion object 만들 때
- anonymous object 만들 때
- Companion object는 이 객체를 포함하는 클래스의 private 멤버에 접근 가능

