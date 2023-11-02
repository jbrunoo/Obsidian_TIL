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



