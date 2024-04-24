아래 에러
```
kotlinx.serialization.json.internal.JsonDecodingException: Expected class kotlinx.serialization.json.JsonObject (Kotlin reflection is not available) as the serialized body of kotlinx.serialization.Polymorphic<Flow>, but had class kotlinx.serialization.json.JsonLiteral (Kotlin reflection is not available)
```

EventService의 반환 값을 Flow<\EventResponse> -> EventResponse로 바꿈.
flow 공부할 때 바로 연결해줄 수 있다고 한 것 같은데 kotlinx.serialization을 사용많이 안해봐서 잘모르겠음.


[공식문서 참고](https://github.com/Kotlin/kotlinx.serialization/blob/master/docs/json.md#array-wrapping)

직접 `@Serializable(with = ListSerializer::class)` 만들어주어야 함

