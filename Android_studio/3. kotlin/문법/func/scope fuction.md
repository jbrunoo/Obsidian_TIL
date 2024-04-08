[공식 문서](https://kotlinlang.org/docs/scope-functions.html)


#### 범위 지정 함수 (apply, with, let, also, run)
	특정 '객체의 컨텍스트 안'에서 코드 블록을 실행하는 것이 유일한 목적인 함수들
	스코프 내에서 객체 이름 참조 없이 객체에 접근하고 핸들링 가능

	모두 동일한 작업 수행. 차이점은 이 객체가 블록 내에서 사용 가능해지는 방식과 전체 표현식의 결과

	2가지 구성요소 : 수신 객체, 수신 객체 지정 람다(lambda with receiver)
	
```kotlin
inline fun <T, R> with(receiver: T, block: T.() -> R): R { 
	return receiver.block()  
}
```


	apply

```kotlin
inline fun <T> T.apply(block : T.() -> Unit): T {
		block()
        return this
 }
```

	객체를 새로 생성하고 특정 변수에 할당하기 전에 초기화 작업을 하는 스코프를 만든다.
	apply 함수 내에 모든 명령들이 수행되면 명령들이 적용되어 새로 생성된 객체를 반환
	반환 결과가 객체 자신
	확장 함수이기 때문에 객체 컨텍스트를 receive(this)로 이용 가능


	run

```kotlin
inline fun <T,R> T.run(block : T.() -> R) : R {
		return block()
    }
```

	apply와 명확하게 다른 점은 반환하는 값이 객체가 아닌 스코프 내에 실행된 값
	특정 객체의 프로퍼티를 출력하거나 계산 값으로 활용하는 등의 핸들링 경우 사용
	확장 함수이기 때문에 receive(this) 이용 가능
	safe call을 통해 non-null일 때만 실행 가능


	let







| Function                                                                  | Object reference | Return value   | Is extension function                        |
| ------------------------------------------------------------------------- | ---------------- | -------------- | -------------------------------------------- |
| [`let`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/let.html)     | `it`             | Lambda result  | Yes                                          |
| [`run`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/run.html)     | `this`           | Lambda result  | Yes                                          |
| [`run`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/run.html)     | -                | Lambda result  | No: called without the context object        |
| [`with`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/with.html)   | `this`           | Lambda result  | No: takes the context object as an argument. |
| [`apply`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/apply.html) | `this`           | Context object | Yes                                          |
| [`also`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/also.html)   | `it`             | Context object | Yes                                          |

###### short guide on the intended purpose
	- Executing a lambda on non-nullable objects: `let`
    
	- Introducing an expression as a variable in local scope: `let`
    
	- Object configuration: `apply`
    
	- Object configuration and computing the result: `run`
    
	- Running statements where an expression is required: non-extension `run`
    
	- Additional effects: `also`
    
	- Grouping function calls on an object: `with`