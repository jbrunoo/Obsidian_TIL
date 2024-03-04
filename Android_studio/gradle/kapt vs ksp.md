[공식 문서](https://developer.android.com/build/migrate-to-ksp?hl=ko)

###### Kapt(Kotlin Annotation Processing Tool)
	Java annotation Processor를 사용하여 Annotation들을 사용할 수 있도록 도와줌
	이 과정에서 stub을 생성하는데 빌드 속도에 영향을 미치는 요인

###### KSP(Kotlin Symbol Processing)
	Kapt와 같은 역할
	Kotlin 코드르 직접 분석하기 때문에 빌드 속도가 훨씬 빠름


함께 사용되지만 Kapt의 우선순위가 높은지 함께 쓰면 성능 향상 없음
특히 databinding을 쓸 때 KSP 지원 계획이 없다고 함 -> compose 쓰면 유의미


라이브러리가 지원하는지 알고 쓰자 [지원목록](https://kotlinlang.org/docs/ksp-overview.html#supported-libraries)
대부분 지원함. room, moshi, rxhttp, koin, glide, dagger, hilt, etc..


