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