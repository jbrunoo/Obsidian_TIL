객체 list를 객체로 받은 경우 발생 에러  [#1](https://stackoverflow.com/questions/63510038/moshi-expected-begin-object-but-was-begin-array-at-path)
Moshi expected BEGIN_OBJECT but was BEGIN_ARRAY at path $

com.squareup.moshi.JsonDataException: Required value 'proverbData' missing at $[1]  
@field:Json -> @Json 바꿔줌으로써 해결 [#2](https://stackoverflow.com/questions/72985108/moshi-jsondataexception-required-value-s-missing-at) [moshi issues](https://github.com/square/moshi/issues/796)
정확한 사유 모르겠으나 kotlin -> java 디컴파일 해보면 어노테이션 간 차이를 볼 수 있다고 함 [#3](https://stackoverflow.com/questions/56976656/moshi-in-kotlin-json-vs-fieldjson)


 