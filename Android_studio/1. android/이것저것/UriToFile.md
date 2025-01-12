이미지를 서버로 보내기 위해 Multipart-formdata 형식을 사용하는데, Uri 형태를 파일로 변경해주어야 한다.

기존에는 Uri에서 파일의 절대경로를 가져올 수 있었으나 Android 10부터 저장소 접근 정책의 변경에 따라,

`불러온 이미지 Uri -> 임시 파일 복사본 생성 -> 해당 복사본 서버로 업로드`을 구현해야 한다.

ps. contentResolver 키워드

- - -
구현 사항
기존 화면 단에서 imageUri(String) 값을 service까지 내려주었는데 multiPart로 보내야된다고 백엔드에서 전해듣게 되면서 contentresolver나 File 처리를 위해 context를 써야했는데, 수정 사항을 줄이면서 uriToFile로 변환될 곳이 data layer 밖에 없다는 판단 하에 처리를 해주면서 @ApplicationContext를 주입해주는 방향으로 처리했다.

- UriUtil
```kotlin
package com.captures2024.soongan.core.data.utils  
  
import android.content.Context  
import android.net.Uri  
import android.webkit.MimeTypeMap  
import java.io.File  
import java.io.FileOutputStream  
import java.io.InputStream  
import java.io.OutputStream  
  
object UriUtil {  
    fun uriToFile(context: Context, contentUri: Uri): File {  
        val fileName = generateFileName(context, contentUri)  
  
        val tempFile = File(context.cacheDir, fileName)  
        tempFile.createNewFile()  
  
        try {  
            val inputStream = context.contentResolver.openInputStream(contentUri)  
            val outputStream = FileOutputStream(tempFile)  
  
            inputStream?.use { input ->  
                copy(input, outputStream)  
            }  
  
            outputStream.flush()  
        } catch (e: Exception) {  
            e.printStackTrace()  
        }  
  
        return tempFile  
    }  
  
    private fun generateFileName(context: Context, uri: Uri): String {  
        val fileType: String? = context.contentResolver.getType(uri)  
        val ext = MimeTypeMap.getSingleton().getExtensionFromMimeType(fileType) ?: ""  
        return "soongan_image.$ext"  
    }  
  
    private fun copy(source: InputStream, target: OutputStream) {  
        val buf = ByteArray(8192)  
        var length: Int  
        while (source.read(buf).also { length = it } > 0) {  
            target.write(buf, 0, length)  
        }  
    }  
}
```

- MultiPartExt
```kotlin
package com.captures2024.soongan.core.data.utils  
  
import android.content.Context  
import android.net.Uri  
import okhttp3.MediaType.Companion.toMediaType  
import okhttp3.MultipartBody  
import okhttp3.RequestBody  
import okhttp3.RequestBody.Companion.asRequestBody  
import okhttp3.RequestBody.Companion.toRequestBody  
  
fun String?.toTextRequestBody(): RequestBody? = this?.toRequestBody("text/plain".toMediaType())  
  
fun String?.toImageMultiPart(context: Context, name: String): MultipartBody.Part? {  
    return this?.let {  
        val file = UriUtil.uriToFile(context = context, contentUri = Uri.parse(this))  
  
        MultipartBody.Part.createFormData(  
            name,  
            file.name,  
            file.asRequestBody("image/*".toMediaType())  
        )  
    }  
}
```

- DataSourceImpl
```kotlin
override suspend fun patchProfile(  
    nickname: String?,  
    selfIntroduction: String?,  
    profileImage: String?,  
): UserInfoDto? = safeAPICall {  
    service.patchProfile(  
        nickname = nickname.toTextRequestBody(),  
        selfIntroduction = selfIntroduction.toTextRequestBody(),  
        profileImage = profileImage.toImageMultiPart(context, "profileImage"),  
    )  
}.body?.responseData?.toUserInfoDto()
```


다만 고민했던 부분은 UriToBitmap으로 바꾸는 소스가 notification, fcm을 App 모듈에 구현하면서 최상단에 위치 중인데 이를 UriUtil로 옮겨와야 한다고 판단을 했지만, UriUtil을 common으로 처리하자니 data layer에서 현재 common에 대한 의존성이 없었고 data layer에 구현 시 okhttp 의존성만 추가하면 됐었다.

이후 resizing 처리 등이 들어가게 되면 UriToBitmap을 사용해서 처리해야하기 때문에 refactoring을 진행하게 되면 다시 한 번 고민해볼 예정이다.
