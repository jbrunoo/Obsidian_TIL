클린 아키텍처(모듈화)

Model + View + Whatever는 클린 아키텍처에서 interface adapter 부분에 속함

[mvvm with clean architecture VS mvvm without clean architecture](https://stackoverflow.com/questions/58484680/what-is-difference-between-mvvm-with-clean-architecture-and-mvvm-without-clean-a)
view - UI 
viewmodel - Presenter/ViewModel (Business logic) 
model - Repository DataSource

presentation Layer - UI, Presenter/ViewModel
Domain/Business Layer - UseCase + Entity (Business logic), Repository(interface)
Data Layer - Repository(impl), DataSource, API

mvvm과 clean architecture의 구성은 위와 같고 mvvm은 presentation layer에 속한다고 함.
물론 이론이랑 또 구현 방법은 차이가 남.
