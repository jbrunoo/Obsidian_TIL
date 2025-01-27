[LineHeight 값 관련 문서](https://medium.com/@seoon53/compose-text-%ED%95%9C-%EC%A4%84%EC%9D%BC-%EB%95%8C-lineheight-%EC%A0%81%EC%9A%A9%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95-%EB%94%94%EC%9E%90%EC%9D%B8%EA%B3%BC-text-%EB%86%92%EC%9D%B4-%EB%A7%9E%EC%B6%94%EB%8A%94-%EB%B0%A9%EB%B2%95-365ee5967288)

[InnerShadow, DropShadow](https://citytexi.tistory.com/87)

color 투명도는 copy (alpha) 조정

letterSpacing % 값 처리 
```kotlin
/* figma에서 letterSpacing이 % 값인 경우, em으로 받아 처리 */
internal fun normalizeLetterSpacing(fontSize: TextUnit, letterSpacing: TextUnit): TextUnit =
    if (letterSpacing.isEm) fontSize.value.sp * (letterSpacing.value / 100) else letterSpacing
```
