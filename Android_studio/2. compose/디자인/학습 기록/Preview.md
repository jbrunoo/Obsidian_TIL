ps. 인텔리제이에서 자바 쓸 때 sout 하듯,
com은 composable 함수, prev는 preview composable 자동 완성해주는 안스 short key


@Preview 파라미터
name = "component", preview 이름 붙여줄 수 있음. 여러 preview 띄울 때 편할 듯함.
showBackground = true, 기본 배경 값 없을 때 흰색으로 만들어 줌. 초반에 많이 썼음.
backgroundColor로 바로 지정 가능하나 Long 값으로 보아 직접 hex code 입력.
locale = "ko", 국가별 언어 코드 활용하여 preview 다른 언어 지정 가능.
showSystemUi = true, 화면 상에서 표시.
device : Devices.NEXUS_10, 특정 기기 설정 가능.
widthDp, heightDp, apiLevel, fontScale도 설정 가능.
preview에서 동적할당 사용 설정된 경우 색상은 wallPaper 속성 활용.

@PreviewFontScale로 모든 종류의 다양한 글꼴 크기 확인 가능.
@PreviewScreenSizes로 다양한 화면 크기의 가로/세로 모두 확인 가능.
@PreviewLightDark 라이트, 다크 테마 확인 가능.(or preview의 uiMode 파라미터)
@PreviewDinamicColors 동적 색상 표시. (사용자가 장치에 설정한 배경 화면의 종류 따라)


PreviewParameterProvider를 상속받는 클래스를 정의하고
preview 함수의 파라미터로 @PreviewParameter 어노테이션을 붙여주면 
컴포넌트의 파라미터의 경우의 수를 볼 수 있음
[youtube 13:47](https://www.youtube.com/watch?v=nCd02GTBbIM)

annotation class 활용해서 preview여러 개 정의한 @CustomMultiPreview [youtube 10:00](https://www.youtube.com/watch?v=OmOYISSlsKw)

