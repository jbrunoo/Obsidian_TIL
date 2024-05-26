[medium 글](https://medium.com/mj-studio/%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C-%EC%96%B4%EB%94%94%EA%B9%8C%EC%A7%80-%EC%95%84%EC%84%B8%EC%9A%94-2-1-service-foreground-service-e19cf74df390)


1. **Activity (액티비티)**
    - 앱의 사용자 인터페이스를 표시하고 관리하는 데 사용됩니다.
    - 사용자와 상호작용하면서 앱의 주된 화면이 됩니다.
    - 주로 하나의 액티비티가 하나의 화면을 나타냅니다.

2. **Service (서비스)**
    - 백그라운드에서 실행되는 컴포넌트로, UI 없이도 오래 걸릴 수 있는 작업을 수행할 수 있습니다.
    - 다른 구성 요소와 통신하거나, 장기 실행되는 작업을 처리하는 데 사용됩니다.
    - 포그라운드, 백그라운드, 바인드 3가지 유형.
    - 포그라운드는 사용자에게 보이는 작업들 ex) 음악 재생, 위치 추적 등
    - 백그라운드는 데이터 동기화, 백업 등

3. **Broadcast Receiver (브로드캐스트 수신자)**  
    - Android 시스템이나 다른 앱으로부터 방송되는 이벤트를 수신하고 처리하는 데 사용됩니다.
    - 예를 들면 배터리 상태 변화, 디바이스 부팅 완료 등의 이벤트를 감지할 수 있습니다.
    - 정적: manifest 등록된 수신자 / 동적: 컨텍스트에 등록된 수신자

4. **Content Provider (컨텐트 프로바이더)**
    - 데이터를 관리하고 다른 앱과 데이터를 공유하는 데 사용됩니다.
    - 데이터베이스나 파일 시스템과 같은 영속적인 데이터를 제공하고, 다른 앱이 이 데이터에 접근할 수 있도록 합니다.
    - ex)  연락처, 미디어 파일, 설정 데이터 등 다른 앱에서 접근할 수 있도록 제공할 때 사용.
    - 위 3개는 Intent인 반면, content resolver에 URI 전달로 데이터 제공 및 쿼리 삽입, 업뎃, 삭제 메서드 제공


이러한 app component들은 AndroidManifest.xml 파일에 선언되어 있으며, Android 시스템이 앱을 시작하고 관리할 때 상호작용합니다. 또한, 이러한 컴포넌트들은 서로를 인텐트(Intent) 등을 통해 호출하고 통신하여 전체 앱의 동작을 결정합니다. 개발자는 이러한 컴포넌트들을 조합하여 앱의 동작을 설계하고 구현합니다.

- - - 
compose에서 SAA(single activity architecture)로 구조를 만들면 액티비티가 하나,
서비스도 기본적으로 메인 스레드기 때문에 비동기 작업이 들어가면 새로운 스레드나 코루틴 등 처리되어야 함,
브로드캐스트 리시버에 일정 시간 알람 



간단한 작업 사진 찍기, 이미지 선택 등은 content provider 아니더라도 rememberLauncherForActivityResult를 이용한 ActivityResultContracts 사용 가능.
```kotlin
var selectUri by remember { // 갤러리 이미지 uri 객체
        mutableStateOf<Uri?>(null)
    }
    var takenPhoto by remember { // 기본 사진 앱 비트맵 객체
        mutableStateOf<Bitmap?>(null)
    }
val launcher = // 갤러리 이미지 런쳐
        rememberLauncherForActivityResult(
            contract = ActivityResultContracts.PickVisualMedia(),
            onResult = { uri ->
                selectUri = uri
                takenPhoto = null
            }
        )
val cameraLauncher = // 카메라 이미지 런쳐
	rememberLauncherForActivityResult(
		contract = ActivityResultContracts.TakePicturePreview(),
		onResult = { photo ->
			takenPhoto = photo
			selectUri = null
		})
```

