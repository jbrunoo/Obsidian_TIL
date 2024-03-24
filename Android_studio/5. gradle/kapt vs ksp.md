[developer 문서](https://developer.android.com/build/migrate-to-ksp?hl=ko)

###### Kapt(Kotlin Annotation Processing Tool) [kotlinlang - kapt](https://kotlinlang.org/docs/kapt.html)
	Java annotation Processor를 사용하여 Annotation들을 사용할 수 있도록 도와줌
	이 과정에서 stub을 생성하는데 빌드 속도에 영향을 미치는 요인
	ps. 정처기에서 test기법 모듈 stub, driver할 때 그 stub과 동명..
	더 이상 신규 기능을 추가할 생각이 없다고 적혀있음. 
	근데 developer hilt 문서에는 아직 kapt 사용함

###### KSP(Kotlin Symbol Processing) [kotlinlang - ksp](https://kotlinlang.org/docs/ksp-overview.html) [ksp release version](https://github.com/google/ksp/releases)
	Kapt와 같은 역할
	Kotlin 코드르 직접 분석하기 때문에 빌드 속도가 훨씬 빠름


함께 사용 가능하지만 Kapt의 우선순위가 높은지 함께 쓰면 성능 향상 없음
특히 xml databinding 쓸 때 KSP 지원 계획이 없다고 함 -> compose 쓰면 유의미


라이브러리가 지원하는지 알고 쓰자 [지원목록](https://kotlinlang.org/docs/ksp-overview.html#supported-libraries)
대부분 지원함. room, moshi, rxhttp, koin, glide, dagger, hilt, etc..


