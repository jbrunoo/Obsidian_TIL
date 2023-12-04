모바일 개발자를 위한 머신러닝 라이브러리

Vission / NL 관련 오프라인에서 사용가능한 기능 제공

firebase → google 통합

[번역 ML Kit](https://developers.google.com/ml-kit/language/translation/android?hl=ko)
```
@Composable fun TranslationScreen() { 
var textEn by remember { mutableStateOf("") } 

val enKoTranslator = remember { 

val options = TranslatorOptions.Builder() 
	.setSourceLanguage(TranslateLanguage.ENGLISH) 
	.setTargetLanguage(TranslateLanguage.KOREAN) 
	.build() 
Translation.getClient(options)// 람다식에는 마지막에 return@remember 가 생략되있는 것 
} 

var enabled by remember { mutableStateOf(false) } // 라이브러리에서 성공하기 전 번역을 하지 말라고 해서 enabled 변수 추가해줌 // remember는 변수 생성 시 쓰는 것, 실행 한번만 하기 위한 compose 코드가 있음 -> launchedEffect(Unit) 

LaunchedEffect(Unit) { 
val conditions = DownloadConditions.Builder() 
	.requireWifi() 
	.build() 
enKoTranslator.downloadModelIfNeeded(conditions) 
	.addOnSuccessListener { enabled = true } 
	.addOnFailureListener { exception -> } } 
var textTranslated by remember { mutableStateOf("") } 

Box(modifier = Modifier.fillMaxSize()) { Column { Box( modifier = Modifier .weight(1f) .fillMaxSize() .background(Color(0xFF2D49E6)), contentAlignment = Alignment.Center ) { TextField( value = textEn, onValueChange = { textEn = it }, modifier = Modifier.padding(start = 24.dp, end = 24.dp), textStyle = TextStyle(fontSize = 32.sp, textAlign = TextAlign.Center), colors = TextFieldDefaults.textFieldColors( textColor = Color.White, containerColor = Color.Transparent ) ) } Box( modifier = Modifier .weight(1f) .fillMaxSize(), contentAlignment = Alignment.Center ) { Text( text = textTranslated, fontSize = 32.sp, modifier = Modifier.padding(start = 24.dp, end = 24.dp) ) } } Box( modifier = Modifier .align(Alignment.BottomCenter) .padding(bottom = 16.dp) ) { Button( onClick = { // textEn = textTranslated enKoTranslator.translate(textEn) .addOnSuccessListener { translatedText -> textTranslated = translatedText } }, enabled = enabled, colors = ButtonDefaults.buttonColors(Color.White), border = BorderStroke(width = 1.dp, color = Color.Gray) ) { Text( text = "번역", color = Color.Black ) } } } }
```

- 다중 언어 번역 지원 // 현재는 translator 여러개 생성하는 방법으로.. 노가다 // 추후 클래스로 빼는 방식 (?)




- [이미지에서 text 추출](https://developers.google.com/ml-kit/vision/text-recognition/v2/android?hl=ko)
- 추출한 text를 ‘텍스트’ 버튼 누르면 TextField에 삽입

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