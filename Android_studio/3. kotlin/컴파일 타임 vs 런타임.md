정적 타입 언어(c, c++, java, kotlin)은 객체 타입을 컴파일 타임에 결정. 런타임에 속도 빠름.
동적 타입 언어(javaScript, python)은 타입을 런타임에 결정. 


kotlin의 컴파일 과정은 jvm 위에서 돌아가고 java와 유사함.
	ps. 안드로이드는 jvm이 아닌 DVM(Dalvik)과 ART(Android RunTime)을 사용.
	ps. DVM은 JIT(Just In Time), ART는 AOT(Ahead Of Time) 방식 사용.
	ps. JIT는 실행 도중 컴파일, AOT는 앱 설치 시 컴파일되어 실행 중 컴파일x 빠른 성능(GC, 디버그 등 개선)
.kt 파일은 kotlin compiler로 바이트 코드로 변환되고 kotlin runtime lib에 의존.
	ps. 컴파일은 소스코드를 기계어로 바꿔주는 역할이지만 java, kotlin은 바이트 코드로 jvm이 이해하는 언어.
	ps. 안드로이드는 바이트 코드를 .dex 바이트 코드로 변환하여 사용.
kotlin runtime lib에는 코틀린 표준 라이브러리, java api 확장 내용이 포함.
gradle, maven같은 빌드 도구가 application 패키징할 때 알아서 kotlin runtime lib을 포함.


각각 대표적인 오류
컴파일 : syntax error, type 체크 에러, 파일 참조 오류 
런타임 : 0 나누기 오류, null 참조 오류, 메모리 부족 오류