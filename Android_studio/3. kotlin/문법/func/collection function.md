colletion == list, set, map 등


collection 함수 - 표준 라이브러리에서 제공하며 확장 함수 형태로 제공.
(자바8에서 스트림 기능, 스트림 함수라고 부름)
sum(), multiple()(?)
sumBy {}
sumOf {} : kotlin 1.4 ? 이후 sumByDouble 등 int, long, double에 대한 각각 연산을 오버로딩해서 한꺼번에 수행 가능함. 성능은 비슷.




map, filter, any, partition
map은 모든 collection을 순회
filter는 람다식 조건에 맞는 새로운 collection 반환
any는 조건에 만족하는 요소가 하나라도 있는지 true, flase 반환



reduce, fold : 컬렉션 요소를 누적해서 연산하여 반환하는 함수. 즉, 누산기. 
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
3. 인자 값 명 추천. total num이나 acc(accumulator), element || next


컬렉션 부분 검색 - [공식문서](https://kotlinlang.org/docs/collection-parts.html)
Slice, Take and Drop, Chunked, Windowed

take, drop 
take(n) : List<\T>  : 앞에서부터 n만큼의 원소를 취해 새로운 List를 반환하는 확장 함수.
takeWhile(predicate: (T) -> Boolean) : List<\T> : predicate 조건이 false 될 때까지 인수를 취해 반환.
takeLast(n): List<\T> :  뒤에서부터 n만큼의 원소를 취해 List 반환.
takeLastWhile(...)
takeIf(predicate) : 수신객체가 특정 조건을 만족하는지 확인하여 만족하면 수신객체 불만족하면 null 반환.
	(collection 만을 위한 확장함수는 아님.)

drop : 앞에서부터 n만큼의 원소를 제거하고 새로운 List를 반환하는 확장 함수.
(dropWhile, dropLast, dropLastWhile)


Chunked(n) : 단일 인자로 chunk size를 주면 주어진 사이즈의 list를 반환.
```kotlin
val numbers = (0..13).toList()
println(numbers.chunked(3))
// [[0, 1, 2], [3, 4, 5], [6, 7, 8], [9, 10, 11], [12, 13]]

println(numbers.chunked(3) { it.sum() })  // `it` is a chunk of the original collection
// [3, 12, 21, 30, 25]
```


groupBy, groupingBy (eachCount)


[BuildList](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/build-list.html#kotlin.collections$buildList(kotlin.Int,%20kotlin.Function1((kotlin.collections.MutableList((kotlin.collections.buildList.E)) 


https://heegs.tistory.com/52