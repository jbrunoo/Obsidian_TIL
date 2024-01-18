[공식문서](https://square.github.io/retrofit/)

Converters 종류
Converters can be added to support other types. Six sibling modules adapt popular serialization libraries for your convenience.

- [Gson](https://github.com/google/gson): `com.squareup.retrofit2:converter-gson`
- [Jackson](https://github.com/FasterXML/jackson): `com.squareup.retrofit2:converter-jackson`
- [Moshi](https://github.com/square/moshi/): `com.squareup.retrofit2:converter-moshi`
- [Protobuf](https://developers.google.com/protocol-buffers/): `com.squareup.retrofit2:converter-protobuf`
- [Wire](https://github.com/square/wire): `com.squareup.retrofit2:converter-wire`
- [Simple XML](http://simple.sourceforge.net/): `com.squareup.retrofit2:converter-simplexml`
- [JAXB](https://docs.oracle.com/javase/tutorial/jaxb/intro/index.html): `com.squareup.retrofit2:converter-jaxb`
- Scalars (primitives, boxed, and String): `com.squareup.retrofit2:converter-scalars`


[블로그](https://velog.io/@jeongminji4490/Android-Retrofit)


retrofit 구현 - 3가지 클래스
- Data Class
- Http 작업 정의하는 Interface
- Retrofit.Builder 선언한 Object

Manifest 인터넷 권한
모듈 수준 build.gradle에 retrofit 라이브러리와 conveter 추가(gson 사용)

