
일단 자바 코드가 많고 설명이 좀 불친절함.
여러 코드 막 붙여넣다보니 의존성 충돌나고 duplicate 문제 계속 뜸.

[참고](https://www.tensorflow.org/lite/android/development?hl=ko)하여 Android Studio ML 모델 바인딩 기능을 활용하여 자동으로 build.gradle도 추가해주고 눌러보면 kotlin/java 코드까지 적어줌. 이걸 활용해야 했어..


```kotlin
val imageProcessor = ImageProcessor.Builder()  
        .add(NormalizeOp(0.0f, 255.0f))  // 이 줄 추가 안해서 입력값 달랐음 (1, 150, 150, 3)//        .add(TransformToGrayScaleOp()) // 회색조 이미지, 라이브러리 tensorflow lite support 필요  
        .add(ResizeOp(150, 150, ResizeOp.ResizeMethod.BILINEAR))  
        .build()  
  
    var outputFeature0 by remember { mutableStateOf<FloatArray>(FloatArray(1)) }
```


```kotlin
val model = WasteClassificatiionCnnModelOptimized.newInstance(context)  
  
var tensorImage = TensorImage(DataType.UINT8)  
tensorImage.load(bitmap)  
  
tensorImage = imageProcessor.process(tensorImage)  
  
// Creates inputs for reference.  
val inputFeature0 =  
    TensorBuffer.createFixedSize(intArrayOf(1, 150, 150, 3), DataType.FLOAT32)  
inputFeature0.loadBuffer(tensorImage.buffer)  
  
// Runs model inference and gets result.  
val outputs = model.process(inputFeature0)  
outputFeature0 = outputs.outputFeature0AsTensorBuffer.floatArray  
  
// Releases model resources if no longer used.  
model.close()
```
자동으로 코드 생성해줌.  input과 output 처리만 해주면 됨. 
즉, 모델에 대한 이해도가 있어야 input과 output의 값을 처리할 수 있음.

```kotlin
// 이렇게 문자열로 찍으면 F@7dd311b 문자열 템플릿 리터럴로 반환
Text(text = outputFeature0.toString())

//
Text(text = "Predicted Class: ${outputFeature0.joinToString(", ")}")
```


- - - 

오랜만에 다시 써본 라이브러리

```kotlin
fun classify(context: Context, imageBitmap: ImageBitmap, imageProcessor: ImageProcessor): Int {
    val model = MnistModel.newInstance(context)
    var tensorImage = TensorImage(DataType.UINT8)
    tensorImage.load(imageBitmap.asAndroidBitmap()) // tensorImage에 compose bitmap 없음
    tensorImage = imageProcessor.process(tensorImage)

    // Creates inputs for reference.
    val inputFeature0 = TensorBuffer.createFixedSize(intArrayOf(1, 28, 28, 1), DataType.UINT8)
//        val byteBuffer = ByteBuffer.allocateDirect()
    Log.d("tensorImage buffer shape", tensorImage.buffer.toString())
    Log.d("shape", inputFeature0.buffer.toString())
    inputFeature0.loadBuffer(tensorImage.buffer)

    // Runs model inference and gets result.
    val outputs = model.process(inputFeature0)
    val outputFeature0 = outputs.outputFeature0AsTensorBuffer

    Log.d("outputFeature0.floatArray", "${outputFeature0.floatArray}")
    Log.d("outputFeature0.intArray", "${outputFeature0.intArray}")

    model.close()
    return outputFeature0.intArray[0]
}


fun drawToBitmap(customPaths: List<CustomPath>): ImageBitmap {
    val drawScope = CanvasDrawScope()
    val size = Size(400f, 400f) // simple example of 400px by 400px image
    val bitmap = ImageBitmap(size.width.toInt(), size.height.toInt())
    val canvas = Canvas(bitmap)

    drawScope.draw(
        density = Density(1f),
        layoutDirection = LayoutDirection.Ltr,
        canvas = canvas,
        size = size
    ) {
        customPaths.forEach { customPath ->
            val path = Path().apply {
                moveTo(customPath.start.x, customPath.start.y)
                lineTo(customPath.end.x, customPath.end.y)
            }
            drawPath(
                path = path,
                color = customPath.color,
                alpha = customPath.alpha,
                style = Stroke(width = customPath.strokeWidth.toPx())
            )
        }
    }
    return bitmap
}

```

```kotlin
 D  java.nio.DirectByteBuffer[pos=0 lim=9408 cap=9408]
 D  java.nio.DirectByteBuffer[pos=0 lim=3136 cap=3136]
 
java.lang.IllegalArgumentException: The size of byte buffer and the shape do not match.

```

로그 상 inputFeature0 byteBuffer가 