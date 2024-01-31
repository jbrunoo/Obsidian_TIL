
[공식 유튜브](https://www.youtube.com/watch?v=B7D3G6tC9n0)
[공식 블로그 문서](https://android-developers.googleblog.com/2021/10/compose-for-wear-os-now-in-developer.html)

화면 구성
Overlay - 모바일과 동일
Notification - 모바일과 동일
+widget
Compliation - 워치 페이스에서 바로 눈으로 정보를 확인 가능, 화면 누르면 해당 앱 환경으로 이동, 독립적인 액션도 수행 가능
Tile - 정보와 액션에 빠르게 엑세스 가능, 워치 페이스에서 좌우로 화면 쓸면 됨.


기본 종속성 세트 차이
모바일 : Material, Foundation, UI, Runtime, Compiler, Navigation, Animation 
wear os : 대부분 동일, Material Wear, Wear Foundation, Wear Navigation 


Wear OS Material
- Buttons
- Cards
	Default는 text 기반
	AppCard, TitleCard
- Chips, Toggle Chips
	빠른 원탭 액션 용이라 모바일보다 wear os나 작은 기기에 적합.
- Curved Text & Time Text
	timeText는 화면 위에 시간 표시할 것 권장. (wear scaffold에서 대신 처리해줌)
- Lists
	ScalingLazyColumn으로 확장 + 투명성.
- Box
	SwipeToDismissBox 화면 쓸어서 닫음. wear os 의 메인 제스쳐
- Scaffold
	timeText
	vignette(비네트, 화면 주변에 적용) - vignettePosition을 설정하면 됨.
	positionIndicator(스크롤 표시, 아래의 컴포저블에서 어느 위치에 있는지) - 화면의 곡률 때문에 필요 
	content

	Scaffold Design
	APP - MaterialTheme - Scaffold - Content
	모바일과 같음. theme 아래, content 위.
	
	Scaffold와 ScalingLazyColumn을 함께 쓰면 list의 상태를 hoisting 하여 positionIndicator를 지원.
	스크롤 표시가 화면 옆쪽에 있게 되면 content로 인해 잘리면 안되고 스크롤 상태([[state]]) 확인하여 스크롤 할 때는 timeText를 없애서 화면 공간을 더 확보할 수도 있음.

- Navigation
	APP - MaterialTheme - Scaffold - SwipeDismissableNavHost - Content



실 기기 연결 [디버깅](https://developer.android.com/training/wearables/apps/debugging?hl=ko)
android studio에서 Wear OS를 실행해보기 위해 여러가지 방법이 있다.
1. 에뮬레이터는 현재 노트북이 bios에서 가상화 사용을 못찾아서 에뮬레이터 실행이 안됨..
	실제 기기가 있어서 다행
2. USB 포트로 유선 연결.(갤럭시 워치 4에 유선 단자는 없다.)  
3. wifi 이용. 
4. 블루투스 이용.

wifi와 블루투스 이용을 위해 adb 명령어를 사용하는데 공식문서에는 사용방법만 나와 있음.
일단 adb란 Android Debug Bridge(ADB)로 기기와 통신할 수 있는 다목적 명령줄 도구로 android SDK Platform 도구 패키지에 포함되어 있다고 함. [adb 문서](https://developer.android.com/studio/command-line/adb?hl=ko) 참고.

Tools -> SDK Manager를 클릭하여 android sdk platform-tools 업데이트 했음.
체크는 이미 설치, -는 업데이트 가능 표시.

"C:\Users\사용자\AppData\Local\Android\Sdk\platform-tools" 경로는 다음과 같아서 시스템변수 path에 등록해주었음

```
PS C:\Users\6pizz\AndroidStudioProjects\MyApplication> cd C:\Users\6pizz\AppData\Local\Android\Sdk\platform-tools

PS C:\Users\6pizz\AppData\Local\Android\Sdk\platform-tools> adb devices
List of devices attached

PS C:\Users\6pizz\AppData\Local\Android\Sdk\platform-tools> adb connect 192.168.0.5:5555      failed to authenticate to 192.168.0.5:5555

PS C:\Users\6pizz\AppData\Local\Android\Sdk\platform-tools> adb connect 192.168.0.5:5555
already connected to 192.168.0.5:5555

PS C:\Users\6pizz\AppData\Local\Android\Sdk\platform-tools> adb connect 192.168.0.5:5555
cannot connect to 192.168.0.5:5555: 연결된 구성원으로부터 응답이 없어 연결하지 못했거나, 호스트로부터 응답이 없어 연결이 끊어졌습니다. (10060)

PS C:\Users\6pizz\AppData\Local\Android\Sdk\platform-tools> adb connect 192.168.0.5:5555
already connected to 192.168.0.5:5555

PS C:\Users\6pizz\AppData\Local\Android\Sdk\platform-tools> adb devices
List of devices attached
192.168.0.5:5555        device

```


갤럭시 워치 업데이트 이후 개발자 도구에 무선 디버깅 + 새 기기 등록 이용(페어링 코드 및 ip, port) 
```
PS C:\Users\6pizz\AppData\Local\Android\Sdk\platform-tools> adb pair 192.168.0.5:35553   
Enter pairing code: 779628
Successfully paired to 192.168.0.5:35553 [guid=adb-RFAT41W36HM-lV6BiQ]
PS C:\Users\6pizz\AppData\Local\Android\Sdk\platform-tools> adb connect 192.168.0.5:40535
connected to 192.168.0.5:40535
```
