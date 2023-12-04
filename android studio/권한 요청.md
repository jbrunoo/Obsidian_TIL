## 퍼미션 요청 API

Activity가 AppCompatActivity를 상속하면 다음과 같은 API를 사용할 수 있습니다.

- checkSelfPermission()
- requestPermissions()
- shouldShowRequestPermissionRationale()

이 API들을 사용하여 사용자에게 퍼미션을 요청할 수 있습니다.

또한, ActivityCompat으로 동일한 동작을 수행하는 API를 사용할 수 있습니다.

- ActivityCompat.checkSelfPermission()
- ContextCompat.checkSelfPermission()
- ActivityCompat.requestPermissions()
- ActivityCompat.shouldShowRequestPermissionRationale()


이제 위의 API들을 사용하여 Runtime permission을 사용자에게 요청하고 얻는 방법을 알아보겠습니다.



```kotlin
if (checkSelfPermission(Manifest.permission.ACCESS_FINE_LOCATION)
        == PackageManager.PERMISSION_GRANTED) {
    // Permission is granted
} else {
    // Permission is not granted
}
```

```kotlin
companion object {
    const val PERMISSION_REQUEST_CODE = 1001
}

if (checkSelfPermission(Manifest.permission.ACCESS_FINE_LOCATION)
        == PackageManager.PERMISSION_GRANTED) {
    showDialog("Permission granted")
} else {
    requestPermissions(arrayOf(Manifest.permission.ACCESS_FINE_LOCATION), // 1
            PERMISSION_REQUEST_CODE) // 2
}
```
1. 퍼미션을 배열로 전달하는 이유는, 이 API는 여러 퍼미션을 동시에 요청할 수 있도록 만들어졌기 때문입니다. 만약 한개의 퍼미션만 요청한다고 해도 배열로 전달해야 합니다.
2. `PERMISSION_REQUEST_CODE`는 단순한 상수인데, 권한 요청에 대한 응답으로 이벤트가 전달될 때 이 code로 전달됩니다. 숫자는 아무거나 해도 되는데, 다른 이벤트들과 구분되어야 합니다.


photoPicker는 권한 없이 갤러리에서 이미지를 가져올 수 있음
하지만 room db에 이미지 uri를 저장하고 가져오려니 퍼미션 오류가 뜸.
[블로그](https://salmonpack.tistory.com/46) 이 글 참고 중

안드로이드 storage 관한  [공식 유튜브](https://www.youtube.com/watch?v=jcO6p5TlcGs)

