[Design for Android](https://developer.android.com/design/ui/mobile/guides/foundations/system-bars?hl=ko)
[특정 text 색깔 바꾸기 - AnnotatedString](https://blog.msg-team.com/how-do-i-color-the-text-for-each-word-differently-in-jetpack-compose-b679cbd0f792)
[색 구성표 및 색상 역할](https://m3.material.io/styles/color/the-color-system/key-colors-tones)
Scaffold 이용.

#### [codelabs](https://developer.android.com/codelabs/jetpack-compose-theming?hl=ko#0)
	M3를 사용한 Compose에서 앱의 테마 설정하기
	M3의 색 구성표, 서체, 도형의 주요 구성요소
	맞춤설정, 접근성 높은 방식으로 애플리케이션 테마 설정에 도움
	+ 다양한 수준의 강조와 함께 동적 테마 설정에 대한 지원 

	학습 내용
		M3 테마 설정의 주요 측면
		M3 색 구성표, 앱의 테마 생성 방법
		앱에 동적 테마 및 밝은/어두운 테마 지원 방법
		서체 및 도형으로 앱 맞춤설정
		M3 구성요소 및 맞춤설정을 통한 앱 스타일 지정

	색 구성표 기반 
	M3 구성요소에 사용되는 13가지 색조의 색조팔레트와 연관된 5가지 주요 색상 세트
	Accent colors(Primary, Secondary, Tertiary), Neutral colors(neutral, neutral variant)

	accent color = 강조색상(기본, 보조, 3차)이 페어링, 강조 정의, 시각적 표현을 위해 다양한 색조의 4가지 호환되는 색상으로 제공
	
	![[Pasted image 20240218142715.png]]


	Neutral color = 중성색은 표면과 배경에 사용되는 4가지 호환 색조로 나뉨
	표면에 배치되는 텍스트 아이콘을 강조하는데도 중요
	
	![[Pasted image 20240218142836.png]]


	색 구성표 생성
	맞춤 Color


color.kt, theme.kt를 custom 또는 색상 빌드 도구를 활용하여 바꿀 수 있음
default 색상이 앱 테스트 시 바뀌지 않는 [원인](https://stackoverflow.com/questions/78526697/how-to-change-the-default-color-of-button-in-jetpack-compose)은 Android 12 이상 테스트할 경우 지정된 색 구성표를 따르지 않음.
또한 동적 색상 구성표를 사용하지 않으면 해결됨. 

```kotlin
// 1. 이 부분을 false로 설정하거나
// Dynamic color is available on Android 12+  
dynamicColor: Boolean = true,

// 2. dynamicColor를 사용하지 않으면 됨.
val colorScheme = when {
        dynamicColor && Build.VERSION.SDK_INT >= Build.VERSION_CODES.S -> {
            val context = LocalContext.current
            if (darkTheme) dynamicDarkColorScheme(context) else dynamicLightColorScheme(context)
        }

        darkTheme -> DarkColorScheme
        else -> LightColorScheme
    }

// 제거
val colorScheme = when {
        darkTheme -> DarkColorScheme
        else -> LightColorScheme
    }
```

[시맨틱 네이밍의 중요성](https://jin-na.tistory.com/entry/%EB%94%94%EC%9E%90%EC%9D%B8-%EC%8B%9C%EC%8A%A4%ED%85%9C-%EC%BB%AC%EB%9F%AC-%EB%84%A4%EC%9D%B4%EB%B0%8D-%EC%A0%95%ED%95%98%EA%B8%B0)

