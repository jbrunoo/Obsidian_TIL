
[kotlin](https://velog.io/@akimcse/Kotlin-in-Action-6.-%EC%BD%94%ED%8B%80%EB%A6%B0-%ED%83%80%EC%9E%85-%EC%8B%9C%EC%8A%A4%ED%85%9C)

kotlin in action


object는 class를 하나만 정의, 싱글톤 개념 (?)
Any, Any?는 최상위 타입
자바에서 object가 클래스 계층의 최상위 타입이듯, 코틀린에서 Any 타입이 모든 null이 될 수 없는 타입의 조상 타입.


[위임 프로퍼티 참고 블로그](https://0391kjy.tistory.com/61)
by -> 프로퍼티 델리게이트 property delegate 


lazy 초기화하지 않다가 그 값이 실제로 쓰일 때 초기화하기 위해 쓰임.
ex) 초기화 과정에서 리소스를 많이 잡아먹거나 객체를 사용할 때마다 초기화하지 않아도 되는 경우

\_변수명 -> backing property라고 부름. 읽기 기능만 제공. viewModel에서 자주 쓰는..


람다 함수
익명 함수
참조 함수

