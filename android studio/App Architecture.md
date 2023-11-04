UI Layer() -> Data Layer (events)
	event -> 사용자가 화면에 새로운 결과 스크롤, 페이지 고침, 새 게시물 저장, 시스템이 앱에 인터넷 연결 끊김 알림, 헤드셋 연결 끊김 알림 등 발생.
	사용자 또는 Android 시스템이 시작함으로 UI Layer에서 제공.
Data Layer -> UI Layer (Data)
	이러한 이벤트를 수신하고 적절한 처리. 
	새로운 데이터가 앱에 저장되거나 인터넷의 추가정보가 로드될 수 있음.
	데이터 계층에 업데이트된 앱 상태 or 업데이트된 데이터가 있으면 UI 계층과 같은 데이터의 모든 관찰자가 업데이트됨.
	
UDF(Undirectional Data Flow) 단방향 데이터 흐름.
	일관성 보장, 디버깅하기 쉬운 단방향 데이터 흐름 패턴 준수.

## UI Layer
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

### state holder 
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