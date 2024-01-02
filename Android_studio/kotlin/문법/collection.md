list, set, map 등


collection 함수
sum(), multiple()



map, filter, any, partition
map은 모든 collection을 순회
filter는 람다식 조건에 맞는 새로운 collection 반환
any는 조건엠 만족하는 요소가 하나라도 있는지 true, flase 반환



reduce, fold : 컬렉션 요소를 누적해서 더하여 반환하는 함수
차이 : 초기 값(첫 번째 요소 / 파라미터), 반환 값(컬렉션 자료형 / 초기 값의 자료형)
```kotlin
public inline fun <S, T : S> Iterable<T>.reduce(
    operation: (S, T) -> S
): S

public inline fun <T, R> Iterable<T>.fold(
    initial: R,
    operation: (R, T) -> R
): R
```
fold 쓰는 것 권장.
1. emptyList의 경우 reduce는 exception 발생 - UnsupportedOperationException
2. ```val doubledSumFromZero = numbers.fold(0) { total, num -> total + num * 2 }``` 같은 연산에서 reduce는 total에 첫 번째 요소가 fold는 0이 들어가기 때문에 원하는 연산은 fold만 나옴.


컬렉션 부분 검색 [공식문서](https://kotlinlang.org/docs/collection-parts.html)