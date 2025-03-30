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

	ps. @DELETE에 @Body를 사용할 수 없다. 보통은 query로 전송하나 @Body로 보내야할 경우,
	`@HTTP(method = "DELETE", path = "경로", hasBody = true)` 해당 방식으로 정의해야 한다.

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


[@MultiPart](https://square.github.io/retrofit/2.x/retrofit/retrofit2/http/Multipart.html) - 단일 요청에서 파일 업로드 및 여러 유형의 처리에 쓰인다.
일반적으로 multipart/form-data 형식이 텍스트와 함께 이미지(바이너리 파일), 동영상 등을 같이 보낼 수 있음.
파라미터에는 @Part 어노테이션을 붙인다.

```kotlin
@Multipart  
@PATCH("members/profile")  
suspend fun patchProfile(  
    @Part("nickname") nickname: RequestBody?,  
    @Part("selfIntroduction") selfIntroduction: RequestBody?,  
    @Part profileImage: MultipartBody.Part?,  
): Response<BaseResponse<PatchProfileResponse>>
```

@Part의 파라미터의 타입에 따라 사용 방식이 조금 다르다.
RequestBody일 경우 contentType과 같이 사용. 어노테이션 이름 명시. - ex) ("nickname")
MultipartBody.Part인 경우 content가 직접 사용. 어노테이션 이름 생략.
다른 객체 타입인 경우, converter를 사용하고 어노테이션 이름 명시.

ps. [[UriToFile]]