

Landscapist is built with a lot of consideration to improve the performance of image loadings in Jetpack Compose.

https://github.com/skydoves/landscapist  - landscapist - Glide, Coil, Fresco

```kotlin
GlideImage(
  imageModel = imageUrl,
  // Crop, Fit, Inside, FillHeight, FillWidth, None
  contentScale = ContentScale.Crop,
  // shows an image with a circular revealed animation.
  circularReveal = CircularReveal(duration = 250),
  // shows a placeholder ImageBitmap when loading.
  placeHolder = ImageBitmap.imageResource(R.drawable.placeholder),
  // shows an error ImageBitmap when the request failed.
  error = ImageBitmap.imageResource(R.drawable.error)
)
```

`imageModel`에는 `String`, `Uri`, `HttpUrl`, `File`, `DrawableRes`, `Drawable`, `Bitmap` 타입 사용 가능.

