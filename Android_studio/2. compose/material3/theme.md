[m3 theme codelab](https://developer.android.com/codelabs/jetpack-compose-theming?hl=ko#0)

ui/theme 파일 theme.kt에서 설정
color, typography, shapes

material3 에서는 color(lightColorScheme/darkColorScheme), Typography(bodyLarge, titleLarge, labelSmall..)

color(# -> 0xff)
`#dd0d3c` -> `Color(0xffdd0d3c)`
color.kt에 color 값 설정
기본/보조/3차 색상, 배경/표면 색상, on 색상
**기본**은 기본 색상으로, 눈에 띄는 버튼 및 활성 상태와 같은 기본 구성요소에 사용
**보조** 키 색상은 필터 칩과 같이 UI에서 눈에 덜 띄는 구성요소에 사용
**3차** 키 색상은 대비 강조를 위해 사용되며, 중성색은 앱의 배경과 표면에 사용
[mertrial builder](https://m3.material.io/theme-builder#/custom) 이용해서 primary color만 설정하고 theme.kt, color.kt를 export 가능
export 해오면 변수명이 달라지니 조심하자 MaterialTheme.colorScheme의 매개변수는 `primary: Color`인데 export하면 `md_theme_light_primary` 로 나옴

font
[상업용 무료 폰트](https://noonnu.cc/)

