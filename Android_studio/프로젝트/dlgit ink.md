
fun classify(context: Context, imageBitmap: ImageBitmap, imageProcessor: ImageProcessor): FloatArray {  
val model = MnistModel.newInstance(context)  
var tensorImage = TensorImage(DataType.UINT8)  
// val bitmap = imageBitmap.asAndroidBitmap().copy(Bitmap.Config.ARGB_8888, true) // java.lang.IllegalArgumentException: Only supports loading ARGB_8888 bitmaps.  
tensorImage.load(imageBitmap.asAndroidBitmap()) // tensorImage에 compose bitmap 없음  
tensorImage = imageProcessor.process(tensorImage)  
  
// Creates inputs for reference.  
val inputFeature0 = TensorBuffer.createFixedSize(intArrayOf(1, 28, 28, 1), DataType.FLOAT32)  
// val byteBuffer = ByteBuffer.allocateDirect()  
// val bytebuffer = convertBitmapGrayByteBuffer(resizeBitmap(imageBitmap.asAndroidBitmap()))  
Log.d("tensorImage buffer shape", tensorImage.toString())  
Log.d("shape", inputFeature0.buffer.toString())  
inputFeature0.loadBuffer(tensorImage.buffer)  
  
// Runs model inference and gets result.  
val outputs = model.process(inputFeature0)  
val outputFeature0 = outputs.outputFeature0AsTensorBuffer  
val confidence = outputFeature0.floatArray  
  
var maxPos = 0  
var maxConfidence = 0f  
// outputFeature0.floatArray.forEachIndexed { index, confidence ->  
// Log.d("outputFeature0 $index", "$confidence")  
// if(confidence > maxConfidence) {  
// maxConfidence = confidence  
// maxPos = index  
// }  
// }  
for (i in confidence.indices) {  
if(confidence[i] > maxConfidence) {  
maxConfidence = confidence[i]  
maxPos = i  
}  
}  
  
model.close()  
  
return confidence  
}  
  
private fun resizeBitmap(bitmap: Bitmap) =  
Bitmap.createScaledBitmap(bitmap, 28, 28, false)  
private fun convertBitmapGrayByteBuffer(bitmap: Bitmap): ByteBuffer {  
val byteBuffer = ByteBuffer.allocateDirect(bitmap.byteCount)  
byteBuffer.order(ByteOrder.nativeOrder())  
  
val pixels = IntArray(bitmap.width * bitmap.height)  
bitmap.getPixels(pixels, 0, bitmap.width, 0, 0, bitmap.width, bitmap.height)  
  
pixels.forEach { pixel ->  
val r = pixel shr 16 and 0xFF  
val g = pixel shr 8 and 0xFF  
val b = pixel and 0xFF  
  
val avgPixelValue = (r + g + b) / 3.0f  
val normalizedPixelValue = avgPixelValue / 255.0f  
  
byteBuffer.putFloat(normalizedPixelValue)  
}  
return byteBuffer  
}  
  
  
fun drawToBitmap(customPaths: List<CustomPath>): ImageBitmap {  
val drawScope = CanvasDrawScope()  
val size = Size(400f, 400f) // simple example of 400px by 400px image  
val bitmap = ImageBitmap(size.width.toInt(), size.height.toInt())  
val canvas = Canvas(bitmap)  
  
customPaths.forEach { customPath ->  
val path = Path().apply {  
moveTo(customPath.start.x, customPath.start.y)  
lineTo(customPath.end.x, customPath.end.y)  
}  
val paint = Paint().apply {  
color = customPath.color  
alpha = customPath.alpha  
}  
canvas.drawPath(  
path = path,  
paint = paint  
// color = customPath.color,  
// alpha = customPath.alpha,  
// style = Stroke(width = customPath.strokeWidth.toPx())  
)  
}  
  
return bitmap  
}


fun drawToBitmap(customPaths: List<CustomPath>): ImageBitmap {  
val drawScope = CanvasDrawScope()  
val size = Size(400f, 400f) // simple example of 400px by 400px image  
val bitmap = ImageBitmap(size.width.toInt(), size.height.toInt())  
val canvas = Canvas(bitmap)  
  
customPaths.forEach { customPath ->  
val path = Path().apply {  
moveTo(customPath.start.x, customPath.start.y)  
lineTo(customPath.end.x, customPath.end.y)  
}  
val paint = Paint().apply {  
color = customPath.color  
alpha = customPath.alpha  
}  
canvas.drawPath(  
path = path,  
paint = paint  
// color = customPath.color,  
// alpha = customPath.alpha,  
// style = Stroke(width = customPath.strokeWidth.toPx())  
)  
}  
  
return bitmap  
}

private fun resizeBitmap(bitmap: Bitmap) =  
Bitmap.createScaledBitmap(bitmap, 28, 28, false)  
private fun convertBitmapGrayByteBuffer(bitmap: Bitmap): ByteBuffer {  
val byteBuffer = ByteBuffer.allocateDirect(bitmap.byteCount)  
byteBuffer.order(ByteOrder.nativeOrder())  
  
val pixels = IntArray(bitmap.width * bitmap.height)  
bitmap.getPixels(pixels, 0, bitmap.width, 0, 0, bitmap.width, bitmap.height)  
  
pixels.forEach { pixel ->  
val r = pixel shr 16 and 0xFF  
val g = pixel shr 8 and 0xFF  
val b = pixel and 0xFF  
  
val avgPixelValue = (r + g + b) / 3.0f  
val normalizedPixelValue = avgPixelValue / 255.0f  
  
byteBuffer.putFloat(normalizedPixelValue)  
}  
return byteBuffer  
}

val imageProcessor = ImageProcessor.Builder()  
.add(NormalizeOp(0.0f, 255.0f)) // 이 줄 추가 안해서 입력값 달랐음  
.add(  
ResizeOp(  
28,  
28,  
ResizeOp.ResizeMethod.BILINEAR  
)  
) // RGB 이미지만 resizeOp 가능, grayScale 전 실행  
// .add(TransformToGrayscaleOp()) // 회색조 이미지, 라이브러리 tensorflow lite support 0.3.1 필요  
.build()

고민한 부분
1. 실시간 draw를 어떻게 할지?

2. 





                                                                                                    java.lang.OutOfMemoryError: Failed to allocate a 24 byte allocation with 1796352 free bytes and 1754KB until OOM, target footprint 201326592, growth limit 201326592; giving up on allocation because  1% of heap free after GC.



                                                                                                    java.lang.IllegalStateException: unable to getPixels(), pixel access is not supported on Config#HARDWARE bitmaps


`e: [ksp] InjectProcessingStep was unable to process 'DigitClassifier(error.NonExistentClass)' because 'error.NonExistentClass' could not be resolved.`



                                                                                                    java.lang.IllegalArgumentException: Only supports loading ARGB_8888 bitmaps.
tensorImage.load(imageBitmap.asAndroidBitmap().copy(Bitmap.Config.ARGB_8888, false))





                                                                                                    java.lang.IllegalArgumentException: Only RGB images are supported in ResizeOp, but not GRAYSCALE

https://github.com/tensorflow/tensorflow/issues/37418




class DigitClassifier @Inject constructor(private val model: MnistModel) : Classifier {  
override fun classify(imageBitmap: ImageBitmap): Int {  
  
var tensorImage = TensorImage(DataType.UINT8)  
tensorImage.load(imageBitmap.asAndroidBitmap().copy(Bitmap.Config.ARGB_8888, false))  
  
val imageProcessor = ImageProcessor.Builder()  
.add(ResizeOp(28, 28, ResizeOp.ResizeMethod.BILINEAR))  
.add(NormalizeOp(0.0f, 255.0f)) // 이 줄 추가 안해서 입력값 달랐음 (1, 150, 150, 3).add(TransformToGrayscaleOp()) // 회색조 이미지, 라이브러리 tensorflow lite support 필요  
.build()  
tensorImage = imageProcessor.process(tensorImage)  
  
val inputFeature0 = TensorBuffer.createFixedSize(intArrayOf(1, 28, 28, 1), DataType.FLOAT32)  
  
// val androidBmp = imageBitmap.asAndroidBitmap()  
// val resizedBmp = resizeBitmap(androidBmp)  
// val grayByteBuffer = convertBitmapGrayByteBuffer(resizedBmp)  
inputFeature0.loadBuffer(tensorImage.buffer)  
  
Log.d("compare buffer shape", "${tensorImage.buffer} / ${inputFeature0.buffer}")  
  
// Runs model inference and gets result.  
val outputs = model.process(inputFeature0)  
val outputFeature0 = outputs.outputFeature0AsTensorBuffer  
  
var maxPos = 0  
var maxConfidence = 0f  
  
outputFeature0.floatArray.forEachIndexed { index, confidence ->  
Log.d("confidence", "$confidence")  
if (confidence > maxConfidence) {  
maxConfidence = confidence  
maxPos = index  
}  
}  
  
model.close()  
  
return maxPos  
}
}
   java.lang.IllegalStateException: Internal error: The Interpreter has already been closed.

기존 코드는 모델을 생성하고 종료하는게 한 사이클이였는데 context 때문에 hilt 의존성 처리하다가 
싱글톤으로 만들어줬는데 함수 호출 시 close 해버려서 그런듯. 
모델 생성을 viewmodel을 따르게 하거나 .. 앱 종료되면 자동으로 종료되거나 .. 


0.100208916
2024-05-21 20:11:23.724  8277-8277  confidence              com.jbrunoo.digitink                 D  0.17835356
2024-05-21 20:11:23.724  8277-8277  confidence              com.jbrunoo.digitink                 D  0.09291862
2024-05-21 20:11:23.724  8277-8277  confidence              com.jbrunoo.digitink                 D  0.10601308
2024-05-21 20:11:23.724  8277-8277  confidence              com.jbrunoo.digitink                 D  0.076521814
2024-05-21 20:11:23.724  8277-8277  confidence              com.jbrunoo.digitink                 D  0.11489505
2024-05-21 20:11:23.724  8277-8277  confidence              com.jbrunoo.digitink                 D  0.070990644
2024-05-21 20:11:23.724  8277-8277  confidence              com.jbrunoo.digitink                 D  0.09512702
2024-05-21 20:11:23.725  8277-8277  confidence              com.jbrunoo.digitink                 D  0.078625016
2024-05-21 20:11:23.725  8277-8277  confidence              com.jbrunoo.digitink                 D  0.08634638
2024-05-21 20:11:23.725  8277-8277  result                  com.jbrunoo.digitink                 D  1

계속 같은 confidence와 result가 찍힘. 무엇이 이유인지 모르겠음.
이미지 처리부분이 문제인줄 알고 삽질 많이 했는데 혹시나 해서 데이터셋과 똑같이
black 배경에 white로 그리니까 제대로 인식됨.. 흠


