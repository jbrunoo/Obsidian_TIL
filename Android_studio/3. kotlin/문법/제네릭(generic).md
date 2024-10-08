#### 개념
	일반화(generalize)
	모든 타입에 대응되는 <T>로 표현하며 런타임 시 타입을 결정
	클래스 내부에서 사용할 자료형을 나중에 인스턴스를 생성할 때 확장하는 개념
	
	배경 : 자료형의 객체들을 다루는 메서드나 클래스에서 컴파일 시간에 자료형을 검사해 적당한 자료형을 선택할 수 있도록 하기 위해서. 객체의 자료형을 컴파일할 때 체크하기 때문에 객체 자료형의 안정성을 높이고 형변환의 번거로움이 줄어듬.
	
	요약하자면 여러 자료형을 받을 수 있는데 컴파일 시 Person이라는 자료형으로 타입이 결정되면 그 타입만 올 수 있음.

	다양한 자료형을 다뤄야하는 컬렉션에 많이 사용됨

	장점 : 의도하지 않은 자료형의 객체를 지정하는 것을 막고 객체를 사용할 때 원래 자료형에서 다른 자료형으로 형 변환 시 발생할 수 있는 오류를 줄여줌

	사용
	앵글 브래킷(<>) 사이에 형식 매개변수(1개 이상)를 넣어 선언

```kotlin
class box<T>(t: T) {
	var name = t
}

fun main() {
	val box1: Box<Int> = Box(1)
	val box1: Box<String> = Box("hello")
	println(box1.name)
	println(box2.name)
}
```

	형식 매개변수 이름 (대부분 Type을 줄인 T 사용. 어떤 알파벳도 사용 가능)
	E(요소 element), K(키 key), N(숫자 number), T(형식 type), V(값 value), S, U, V(2nd, 3rd, 4th types)

	제네릭을 사용하면 인자의 자료형을 고정할 수 없거나 예측할 수 없을 때 형식 매개변수인 T를 이용하여 실행 시간에 자료형을 결정할 수 있으므로 편리

- - -

#### 제네릭 클래스
	형식 매개변수를 1개 이상 받는 클래스. 클래스 선언할 때 자료형을 특정하지 않고 인스턴스를 생성하는 시점에서 클래스의 자료형을 정함.
	클래스 내의 매서드에도 형식 매개변수 사용 가능

```kotlin
class MyClass<T> {
	fun myMethod(a: T) {
		
	}
}
```

형식 매개변수를 프로퍼티에 사용하는 경우 클래스 내부에서는 불가 
(자료형이 특정되지 않으므로 인스턴스 생성할 수 없음)

```kotlin
// 불가능
class MyClass<T> {
	var myProp: T
}

// 주 생성자 이용
class MyClass<T>(val myProp: T) {

}

// 부 생성자 이용
class MyClass<T> {
	val myProp: T
	constructor(myProp: T) {
		this.myProp = myProp
	}
}

```



- - -
#### reified 키워드
	구체화하다 라는 뜻.
	런타임 시 불분명한 타입을 구체화 한다고 이해하면 된다.
	generic, inline fun, reified 함께 이해하는게 좋을 듯함. 
[[inline function]] 참고

[generic & reified 참고 블로그](https://jaeyeong951.medium.com/kotlin-generic-%ED%83%80%EC%9E%85-reified-1726e9a23d40)