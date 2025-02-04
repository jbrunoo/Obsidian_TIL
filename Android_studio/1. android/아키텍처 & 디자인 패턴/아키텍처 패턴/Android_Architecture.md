android developer 공식 문서 상 권장 아키텍처에서는 domain layer는 선택 사항.

핵심 가치  : UDF(단방향 흐름)

UI Layer -> Domain Layer(optional) -> Data Layer

UI Layer() -> Data Layer (events)
	event -> 사용자가 화면에 새로운 결과 스크롤, 페이지 고침, 새 게시물 저장, 시스템이 앱에 인터넷 연결 끊김 알림, 헤드셋 연결 끊김 알림 등 발생.
	사용자 또는 Android 시스템이 시작함으로 UI Layer에서 제공.
Data Layer -> UI Layer (Data)
	이러한 이벤트를 수신하고 적절한 처리. 
	새로운 데이터가 앱에 저장되거나 인터넷의 추가정보가 로드될 수 있음.
	데이터 계층에 업데이트된 앱 상태 or 업데이트된 데이터가 있으면 UI 계층과 같은 데이터의 모든 관찰자가 업데이트됨.
	
UDF(Undirectional Data Flow) 단방향 데이터 흐름.
	일관성 보장, 디버깅하기 쉬운 단방향 데이터 흐름 패턴 준수.

# UI Layer [참고](https://developer.android.com/jetpack/guide/ui-layer?hl=ko)
view or 작업 수행한 API 상관없이 데이터를 표시하는 activity, fragment 같은 UI 요소를 나타냄.
데이터 계층 - 앱 데이터에 대한 액세스를 보유, 관리, 제공.
즉, 
1. App data -> UI data
2. UI data -> UI elements
3. User events -> UI changes
4. Repeat

## UI Layer Concepts
1. Define UI state - UI 상태 정의
2. Production of UI state - UI 상태 생성 관리
3. Expose UI state - 단방향 데이터 흐름 원칙에 따라 관찰 가능한 데이터 유형으로 UI 상태 노출
4. Consume UI state - 관찰가능한 UI 상태를 사용하는 UI 구현


1. UI Elements + UI state = UI, UI는 UI 상태에 대한 모든 변경 사항을 즉시 반영.
	UI를 완전히 렌더링하는데 필요한 정보는 데이터 클래스에 캡슐화될 수 있음.
	UI 상태 정의는 변경할 수 없기에 단일 역할에 집중할 수 있음.
	상태를 읽고 그에 따라 UI 요소 업데이트.
	즉, UI 자체가 해당 데이터의 유일한 소스가 아닌 한 UI에서 UI 상태를 직접 수정해서는 안됨.
	위반하면 동일한 정보에 대해 여러가지 진실 소스가 생성됨 / 데이터 불일치 및 미묘한 버그 발생
2. UI 상태가 생성되면 이를 업데이트하는 방법
	앱 데이터는 데이터 업데이트나 사용자 작업으로 인해 시간이 지나면서 변경.
	즉, 상태 관리를 별도의 유형에 위임하는 것이 유용한 경우가 많음.
	UI가 UI이자 상태 관리자가 되는 것을 방지할 수 있음.
	이 별도의 유형을 state holder(상태 홀더)라고 함.

### State holder 
	상태를 생성하는 UI 요소에 따라 다양한 모양과 크기로 제공됨.
	screens, activities, fragments, navigation 대상의 경우, 
	viewModel 인스턴스는 일반적으로 기본 상태 홀더 

요약 : Data Layer -> ViewModel (current app data)
	viewModel -> UI Elements (current UI state)
	UI Elements -> viewModel (Ul events)
	viewModel -> Data Layer (viewModel notifies state change)
	Data Layer (persists the data changes and updates the application data)
	Data Layer -> viewModel (new app data)
	viewModel -> Data Layer (new UI state)

UI 상태가 변경 되었음을 어떻게 알 수 있는지?
=> 관찰 가능한 데이터(stateflow or livedata 같은 관찰 가능한 데이터 홀더에 노출되어야 함)
이를 통해  UI는 viewModel에서 데이터를 수동으로 가져오지 않고도 상태의 모든 변경 사항에 반응 가능.

viewModel을 쓰는 정확한 이유.
activity 수명 주기와 구성 변경과 관련된 수명 주기 문제에서 구성 변경 시 rememberSaveable 사용하거나 인스턴스 상태를 저장하는 등 다양한 방법으로 앱 데이터 저장할 수 있음.
대부분 `rememberSaveable`로 사용가능하나 composable 함수 내 또는 근처에 로직을 유지해야 함.
즉, 앱이 성장하면 데이터와 로직을 composable 함수와 분리해야 하기에.

정리 : UI Layer는 두 가지로 구성
1. 화면에 데이터를 렌더링하는 요소. 뷰 or jetpack compose 함수 사용
2. 데이터를 보유하고 이를 UI에 노출하는 로직을 처리하는 상태 홀(ex) viewModel 클래스)

# Data Layer [참고](https://developer.android.com/jetpack/guide/data-layer?hl=ko)
비즈니스 로직이 포함됨. 비즈니스 로직은 앱에 가치를 부여하는 요소. 
앱의 데이터 생성, 저장, 번경 방을 결정하는 규칙으로 구성

Data Layer는 0개부터 여러 개의 데이터 소스를 포함할 수 있는 저장소로 구성.
앱에서 처리하는 다양한 유형의 데이터별로 저장소 클래스를 만들어야 함.
	ex) 영화 관련 데이터 - MovieRepository 클래스, 결제 - PaymentRepository 클래스

Repository 클래스 담당 작업
	앱의 나머지 부분에 대한 데이터 노출
	데이터 변경사항을 한 곳에 집중
	여러 데이터 소스 간의 충돌 해결
	앱의 나머지 부분에서 데이터 소스 추상화
	비즈니스 로직 포함

Data Sources 클래스는 파일, 네트워크 소스, 로컬 데이터베이스와 같은 하나의 데이터 소스만 사용해야 함. 데이터 작업을 위해 애플리케이션과 시스템 간의 가교 역할.


API 노출
데이터 영역의 클래스는 일반적으로 원샷 생성, 조회, 업데이트 및 삭제(CRUD) 호출 실행 or 시간 경과에 따른 데이터 변경 사항에 대한 알림을 받는 함수를 노출.
1. 원샷 작업 : 데이터 영역에서 Kotlin의 suspend 함수를 노출해야 함. java에서는 RxJava Single, Maybe or Completable 유형에 대한 콜백 제공하는 함수를 노출해야 함.
2. 시간 경과에 따른 데이터 변경사항에 관한 알림을 받으려면 : 데이터 영역에서 Kotlin의 [flow](https://developer.android.com/kotlin/flow?hl=ko)를 노출해야 함. java에서는 데이터 영역에서 새 데이터 or RxJava Observable or Flowable 유형을 내보내는 콜백을 노출해야 함.
ps. 코틀린 flow는 단일 값 반환하는 suspend 함수와 달리 여러 값을 순차적으로 내보낼 수 있는 유형. flow를 사용하여 db에서 실시간 업데이트를 수신할 수 있음. flow는 코루틴 기반 빌드되며 여러 값 제공 가능. 비동기식으로 계산할 수 있는 데이터 스트림 개념. 내보낸 값은 동일 유형이어야 함.
ex) Flow\<Int>는 정수 값을 내보내는 flow
문서 아래 - Jetpack 라이브러리 flow와 콜백 기반 API -> flow 변환 있음.


1. 저장소 클래스 이름 규칙 : 데이터 유형 + 저장소
	ex) `NewsRepository`, `MoviesRepository`, `PaymentsRepository`
2. 데이터 소스 클래스 이름 규칙 : 데이터 유형 + 소스 유형 + DataSource
	ex) 데이터 유형의 경우 구현이 변경될 수 있으므로 좀 더 일반적인 _Remote_ 또는 _Local_을 사용
	예를 들면 `NewsRemoteDataSource` 또는 `NewsLocalDataSource`
	 소스가 중요한 경우를 좀 더 구체적으로 지정하려면 소스 유형을 사용. 
	 예를 들면 `NewsNetworkDataSource` 또는 `NewsDiskDataSource`

ps. 구현 세부정보에 따라 데이터 소스의 이름을 지정 X
	해당 데이터 소스를 사용하는 저장소가 데이터 저장 방법을 알 수 없음.
	이 규칙을 따르면 데이터 소스의 구현을 변경(SharedPre -> DataStore)해도 
	해당소스를 호출하는 레이어에 영향을 주지 않을 수 있음.
	ex) `UserSharedPreferencesDataSource` (x)


정리 : Data Layer는 Repository와 Data Sources로 구성.
	이 레이어에서 노출된 데이터는 변경 불가능 해야 함.
	종속 항목 삽입([의존성 주입 -  Dagger, Hilt](Android_Studio/1.%20android/lib/DI%20의존성%20주입/readme.md)) 권장 사항에 따라 저장소는 데이터 소스를 생성자의 종속 항목으로 사용함.



# Domain Layer [참고](https://developer.android.com/jetpack/guide/domain-layer?hl=ko)
복잡한 비즈니스 로직, 여러 ViewModel에서 재사용되는 간단한 비즈니스 로직의 캡슐화를 담당.
선택사항이므로 복잡성 처리 or 재사용성을 선호하는 등 필요한 경우에만 사용.

ex) 여러 ViewModel에서 시간대를 사용하여 화면에 적절한 메시지를 표현하려는 경우
앱에는 `GetTimeZoneUseCase` 클래스가 있을 수 있음.

이 가이드 이름 지정 규칙 : 동사 + 명사/대상(선택사항) + UseCase
ex) `FormatDateUseCase`, `LogOutUserUseCase`, `GetLatestNewsWithAuthorsUseCase`, `MakeLoginRequestUseCase`



# 구성요소 간 종속 항목 관리
앱의 클래스는 올바른 작동을 위해 다른 클래스에 종속.
특정 클래스의 종속 항목을 수집하는데 다음 디자인 패턴 중 하나를 사용 가능.
1. 종속 항목 주입(DI) : 사용 시, 클래스가 자신의 종속 항목을 구성할 필요 없이 종속 항목을 정의할 수 있음. 런타임 시 다른 클래스가 이 종속 항목을 제공해야 함.
2. 서비스 로케이터 : 서비스 로케이터 패턴은 클래스가 자신의 종속 항목을 구성하는 대신 종속 항목을 가져올 수 있는 레지스트리를 제공.

이 패턴은 코드를 중복하거나 복잡성을 추가하지 않아도 종속 항목을 관리하기 위한 명확한 패턴을 제공하므로 코드를 확장할 수 있음. 또한, 이러한 패턴을 사용하면 테스트와 프로덕션 구현 간에 신속하게 전환 가능.

**종속 항목 삽입 패턴을 따르고 Android 앱에서 [Hilt 라이브러리](https://developer.android.com/training/dependency-injection/hilt-android?hl=ko)를 사용하는 것이 좋음.**
Hilt는 종속 항목 트리를 따라 이동하여 객체를 자동으로 구성하고 종속 항목의 컴파일 시간 보장, Android 프레임워크 클래스의 종속 항목 컨테이너를 만듦.


# 앱 아키텍쳐 일반 권장사항
필수는 아니지만 대부분의 경우, 장기적으로 더 강력하고 테스트 및 유지관리가 쉬운 코드베이스를 만드는데 도움이 됨.

1. 앱 구성요소에 데이터를 저장.
2. Android 클래스의 종속 항목 줄이기.
3. 앱의 다양한 모듈 간 책임이 잘 정의된 경계 만들기.
4. 각 모듈에서 가능하면 적게 노출.
5. 다른 앱과 차별되도록 앱의 고유한 핵심에 초점.
6. 앱의 각 부분을 독립적으로 테스트하는 방법 고려.
7. 유형은 동시 실행 정책 담당.
8. 가능한 한 관련성이 높은 최신 데이터 보존.


# 아키텍쳐의 이점
- 앱의 전반적인 유지관리성, 품질, 견고성 개선.
- 앱을 확장 가능, 코드 충돌 최소화, 더 많은 인력과 팀이 코드베이스에 기여 가능.
- 온보딩에 도움, 새로운 팀원이 빠르게 업무 시작 및 짧은 시간에 효율 높임.
- 테스트 쉬움.
- 잘 정의된 프로세스를 사용하여 버그를 체계적으로 조사.

But, 아키텍쳐는 초기에 투자하는 시간이 필요.

