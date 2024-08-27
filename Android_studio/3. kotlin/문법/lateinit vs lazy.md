둘 다 지연 초기화를 위해 사용

lateinit var 형태, var만 가능 + 원시 타입은 쓸 수 없음. nullable x
by lazy 형태, val만 가능. nullable
ps. 위임 프로퍼티인 by 키워드. (getter/setter를 특정 변수에 위임)

응용 버전
```kotlin
lateinit var text: String
val textLength: Int by lazy { text.lenghth }
```
