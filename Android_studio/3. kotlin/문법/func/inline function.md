고차 함수는 함수(람다)를 함수의 호출 인자로 사용하거나, 반환 값으로 활용한다.


이렇게 람다를 사용하게 되면, 부가적인 메모리 할당으로 인해 메모리 효율이 안 좋아지고, 함수 호출로 인한 런타임 오버헤드가 발생하게 된다.


[블로그1](https://velog.io/@haero_kim/Kotlin-Inline-Function-%ED%8C%8C%ED%97%A4%EC%B9%98%EA%B8%B0) [블로그2](https://sabarada.tistory.com/176) 참고


람다를 사용하여 작성한 코드를 컴파일 한 Java 코드를 보게 되면,
람다식을 전달하여 구현한 부분을 새로운 객체를 생성하여 넘겨주는 방식으로 변환된다.
넘긴 객체를 통해 함수를 호출한다.
무의미한 객체 생성 - 메모리 차지, 내부적 연쇄적인 함수 호출 - 오버헤드 발생 가능


즉, 이러한 방식이 아닌 람다식 내부의 실행문(코드)들을 컴파일 시 람다를 호출하는 부분에 주입하는 것이 inline function


inline 키워드를 fun 앞에 붙여주면 됨.
그러면 컴파일한 java 코드에서 람다를 호출하는 부분에 람다식 내부의 코드가 그대로 복사되어 있음.
but, 코드를 그대로 가져갔기 때문에 인자로 전달받은 함수를 다른 함수로 전달하거나 참조할 수 없다.
inline 함수의 람다 인자들 중 다른 함수로 넘겨주려면 인자 앞에 noinline 키워드를 붙여 해결할 수 있다.



- - -

```kotlin
class EventRepositoryImpl @Inject constructor(private val eventRemoteDataSource: EventRemoteDataSource) :  
EventRepository {  
override fun fetchEventsByCodeFlow(  
codeName: String,  
title: String  
): Flow<Resource<List<Event>>> {  
return eventRemoteDataSource.fetchEventsByCodeNameFlow(codeName = codeName, title = title)  
.map { eventResponse ->  
val result = eventResponse.culturalEventInfo.result  
val row = eventResponse.culturalEventInfo.row  
  
if(result.code == "INFO-000") {  
Timber.d("SUCCESS")  
val events = row.map { it.toEvent() }  
Resource.Success(events)  
} else {  
handleResult(result = result)  
}  
}  
}  
  
override fun fetchRecentEventsFlow(endIndex: Int): Flow<Resource<List<RecentEventImage>>> {  
return eventRemoteDataSource.fetchRecentEventsFlow(endIndex = endIndex).map { eventResponse ->  
val result = eventResponse.culturalEventInfo.result  
val row = eventResponse.culturalEventInfo.row  
  
if(result.code == "INFO-000") {  
Timber.d("SUCCESS")  
val eventImages = row.map { it.toRecentEventImage() }  
Resource.Success(eventImages)  
} else {  
handleResult(result = result)  
}  
}  
}  
  
private inline fun <reified T> handleResult(result: RESULT): Resource<T> {  
return try {  
when (result.code) {  
"INFO-200" -> {  
Timber.d("INFO-200")  
Resource.Error(message = result.message)  
}  
"ERROR-500", "ERROR-600", "ERROR-601" -> {  
Timber.d("SERVER-ERROR")  
Resource.Error(message = "서버 데이터 점검 중 입니다.")  
}  
else -> {  
Timber.d("OTHER ERRORS")  
Resource.Error(message = "데이터 요청에 실패하였습니다")  
}  
}  
} catch (e: HttpException) {  
Timber.d("HTTP EXCEPTION")  
Resource.Error(e.localizedMessage ?: "HTTP EXCEPTION")  
} catch (e: IOException) {  
Timber.d("IO EXCEPTION")  
Resource.Error("IO EXCEPTION")  
}  
}  
}
```

프로젝트 중 handleResult의 코드가 위 2가지 메서드에 공통적으로 들어가는데 두 메서드의 반환 타입이 다르다보니 제네릭으로 생성해야 하는 경우가 생겼음. gpt가 inline, reified(뜻: 구체화된)를 포함한 코드를 만들어주었음.

Q. inline을 왜 쓸까?
위에 설명했지만 함수를 람다/고차 함수로 사용할 때, inline을 쓰지 않으면 컴파일 시 새로운 객체를 생성하여 파라미터로 넘겨줌. => 무의미한 객체 생성, 오버헤드 발생
inline을 사용하면 inline함수를 호출한 함수 내에 inline 함수의 내용을 넣는 방식.


Q. 그럼 다 inline 함수로 만들어야 하는거 아닌지?
컴파일 시 코드 크기 증가, 컴파일 시간 증가와 같은 부작용. 메모리 사용량, 캐시를 더 많이 사용함.
또한, 기본적인 일반 함수 호출의 경우에는 jvm(컴파일러에 의해)이 강력한 인라이닝을 지원함.
	ps. 안드로이드는 dvm, art 이긴 하고 컴파일러에는 JIT와 AOT 있는데 딥하니 추후 정리..
코드 크기를 주의하며 람다 인자와 무관한 코드는 별도의 비인라인 함수로 빼내는 것이 좋음.
	ps. 어느 블로그에서는 1~3 줄의 인라인 함수를 사용한다고 적혀있는 것을 봄
public inline 함수는 private 함수에 접근할 수 없기도 함


Q. reified는 무엇이고 왜 inline과 함께 쓰는가?
이전에 런타임 시 제네릭 동작을 알 필요가 있음.
jvm의 제네릭은 타입 소거(type erase/erasure)를 사용하여 구현됨.
	컴파일 타임에만 타입에 대한 제약 조건을 적용하고 런타임에는 타입 정보를 제거함
함수를 inline으로 만들면 타입 파라미터가 실체화되어 타입 파라미터가 소거되지 않게 할 수 있음.
여기서 타입 파라미터를 reified로 지정하면 제네릭을 실체화하여 런타임 시점에 알 수 있음.
	ps. reified가 없으면 Class<\T>를 전달하여 타입 소거 문제를 방지해야 함
