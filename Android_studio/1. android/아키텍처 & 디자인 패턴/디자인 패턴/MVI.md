Model
View
Intent - android intent 아님.. (mvvm의 vm도 AAC의 viewmodel 아님..)

바뀐 것은 Intent 즉, 의도(사용자 액션 및 시스템 이벤트와 따른 결과)
구조 자체는 mvvm처럼 viewmodel을 이용해 상태 관리를 함.
ui state, ui event, ui effect 3가지 인터페이스를 활용한 viewmodel 구성.
각각을 stateFlow, sharedFlow, Channel로 관리

상관관계는 순수 함수 형식으로 표현: view(model(Intent()))

단방향 구조: user -> Intent -> model -> view -> user -> intent ...

\+ Side Effect
순수 함수 순환으로 처리할 수 없는 일들 - 오래 걸리는 I/O, API 통신, 백그라운드, 토스트 등
Intent와 Model 사이 존재

MVI 구현을 위해 선택할 수 있는 라이브러리
Mavericks, MVIKotlin, Orbit, Redux-Kotlin, Roxie, Uniflow

- - -
MVVM과 구현에서의 차이는 viewModel에서 상태와 이벤트(액션) 관리를 어떻게 하는지에서 나타남

MVVM에서 state를 각각 언더바를 이용하여 하나씩 정의해주었다면
MVI에서는 data/sealed class로 state, sealed interface로 이벤트/액션을 정의해줌

