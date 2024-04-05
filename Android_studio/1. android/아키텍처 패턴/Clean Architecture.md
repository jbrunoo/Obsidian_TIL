[nhn post](https://meetup.nhncloud.com/posts/345)
[medium 1](https://medium.com/mj-studio/%ED%81%B4%EB%A6%B0%EC%95%84%ED%82%A4%ED%85%8D%EC%B2%98-%EC%8D%BC%EB%8A%94%EB%8D%B0-%EC%99%9C-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8%EA%B0%80-%EB%8D%94-%EB%8D%94%EB%9F%AC%EC%9B%8C%EC%A7%80%EC%A7%80-3565aaffca8c)
[medium 2](https://medium.com/@BerkOzyurt/android-clean-architecture-mvvm-usecase-ae1647f0aea3)
[medium 3](https://markonovakovic.medium.com/clean-architecture-is-not-domain-data-presentation-e368d7ff8579)
[velog](https://velog.io/@ja2ykweon/%EA%B0%95%EC%97%B0-%EB%82%B4%EC%9A%A9-%EC%A0%95%EB%A6%ACNHN-FORWARD-22-%ED%81%B4%EB%A6%B0-%EC%95%84%ED%82%A4%ED%85%8D%EC%B2%98-%EC%95%A0%EB%A7%A4%ED%95%9C-%EB%B6%80%EB%B6%84-%EC%A0%95%ED%95%B4-%EB%93%9C%EB%A6%BD%EB%8B%88%EB%8B%A4)

[github 예시](https://github.com/Farhandroid/AndroidCleanArchitecture/blob/master/app/src/main/java/com/farhan/tanvir/androidcleanarchitecture/presentation/screen/home/HomeScreen.kt)
[쉽게 이해하기](https://yebon.kim/posts/2024-clean-architecture)

보편적인 설계 : App 디렉토리와 같이 모듈 별로 나누고 각 Build gradle을 구성해 준다.
mvvm과 같은 아키텍처와 함께 사용할 수 있다.


★ 핵심 가치 == 관심사의 분리.

#### 보편적인 구현 방법
###### data layer
	DTO(local/remote data), DB(ROOM, DAO), Repository(Impl), DataSource(repo에서 정의한 함수들의)
###### domain layer
	Entity(Data Model - 실제 앱에서 쓰려는 데이터 클래스), Repository, Usecase(각 repo에 정의된 ) 
###### presentation layer
	UI, ViewModel, components, navigation, DI


클린 아키텍처를 앱이 커졌을 때 추천하는데 구조별로 나누기 때문에 더 많은 코드와 파일들이 생성되기 때문.
깔끔하게 나뉜다는 느낌을 받을 수 있으나 이점을 못느끼는 이유는 앱이 크지 않을 때 단일 소스에서 참고하는 경우 repository와 data source가 같은 역할을 하고 있다거나 repo, usecase 사이에서도 이점을 잘 못 찾기 때문.
즉, repo에서 단일 data source가 아닌 다른 소스의 데이터도 처리해야한다거나 더 복잡할 경우 일부 repo는 다른 repo에 종속될 수도 있다. 즉, 앱이 커질수록 data source나 usecase는 단일 책임을 가지게 해야하고 repo에서 추상화 작업이 이뤄지며 repo의 impl을 data layer에서 interface를 domain layer에서 관리함으로 의존성 역전이 일어나고 dip 원칙을 지키고 있다고 볼 수 있음.

ps. 멀티 모듈화를 진행한다면 위 layer를 모듈별로 나누고 app/common/core 등의 패키지에 다른 구성요소들을 포함할 수 있음 ex) di, navigation, 공통 함수, util 등

