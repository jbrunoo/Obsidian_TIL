범위 지정 함수. 
apply, with, let, also, run

2가지 구성요소 : 수신 객체, 수신 객체 지정 람다(lambda with receiver)

**inline fun** <T, R> with(receiver: T, block: T.() -> R): R {**return** receiver.block()  
}


