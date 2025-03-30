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