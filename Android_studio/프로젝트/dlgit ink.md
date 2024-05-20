
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

3. 