4, 3 좌표를 저장한다면 2차원 배열을 사용할 수 있지만
메모리를 줄이자 + 배열의 범위가  ex) 300이다. 
그럼 더 큰 수 ex) 1000 활용하여 저장.

```kotlin
val x = 4; val y = 3
val ad = ArrayDeque<Int>()
val pos = 1000 * x + y

ad.offer(pos)
val a = ad.peek() / 1000; val b = ad.poll() % 1000
```
 - - -

