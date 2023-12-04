```kotlin
var selectUri by remember { mutableStateOf<Uri?>(null) } 

val launcher = rememberLauncherForActivityResult( contract = ActivityResultContracts.PickVisualMedia(), onResult = { uri -> selectUri = uri } ) 

selectUri?.let { val context = LocalContext.current // uri를 비트맵으로 바꿔서 쓰는 방법 검색하면 나옴 // selectUri = null인데 createSourece는 uri를 null값 받지 않기 때문에 let처리 -> let안에 it:Uri로 null값이 아닐 때 실행됨. null값이면 넘어감. 
// 이런식으로 한 코드에 버전별로 처리해줌
val bitmap = if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.P) { ImageDecoder.decodeBitmap(ImageDecoder.createSource(context.contentResolver, it)) } else { MediaStore.Images.Media.getBitmap(context.contentResolver, it) } Image(bitmap = bitmap.asImageBitmap(), contentDescription = null) }

Image(bitmap = bitmap.asImageBitmap(), contentDescription = null)

Button( onClick = { launcher.launch(PickVisualMediaRequest(ActivityResultContracts.PickVisualMedia.ImageOnly)) }, colors = ButtonDefaults.buttonColors(Color.White), modifier = Modifier.align(Alignment.Center) ) { Text( text = "CHANGE", color = Color.Black ) }
```
decodeBitmap 에서 alt enter surrond 감싸주면 버전별 관리



```kotlin
val image: InputImage 
val context = LocalContext.current // 추가 
selectedUri?.let // 부분으로 아래 예외 처리 감싸주기 
try {     
	image = InputImage.fromFilePath(context, it) 
		.recognizer.process(image) 
		.addOnSuccessListener { result -> 
		text = result.text 
		} 
		.addOnFailureListener { e -> 
		// Task failed with an exception // ... } 
} catch (e: IOException) {     
	e.printStackTrace() 
} 
Text = "imageText : ${text}"
```
// 파일 URI 사용.


물론 Image 대신 [[이미지 Glide, Coil, Fresco]] 등의 라이브러리에 asyncimage 등을 사용하면 uri로 이미지 보여줄 수 있음


