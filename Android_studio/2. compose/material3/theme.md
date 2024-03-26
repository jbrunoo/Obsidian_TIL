[m3 theme codelab](https://developer.android.com/codelabs/jetpack-compose-theming?hl=ko#0)

ui/theme 파일 theme.kt에서 설정
color, typography, shapes

material3 에서는 color(lightColorScheme/darkColorScheme), Typography(bodyLarge, titleLarge, labelSmall..)

# color(# -> 0xff)
	`#dd0d3c` -> `Color(0xffdd0d3c)`
	color.kt에 color 값 설정
	[mertrial builder](https://m3.material.io/theme-builder#/custom) 이용해서 primary color만 설정하고 theme.kt, color.kt를 export 가능
	export 해오면 변수명이 달라지니 조심하자 MaterialTheme.colorScheme의 매개변수는 `primary: Color`인데 export하면 `md_theme_light_primary` 로 나옴

```kotlin
// 컬러 값 예시
// on은 대비되는 색상 ex) 버튼 색
val primary = Color(0xFF3B6A1D)  
val onPrimary = Color(0xFFFFFFFF)  
val primaryContainer = Color(0xFFBAF294)  
val onPrimaryContainer = Color(0xFF092100)  
val secondary = Color(0xFF56624B)  
val onSecondary = Color(0xFFFFFFFF)  
val secondaryContainer = Color(0xFFD9E7CA)  
val onSecondaryContainer = Color(0xFF141E0D)  
val tertiary = Color(0xFF386666)  
val onTertiary = Color(0xFFFFFFFF)  
val tertiaryContainer = Color(0xFFBBECEB)  
val onTertiaryContainer = Color(0xFF002020)  
val error = Color(0xFFBA1A1A)  
val errorContainer = Color(0xFFFFDAD6)  
val onError = Color(0xFFFFFFFF)  
val onErrorContainer = Color(0xFF410002)  
val background = Color(0xFFFDFDF5)  
val onBackground = Color(0xFF1A1C18)  
val surface = Color(0xFFFDFDF5)  
val onSurface = Color(0xFF1A1C18)  
val surfaceVariant = Color(0xFFE0E4D6)  
val onSurfaceVariant = Color(0xFF43483E)  
val outline = Color(0xFF74796D)  
val inverseOnSurface = Color(0xFFF1F1EA)  
val inverseSurface = Color(0xFF2F312C)  
val inversePrimary = Color(0xFF9FD67B)  
val surfaceTint = Color(0xFF3B6A1D)  
val outlineVariant = Color(0xFFC4C8BB)  
val scrim = Color(0xFF000000)
```


# font
[상업용 무료 폰트](https://noonnu.cc/)

