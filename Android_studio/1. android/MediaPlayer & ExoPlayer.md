
## MediaPlayer vs ExoPlayer
MediaPlayer - 오디오 재생만 하거나 애플리케이션 용량 줄임
ExoPlayer - 그 외 모든 경우


## MediaPlayer

MediaPlayer 생성하는 방법 2가지.- 기본 생성자 사용 or 팩토리 메서드 사용
1. 기본생성자 : 기본 생성자인 MediaPlayer를 생성한 후 setDataSource() 메서드로 데이터 소스를 설정하고 setDisplay() 메서드로 서피스홀더를 넘겨준 다음 동기적인 준비 메서드인 prepare()나 비동기적인 준비 메서드 prepareAsync()를 호출하면 됩니다.
2. 팩토리 메서드 : static인 create 메서드로 MediaPlayer를 생성하고 인자로 Context와 데이터 소스, SurfaceHolder 등을 넘겨주면 됩니다. SurfaceHolder를 지정하지 않으면 오디오만 재생됨.
	but, 성공적으로 MediaPlayer가 로드되면 자동으로 동기적 메서드(prepare()) 실행됨. 
	대용량 미디어에 비효율적.

근데 이건 15년 자료라서 [MediaPlayer 공식문서](https://developer.android.com/guide/topics/media/mediaplayer?hl=ko#kotlin) 참고하면 위에 말한 팩토리 메서드 느낌으로 .create(), .start() 사용하는 것 같긴 함. [tistory 참고](https://lovestudycom.tistory.com/entry/MediaPlayer%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C%EC%97%90%EC%84%9C-%EA%B0%84%EB%8B%A8%ED%95%9C-%EB%B9%84%EB%94%94%EC%98%A4-%EC%9E%AC%EC%83%9D)

MediaPlayer에서 지원하는 미디어 소스
1. **로컬 리소스**
2. **(\*)ContentResolver에서 가져올 수 있는 것과 같은 내부 URI**
	**(\*)ContentProvider : 다른 앱에서 자신의 앱의 데이터베이스를 접근할 수 있도록 도와주는 역할**
	(\*)ContentResolver : ContentProvider를 접근할 때에는 ContentResolver를 통해서 접근하게 됩니다. ContentResolver는 기본적으로 CRUD(Create Retreive Update Delete) 함수들을 제공하여서 이를 통해 다른 어플리케이션에 있는 데이터베이스를 조작할 수 있습니다.
3. **외부 URL (스트리밍)**


원시 리소스 (res/raw)
```kotlin
	var mediaPlayer: MediaPlayer? = MediaPlayer.create(context, R.raw.sound_file_1)
	mediaPlayer?.start() // no need to call prepare(); create() does that for you
```

로컬로 사용가능한 URI에서 재생(content resolver 통해 획득)
```kotlin
    val myUri: Uri = .... // initialize Uri here    
    val mediaPlayer: MediaPlayer? = MediaPlayer().apply {           
    setAudioStreamType(AudioManager.STREAM_MUSIC)        
    setDataSource(applicationContext, myUri)        
    prepare() // prepareAsync()
    start()    
    }
```

HTTP 스트리밍을 통해 원격 URL에서 재생하는 방법
```kotlin
	val url = "http://........" // your URL here    
	val mediaPlayer: MediaPlayer? = MediaPlayer().apply {    
	setAudioStreamType(AudioManager.STREAM_MUSIC)        
	setDataSource(url)        
	prepare() // might take long! (for buffering, etc) // prepareAsync()
	start()    
	}
```
ps. URL을 전달하여 온라인 미디어 파일 스트리밍할 때, 파일을 점진적으로 다운할 수 있어야 함.


비동기 처리, 상태 관리 등 문서 참고.

wake lock(백그라운드 미디어 재생 서비스에서 시스템 방해없이 계속 실행) - 절전, 화면 어두워짐
```kotlin
mediaPlayer = MediaPlayer().apply {        // ... other initialization here ...        
	setWakeMode(applicationContext, PowerManager.PARTIAL_WAKE_LOCK)    
}
```
ps. wake lock은 항상 예외적인 경우만 사용. 반드시 필요할 때만 유지. 기기 배터리 수명 크게 줄임.

wake lock은 CPU의 절전모드가 해제된 상태만 보장
네트워크를 통해 미디어 스트리밍, WIFI 사용한다면 WifiLock도 좋음. 수동으로 가져오고 해제해야 함.
```kotlin
    val wifiManager = getSystemService(Context.WIFI_SERVICE) as WifiManager    
    val wifiLock: WifiManager.WifiLock =        
    wifiManager.createWifiLock(WifiManager.WIFI_MODE_FULL, "mylock")    
    wifiLock.acquire()
```
미디어 중지, 일시 중지, 네트워크가 더 이상 필요치 않을 때 잠금 해제해야 함.
```kotlin
wifiLock.release()
```


### 정리 실행

앞에서 언급했듯이 MediaPlayer 객체는 상당량의 시스템 리소스를 소비할 수 있으므로 필요할 때만 유지하고 작업이 끝나면 release()를 호출해야 합니다. 시스템 가비지 컬렉션을 사용하기보다는 이 정리 메서드를 명시적으로 호출하는 것이 중요합니다. 가비지 컬렉터는 메모리 요구에만 민감하고 다른 미디어 관련 리소스의 부족에는 민감하지 않아서 MediaPlayer를 회수하는 데 시간이 걸리기 때문입니다. 따라서 서비스를 사용하는 경우에는 항상 onDestroy() 메서드를 재정의하여 MediaPlayer를 해제해야 합니다.

```kotlin
    class MyService : Service() {        
	    private var mediaPlayer: MediaPlayer? = null        
	    // ...        
	    override fun onDestroy() {            
		    super.onDestroy()            
		    mediaPlayer?.release()        
	    }    
    }
```


## ExoPlayer [공식문서](https://developer.android.com/guide/topics/media/exoplayer?hl=ko)
Android 프레임워크에 속하지 않고 Android SDK에서 별도로 배포되는 오픈소스 프로젝트입니다. ExoPlayer의 표준 오디오 및 동영상 구성요소는 Android 4.1(API 레벨 16)에서 출시된 Android MediaCodec API를 기반으로 합니다. ExoPlayer는 라이브러리이므로 앱을 업데이트하여 새로운 기능을 쉽게 사용할 수 있습니다.

스트리밍 서비스가 주를 이루면서 구글에서 DASH와 SmoothStreaming을 지원하는 ExoPlayer 도입.







# 사용
로컬 리소스 - Android Resource Directory에서 type을 raw로 설정하여 폴더 만들어서 음악 파일을 넣어줘야 함(그냥 디렉토리 만들어도 되는지?)

1. mp4 파일이라 안되는 듯? 지원 확장자에는 있는데, 오디오만 출력하려고 해서 그런지..
