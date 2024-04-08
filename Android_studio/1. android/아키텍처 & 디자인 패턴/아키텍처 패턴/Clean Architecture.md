[nhn post](https://meetup.nhncloud.com/posts/345)
[medium 1](https://medium.com/mj-studio/%ED%81%B4%EB%A6%B0%EC%95%84%ED%82%A4%ED%85%8D%EC%B2%98-%EC%8D%BC%EB%8A%94%EB%8D%B0-%EC%99%9C-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8%EA%B0%80-%EB%8D%94-%EB%8D%94%EB%9F%AC%EC%9B%8C%EC%A7%80%EC%A7%80-3565aaffca8c)
[medium 2](https://medium.com/@BerkOzyurt/android-clean-architecture-mvvm-usecase-ae1647f0aea3)
[medium 3](https://markonovakovic.medium.com/clean-architecture-is-not-domain-data-presentation-e368d7ff8579)
[velog](https://velog.io/@ja2ykweon/%EA%B0%95%EC%97%B0-%EB%82%B4%EC%9A%A9-%EC%A0%95%EB%A6%ACNHN-FORWARD-22-%ED%81%B4%EB%A6%B0-%EC%95%84%ED%82%A4%ED%85%8D%EC%B2%98-%EC%95%A0%EB%A7%A4%ED%95%9C-%EB%B6%80%EB%B6%84-%EC%A0%95%ED%95%B4-%EB%93%9C%EB%A6%BD%EB%8B%88%EB%8B%A4)

[github 예시](https://github.com/Farhandroid/AndroidCleanArchitecture/blob/master/app/src/main/java/com/farhan/tanvir/androidcleanarchitecture/presentation/screen/home/HomeScreen.kt)
[쉽게 이해하기](https://yebon.kim/posts/2024-clean-architecture)

#### ★★★ 핵심 가치 
	1. 관심사의 분리
	2. 내부 원은 외부 원의 내용을 몰라야 함(의존성 규칙)
	이 2가지가 클린 아키텍쳐의 전부. 이외에 정해진 것이 없어 오히려 애매한 개념


#### 그래도.. 보편적인 구현 방법
###### data layer
	DTO(local/remote data), DB(ROOM, DAO), Repository(Impl), DataSource(데이터 가져오는 방식 정의)
###### domain layer
	Entity(Data Model - 실제 앱에서 쓰려는 데이터 클래스), Repository, Usecase(repo만으로 구분이 어려움. 실제 사용자가 사용할 각 비즈니스 로직) 
###### presentation layer
	UI, ViewModel, components, navigation, DI

#### 정리
	양 layer가 domain에 의존하고 있음
	presentation -> domain <- data 
	실제 데이터 흐름은
	screen -> viewModel -> useCase -> repo -> dataSource 라고 볼 수 있음


클린 아키텍처를 앱이 커졌을 때 추천하는데 일단 구조별로 나누기 때문에 더 많은 코드와 파일들이 생성된다.
깔끔하게 나뉜다는 느낌을 받을 수 있으나 이점을 못느끼는 이유는 앱이 크지 않을 때 단일 소스에서 참고하는 경우 repository와 data source가 같은 역할을 하고 있다거나 repo, usecase 사이에서도 이점을 잘 못 찾기 때문이다.
즉, repo에서 단일 data source가 아닌 다른 소스의 데이터도 처리해야한다거나 더 복잡할 경우 일부 repo는 다른 repo에 종속될 수도 있다. 즉, 앱이 커질수록 data source나 usecase는 단일 책임을 가지게 해야하고 repo에서 추상화 작업이 이뤄지며 repo의 impl을 data layer에서 interface를 domain layer에서 관리함으로 의존성 역전이 일어나고 dip 원칙을 지키고 있다고 볼 수 있다. (+ 위에서 정리한 데이터 흐름이 usecase -> repo이므로 domain에 repo interface 구현하고 repoImpl을 data에서 구현하므로써 의존성 역전 원칙으로 클린 아키텍처의 의존성을 지킴)
즉, 컴파일 타임에 repo impl이 빌드되므로 종속성을 반전시키지만 런타임에는 구현체는 이미 컴파일되어 있고 repo 메소드가 호출되므로 적절한 흐름을 유지할 수 있음


ps. 
	하나의 app 모듈보다 App 디렉토리와 같이 모듈 별로 나누고 각 Build gradle을 구성해 준다.(멀티 모듈화)
	mvvm과 같은 디자인 패턴은 presentation layer에서 함께 사용될 수 있다. 
	즉, 위 layer를 모듈별로 나누고 app/common/core 등의 패키지에 다른 구성요소들을 포함할 수 있음 ex) di, navigation, 공통 함수, util 등


![[Pasted image 20240408175323.png]]

 compose에서 mapper 부분은 dto에서 확장함수로 entity 구현
 