	의존성 및 관심사 분리를 위해 패키지 단위에서 모듈 단위로 프로젝트를 나누려고 함
	즉, 멀티 모듈 프로젝트 과정에서 여러 모듈로 인한 build.gradle과 같은 중복 파일이 발생함
	중복된 코드들이 많아지므로 BuildSrc나 Build-logic 모듈을 두어 관리할 수 있음

[작업 내역](https://github.com/jbrunoo/BoulderLog/pull/1)

###### Base 이해 및 설정
	build.gradle.kts
		모듈 단위의 빌드 설정을 담당
		플러그인/의존성 선언, 빌드 옵션(sdk 버전), task 정의 등
	settings.gradle.kts
		프로젝트 전체의 구조와 설정 정의
		루트 프로젝트 이름 설정, 서브모듈 포함 선언, 빌드 캐시, 리포지토리 설정 등
	Gradle은 상위 디렉토리로 올라가며 settings.gradle.kts를 찾고 이를 기준으로
	프로젝트 경계를 인식함
	
	즉, 개별 모듈은 root settings.gradle.kts에서 include로 서브 모듈임을 알려주면 되고
	build-logic 모듈은 settings.gradle.kts를 추가하여 빌드시 사용되도록 선언하고
	root settings.gradle.kts에서 include가 아닌 includeBuild로 추가해주어야 함

		이외 증분 빌드와 같은 gradle 이론적인 개념은 다른 노트에서 다루는 것으로

---
#### plugin
	새로운 task, configuration, 도메인 객체 등을 추가하여 Gradle의 기본 기능을 확장
	재사용가능한 소프트웨어 조각
	ex
		java 플러그인은 컴파일, 테스트, JAR 생성 등의 작업을 자동으로 추가
		com.android.application 플러그인은 Android 앱 빌드에 필요한 모든 설정과 작업 제공

	3종류의 작성 방식 (Script/Binary/Precompiled Script Plugins) - ref 참고
	Script Plugins
		xxx.build.gradle.kts와 같이 Gradle Script 파일을 만들어서
		중복된 코드 붙여 넣으면 됨. 다만 Groovy로 작성해야하고 plugins{} blocks는 사용 X
		Gradle Kotlin DSL은 타입에 안전하기 때문에 Sctipt Plugin을 사용못해서 Groovy
		플러그인이 필요한 모듈 내 build.gradle.kts에서 apply(from = "xxx.gradle.kts")
	
	Precompiled Script Plugins
		.gradle(.gradle.kts) 확장자의 Gradle 스크립트 파일을 미리 컴파일하여
		다른 프로젝트에서 binary 라이브러리처럼 사용할 수 있도록 만듦
		Script Plugin처럼 Sctipt 파일을 두지만
		해당 플러그인을 사용할 모듈의 build.gradle.kts 내
		plugins { id("x.x.x)" }로 선언
	
	Binary plugins (JAR 배포 가능)
		build.gradle.kts에 코드 작성이 아니라 
		src/main/kotlin 프로젝트 폴더에 플러그인을 작성하고
		build-logic 모듈 내 build.gradle.kts에서 
		gradlePlugin { plugins { register("xx") {id, implClass} } }
		이후,
		플러그인을 사용할 모듈 내 build.gradle.kts에서 plugins { id("xx") } 선언
		사전에 바이트 코드로 컴파일 되고 필요한 곳에서 바로 실행

	NIV에서는 Binary로 사용. DroidKnights에서 Precompiled Script를 사용중
	Binary 방식의 ConventionPlugin class 작성이 힘들거나 귀찮다면 대안이 될 수 있는 듯


	이후는 Kotlin DSL를 활용하여 공통 로직을 플러그인으로 분리하는 작업 뿐

---
참고 레퍼런스

[youtube, DroidKnights2023-Gradle Kotlin 컨벤션 플러그인으로 효율적인 멀티 모듈 관리](https://www.youtube.com/watch?v=7iag6zpGd98)
[github, NIV build-logic](https://github.com/android/nowinandroid/tree/main/build-logic)
[github, droidknights build-logic](https://github.com/droidknights/DroidKnightsApp/tree/main/build-logic)
[velog](https://velog.io/@couch_potato/Android-%EC%BB%A8%EB%B2%A4%EC%85%98-%ED%94%8C%EB%9F%AC%EA%B7%B8%EC%9D%B8%EC%9D%84-%EC%82%AC%EC%9A%A9%ED%95%B4%EC%84%9C-%EB%A9%80%ED%8B%B0-%EB%AA%A8%EB%93%88%EC%9D%84-%ED%9A%A8%EC%9C%A8%EC%A0%81%EC%9C%BC%EB%A1%9C-%EA%B4%80%EB%A6%AC%ED%95%B4%EB%B3%B4%EC%9E%90)
