[공식문서 returns and jumps](https://kotlinlang.org/docs/returns.html#return-to-labels)
kotlin의 모든 표현식에는 라벨을 설정할 수 있다.

forEach 람다식에서 break, continue 사용 방법 - scope func run + label@ 활용
```kotlin
fun main() {  
    val list = listOf(1, 2, 3, 4, 5)  
    val list2 = listOf('a', 'b', 'c')  
  
    fun example() {  
        list.forEach { f ->  
            run stop@ {  
                list2.forEach { s ->  
                    if (f > 1) return@forEach // continue 효과
                    if (f > 2) return@stop // break 효과
                    if (f > 3) return // example 함수 종료
                    println(s)  
                }  
            }        
        }   
        println("test")
     }  

    example()  
}
```

scope 없이도 가능
```kotlin
list.forEach li@ {
	return@li
}
```