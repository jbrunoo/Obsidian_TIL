model - view - controller (스프링의 구조로 유명한)

우테코 프리코스 2주차 미션을 mvc로 해결하였다. [참고](https://github.com/woowacourse-precourse/kotlin-racingcar-7/pull/44/files)

 다양한 플랫폼에서 여러 정의와 잘못된 내용이 뒤섞여있다고 한다.
[참고 테코톡](https://youtu.be/ogaXW6KPc8I?feature=shared)이 제일 나의 생각과 비슷했다.

- - -

사실 MV+X 패턴에서는 모델과 뷰는 고정적이고 X를 무엇으로 관리할 것인지, 의존성이 어떻게 이루어져있는지, 양방향인지 단일방향인지 등의 차이를 정한다고 볼 수 있다. + 모델은 다른 파트에 의존성이 있으면 안된다.

MVC는 컨트롤러가 모델과 뷰를 의존하며 비대해진다는 특징을 지니고 있다.

구현할 때 주의할 점은 모델과 뷰, 컨트롤러의 본질적인 역할을 생각하며 코딩해야 한다는 것이다.

<br>
고민한 부분은 
1. `모델의 객체 메서드로 들어가야할지 비즈니스 로직으로 판단되어야 할지` - 기능별 역할의 의미를 생각하는게 도움된다.
2. `뷰에서 모델을 의존할 수 있느냐` 

2번 예시)
```
package racingcar.view

import racingcar.model.Car
```

뷰에서 모델이 import 되어 있다니 처음에는 기피하였다. MVVM 패턴을 먼저 공부하였기 때문일수도..

```kotlin
data class Car(val name: String) {  
    var moves = 0  
        private set  
  
    fun move() = moves++  
}
```
이렇게 Car를 정의하였는데 OutputView에서 형식이 `"${car.name} : ${"-".repeat(car.moves)}"` 였다.
그렇다면 view 메서드에 Car 클래스를 import 해주어야 할지 컨트롤러에서 car.name, car.moves를 구조 분해해서 값을 보내줘야할지 고민이 되었다.

이는 [참고 테코톡](https://youtu.be/ogaXW6KPc8I?feature=shared)의 발표자가 5번째 규칙으로
`view가 model로부터 데이터를 받을 때 반드시 controller를 통해서 받아야 한다`고 언급하고 있다.
즉, 모델에서 직접 의존이 아닌 컨트롤러를 통해 모델의 형식만 파라미터로 빌려준다는 뜻으로 이해하고
controller를 통해 List\<Car>를 넘겨주는 것으로 구현하였다.

```kotlin
// controller 내
private fun showRoundsResult() {  
    outputView.printStartMassage()  
    race.play { carList ->  
        outputView.printRoundResult(carList)  
    }  
}

// view 내
fun printRoundResult(carList: List<Car>) {  
    val sb = StringBuilder()  
    carList.forEach { car ->  
        sb.appendLine(car.formatRoundResult())  
    }  
    println(sb)  
}

private fun Car.formatRoundResult() = "$name : ${"-".repeat(moves)}"
```