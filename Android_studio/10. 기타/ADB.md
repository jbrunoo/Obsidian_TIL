[공식문서](https://developer.android.com/tools/adb?hl=ko)
#### 개념
	Android Debug Bridge
	기기와 통신할 수 있도록 지원하는 다목적 명령줄 도구
	구성요소 
		클라이언트 - 명령어 전송, 개발 머신에서 실행(adb 명령어 실행으로 명령줄 터미널에서 클라 호출 가능)
		데몬 - 기기에서 명령어 실행, 각 기기에서 백그라운드 프로세스로 실행
		서버 - 클라와 데몬 간의 통신을 관리, 개발 머신에서 백그라운드 프로세스로 실행


	Android SDK 플랫폼 도구 패키지에 포함, 위치 : android_sdk/platform-tools/


- - -
adb pair, connect를 통해 무선 연결을 시도하는데 겪었던 문제.
