
java.lang.SecurityException: Calling uid ( 10680 ) does not have permission to access picker uri: content://media/picker/0/com.android.providers.media.photopicker/media/1000004083

```
var userQuestion by remember { mutableStateOf("") }
val imageList by db.imageDao().getAll().collectAsState(initial = emptyList())
var inference: List<String> by remember { mutableStateOf(emptyList()) }

Button(onClick = {
            scope.launch {
                for (image in imageList) {
                    // 이 로그만 찍혀요..
                    Log.d("Permission", "Before taking permission for ${image.imageUrl}")

                    val uri = Uri.parse(image.imageUrl)
                    // uri 사용하려고 하면 저 에러가 나요..
                    if (uri != null) {
                        contentResolver.takePersistableUriPermission(
                            uri,
                            Intent.FLAG_GRANT_READ_URI_PERMISSION or Intent.FLAG_GRANT_WRITE_URI_PERMISSION
                        )
                        inference = apiManager.sendUriAndQuestion(context, uri, userQuestion)
                    } else {
                        Log.e("Error", "Image URL is null for image: $image")
                    }
                    Log.d("Permission", "After taking permission for $image.imageUrl")
                }
            }

        }) {
            Text(text = "검색")
        }
```

사용자 갤러리와 room db에서 가져온 uri 값이 권한 허용이 필요한 일인지 궁금.
바로 picker 한 image uri를 사용하는데에는 문제가 없었는데 room에 넣고 쓰려니 발생.

결론 : 권한 허용을 주는 방법이 잘못된 건지.. 새로 다시 프로젝트를 만들어서 구현함.
해결방법이 [공식문서]([https://developer.android.com/training/data-storage/shared/photopicker?#persist-media-file-access](https://developer.android.com/training/data-storage/shared/photopicker?#persist-media-file-access)) 말고는 자료가 거의 없었음 + 옛날에는 권한 허용이 필요했으나 지금은 photopicker에서는 필요하지 않음. 가져올 때는 그래서 권한 관련 경험이 없었음. 
프로젝트 이후 탐구 예정..