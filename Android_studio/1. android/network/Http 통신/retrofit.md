[공식문서](https://square.github.io/retrofit/)

#### Converters 종류
Converters can be added to support other types. Six sibling modules adapt popular serialization libraries for your convenience.

- [Gson](https://github.com/google/gson): `com.squareup.retrofit2:converter-gson`
- [Jackson](https://github.com/FasterXML/jackson): `com.squareup.retrofit2:converter-jackson`
- [Moshi](https://github.com/square/moshi/): `com.squareup.retrofit2:converter-moshi`
- [Protobuf](https://developers.google.com/protocol-buffers/): `com.squareup.retrofit2:converter-protobuf`
- [Wire](https://github.com/square/wire): `com.squareup.retrofit2:converter-wire`
- [Simple XML](http://simple.sourceforge.net/): `com.squareup.retrofit2:converter-simplexml`
- [JAXB](https://docs.oracle.com/javase/tutorial/jaxb/intro/index.html): `com.squareup.retrofit2:converter-jaxb`
- Scalars (primitives, boxed, and String): `com.squareup.retrofit2:converter-scalars`

- [kotlinx.serialization](https://github.com/Kotlin/kotlinx.serialization) : 위 자바 라이브러리들은 data class의 default value를 무시하고 0, null로 초기화

[블로그](https://velog.io/@jeongminji4490/Android-Retrofit)


#### retrofit 구현 - 3가지 클래스
- Data Class
- Http 작업 정의하는 Interface
- Retrofit.Builder 선언한 Object(or ApiModule with hilt)

Manifest 인터넷 권한
모듈 수준 build.gradle에 retrofit 라이브러리와 conveter 추가(gson 사용)


#### retrofit 어노테이션
- 1. **http 메서드 정의**
	@ + GET, POST, PUT, DELETE, HEAD
	이 뒤에 지정된 경로는 BASE_URL 뒤에 추가되어 최종 서버 요청 URL이 됨.


- 2. **뒤에 지정된 경로에 필요한 어노테이션**
	@ + Path, Query, QueryMap, Body, FormUrlEncoded, Field, Header, Url

1. @Path
	URL의 경로를 동적으로 지정할 때.
	path param -> /{param}
	동적으로 들어갈 부분을 중괄호로 감싸고 변수에 어노테이션 추가.
	Query 뒤에 Path 추가하면 오류남. 모두 Path로 받아주었음.

2. @Query
	서버 요청 시 query를 키로 매개변수를 값으로 데이터 전달.
	key param -> /?param=
	```kotlin
// 경로에 ?를 이용해 데이터 전달
@GET("${BuildConfig.OPEN_API_KEY}/json/realtimeStationArrival/?START_INDEX=0&END_INDEX=5")
    fun getParkInfo(): Call<ParkInfoModel>

// @Query 이용
@GET("${BuildConfig.OPEN_API_KEY}/json/realtimeStationArrival")  
fun getSubwayInfo(  
    @Query("START_INDEX") startIndex: String,  
    @Query("END_INDEX") endIndex: String,  
    // @Path("statnNm") statnNm: String // path annotation은 query 뒤에 위치할 수 없음  
): Call<SubwayDTO>
```

3. @QueryMap
	전송할 데이터가 많을 때 MAP 타입으로 받기.
	`@QueryMap values: Map<String, String>`

4. @Body
	전송할 데이터를 모델 객체로 지정하고 싶을 때(@POST와 함께 사용).
	객체를 key, value 형태의 JSON 문자열로 만들어 전송.
	URL이 아닌 데이터 스트림으로 서버에 전송. ex) `{"userId" : "textId", "password" : "textPwd"}`

5. @FormUrlEncoded
	데이터를 URL 인코딩 형태로 만들어 전송할 때. (@POST와 함께, 위에 @Body는 JSON 전송)
	전송된 데이터 ex) `userId=textId&password=textPwd`

6. @Field
	인코딩 될 때 데이터에 추가하는 어노테이션.
	모델 객체에는 사용 불가 - 여러 데이터 지정 시 배열이나 List 사용.

7. @Header
	헤더 값 변경 시 `@Headers("Context-Type: application/json;charset=UTF-8")`

8. @Url
	 BASE_URL을 무시하고 전혀 다른 URI 지정 시.
```kotlin
@GET
fun getUserInfo(
	@Url url: String
): Call<UserModel>

val call = service.getUserInfo("$url")
 ```



ps. Callback [공식문서](https://square.github.io/retrofit/2.x/retrofit/retrofit2/Callback.html)



Call vs Response
응답 값으로 Call과 Response를 받을 수 있는데, 
Call을 사용하여 서버에 요청을 보내면 각각의 Call은 자체적으로 HTTP 요청과 응답 쌍을 생성.
execute()나 enqueue()을 통해 실행.
execute()는 동기적인데 메인스레드가 차단되기 때문에 권장 x
enqueue()는 비동기적 방식.
enqueue()는 콜백 메서드를 통해 onResponse, onFailure로 핸들링 가능.

Response는 요청 이후 응답 결과만 반환.
Response는 try ~ catch 등을 이용하여 response.isSuccessful 을 통해 각 케이스 핸들링 해주어야 함.

Call은 응답 결과의 성공 유무에 직관적인 처리. Response는 조금 더 간결한 코드 작성 가능.
RxJava 또는 Coroutine을 사용하여 처리하면 앞에 내용들을 작성할 필요 없긴 함.


- - -
okhttp 라이브러리를 같이 쓰는 이유
	Interceptor 기능 [#1](https://medium.com/@myofficework000/retrofit-interceptors-for-beginners-76943e987ad5)
		 HTTP 호출을 중간에서 가로챌 수 있는 Interceptor 기능 제공
		 API 호출 전후에 특정 로직을 실행할 수 있으며, 예를 들어 로깅, 헤더 추가, 캐싱 정책 설정 등이 가능  
	서버 통신 시간 조절


- - -
즉, 구현해야할 클래스는 apiService, Retrofit, (Okhttp)Client, Interceptor가 있음.

ApiService 인터페이스에서 API endPoint를 정의하고
Retrofit에서 위 인터페이스를 구현하여 네트워크를 호출 + url, 직렬화(Gson, Moshi, kotlinx.serialization 등)
네트워크 요청을 위해 OkHttpClient가 네트워크 요청을 수행하기 위한 HttpClient이며
Interceptor에서 네트워크 요청이나 응답을 가로채 수정할 수 있는 기능을 제공(공통 헤더 추가, 응답 로깅 등등)

di 모듈에 정의한 부분을 보면 다음과 같음
```kotlin
@Module
@InstallIn(SingletonComponent::class)
object ApiModule {
    @Provides
    @Singleton
    fun provideNetworkJson(): Json = Json {
        ignoreUnknownKeys = true
    }

    @Provides
    @Singleton
    fun provideQueryInterceptor(): QueryInterceptor {
        return QueryInterceptor()
    }

    @Provides
    @Singleton
    fun provideOkHttpClient(
        queryInterceptor: QueryInterceptor
    ): OkHttpClient =
        OkHttpClient.Builder()
            .addInterceptor(queryInterceptor)
            .build()

    @Provides
    @Singleton
    fun provideApiService(networkJson: Json, okHttpClient: OkHttpClient): Retrofit =
        Retrofit.Builder()
            .baseUrl(BuildConfig.BASE_URL)
            .addConverterFactory(networkJson.asConverterFactory("application/json".toMediaType()))
            .client(okHttpClient)
            .build()

    @Provides
    @Singleton
    fun provideRetrofitService(retrofit: Retrofit): EventService {
        return retrofit.create(EventService::class.java)
    }
}
```
