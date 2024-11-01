단위 테스트를 위한 라이브러리
[참고 유튜브](https://www.youtube.com/watch?v=tyZMdwT3rIY&t=907s)
[JUnit 5 User Guide](https://junit.org/junit5/docs/current/user-guide)
[AssertJ User Guide](https://assertj.github.io/doc)
[AssertJ Exception Assertions](https://www.baeldung.com/assertj-exception-assertion)
[Guide to JUnit 5 Parameterized Tests](https://www.baeldung.com/parameterized-tests-junit-5)


Junit platform + Junit Jupiter(Junit5) + Junit Vintage(Junit4 미만)로 이루어져있음. 


기본적인 사용
테스트 파일은 클래스 명 + test로 만든다.
어노테이션을 활용한다.(@Before, @Test, @After)
junit.Assert 라이브러리를 활용하여 테스트를 수행한다.(assertEquals, ) [메서드 참고](https://has3ong.github.io/programming/java-junit5assert/)


자주 발생하는 Exception의 경우 Exception별 메서드를 제공한다.
- assertThatIllegalArgumentException() 
- assertThatIllegalStateException() 
- assertThatIOException() 
- assertThatNullPointerException()


@Nested - 하나의 테스트 파일에 inner class로 테스트로 또 묶어주기 위해 사용
@ParameteriedTest + @ValueSource - 여러 inputs에 대한 검증
ps. @CsvSource
```java
@ParameterizedTest @CsvSource(value = {"test:test" , "tEst:test" , "Java:java"}, delimiter = ':') 
void toLowerCase_ShouldGenerateTheExpectedLowercaseValue(String input, String expected) {
```

