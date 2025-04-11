```kotlin
override suspend fun getFcm(): String = suspendCoroutine { continuation ->

        FirebaseMessaging.getInstance()
            .token
            .addOnCompleteListener { task ->

                if (!task.isSuccessful) {
                    Log.d("token onCompleteListener fail", "token onCompleteListen fail: ${task.exception}")
...
```

위 코드에서 

`token onCompleteListen fail: java.io.IOException: java.util.concurrent.ExecutionException: java.io.IOException: SERVICE_NOT_AVAILABLE`

해당 오류를 만났다. [stackOverFlow](https://stackoverflow.com/questions/50208426/android-fcm-java-io-ioexception-service-not-available-error-on-some-devices)

크게 **네트워크 오류, 날짜/시간 설정, GMS 관련 문제**로 나와있는데,
기존에는 문제가 없었고 날짜/시간을 만진 적이 없어서 단 하나 새로운 에뮬레이터를 사용했었다.

최근에 리더보드 연동한 개인 앱을 같은 에뮬레이터에서 테스트한 적이 있는데 충돌이 일어났나 했지만
스택오버플로우에서 제안한 캐시 등을 제거해보려해도 google play service 앱 자체가 에뮬레이터에 없었다.

gpt에게 받은 GMS 서비스 상태를 볼 수 있는 코드를 입력해보았다.
`adb shell dumpsys activity service com.google.android.gms/.chimera.GmsService`

실패한 에뮬레이터 `No services match: com.google.android.gms/.chimera.GmsService
Use -h for help.`

성공한 에뮬레이터 `adb: more than one device/emulator`

일단은 에뮬레이터 내부 GMSService 자체 문제로 파악하였다.
