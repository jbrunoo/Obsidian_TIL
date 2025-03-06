중국인의 나머지 정리

연립 합동식의 해의 존재성과 유일성 증명

증명 관련 개념
1. 쌍마다 서로소
	각 mod의 곱에 대해 유일한 해를 가짐.
2. 확장 유클리드 호제법


결론: 서로소인 n개의 수 각각에 일정한 나머지를 만족하는 수는 그들 n개의 최소공배수에 일정한 나머지를 더한 값


```kotlin
// boj 6064

private fun calc(m: Int, n: Int, x: Int, y: Int): Int {
    var sum = x
    val lcm = m * n / gcp(m, n)

    while (lcm >= sum) {
        if ((sum - y) % n == 0) return sum
        sum += m
    }

    return -1
}

```

(sum - x) % m == 0 || (sum - y) & n == 0.
위는 시간 초과로 인해 서로소 2개이므로 x += m으로 하나를 고정시켜두고 y만 검사.