[[컴파일 타임 vs 런타임]]에서 가볍게 정리했으나 심층적으로 다시 살펴보았음.

이번엔 android와 compose에서의 작동 방식에 초점을 맞추어 이해


#### basic
	빌드 과정은 실행 파일을 만들기 까지 과정
	컴파일 과정은 빌드 단계에 포함되며 소스코드를 기계어로 변환
	(정확히는 java, kotlin은 기계어나 어셈블리어 변환 과정이 아닌 가상 머신에서 돌아가는 바이트 코드로 변환)
	응용프로그램에서 실행 파일이 동작하고 종료되기까지 과정이 런타임이다.


#### 컴파일
	kotlin 컴파일러가 .kt -> .class로 변환(jvm용 바이트코드)


#### 빌드
	Gradle 빌드 도구 사용
	컴파일 과정에서 변환된 .class 코드를 d8(or dx)도구를 사용해 android용 바이트 코드인 .dex 파일로 변환
	.dex(Dalvik EXcutable)파일은 안드로이드의 가상 머신인 ART(Android Runtime) 또는 Dalvik(4.4 킷캣 이전 버전)에서 사용
	AAPT2(Android Asset Packaging Tool)을 사용하여 리소스를 처리하고 R.java 파일 생성
	.dex, 리소스, manifest, 라이브러리 등을 패키징하여 APK, AAB(플레이스토어 권장) 실행 파일 생성
	실행 파일은 app/build/outputs에 있음. 단순 IDE에서 실행할 때는 임시 위치에서 저장되는 듯함.

#### 런타임
	ART에서 실행 파일의 바이트 코드를 기계어로 읽기 위해 AOT(Ahead Of Time)와 JIT(Just in Time) 컴파일러를 사용
	AOT는 프로그램 실행 전 설치 시 or 최초 실행 시 기계어로 미리 컴파일, JIT는 실행 도중 필요한 부분만


#### Compose 컴파일, 런타임 과정 작동 방식
	Compose 컴파일러는 kotlin 컴파일러 플러그인이다.
	그래서 kapt, ksp 어노테이션 프로세서와 달리 'buildFeatures {  compose = true  }'로 @Composable 사용 가능 및 FIR(Frontend Intermediate Representation)로 코드 정적 분석 및 변형
	Compose 컴파일러에서 주요한 역할은 상태를 관찰하는 모든 Composable 함수의 정보를 Compose의 런타임에 전달. 함수의 매개변수에 stable, unstable 타입 부여
	
	Compose 런타임은 Compose 모델 및 상태 관리의 초석 역할로 Slot Table을 사용하여 상태 저장 등 메모리 운용, Composable 함수에 대한 실질적인 생명주기 관리 및 UI에 대한 정보를 담고 있는 메모리 표현
	compose ui에서 ui tree를 만들어 렌더링함(런타임에 의해 소비되는 것이 기본 원칙)
	런타임에서 state에 따른 recomposition 수행 동작 원리는 equals 함수의 boolean 값에 따라 데이터 변경 인지


#### Compose에서 Stability 관리
	Compose 컴파일러는 stable할 경우 Smart Recomposition을 수행한다.
	
	Stable로 간주되는 유형
		String 포함 원시 타입
		(Int) -> String과 같은 람다 표현식
		(data) class의 프로퍼티가 모두 불변(val)인 경우
		(data) class에 @StableMaker(@Stable, @Immutable 어노테이션)로 명시적 선언
			@Immutable은 필드 초기화 이후 절대 변경되지 않음 보장 의미
			@Stable은 좀 더 느슨한 의미, 멱등성 + 잠재적 변경 가능성에 예측 가능한 동작 보장 의미
			
			ps. 또는 컬렉션 자체를 불변으로 list -> ImmutableList
			ps. @NonRestartableComposable 사용 시, recomposition 생략
	
	Compose 컴파일러는 컴파일 타임에 Composable 함수를 Restartable, Skippable, Movable과 같은 특성 추론하여 런타임에 정보 전달
	대부분 Composable 함수는 Restartable하고 멱등성 가짐(멱등성: 동일한 연산 수행 시 결과 달라지지 않음)
	stable 매개변수로만 구성된 경우 Skippable
	




ps. 추가 용어
[AAPT2](https://developer.android.com/tools/aapt2?hl=ko)
.jar(Java ARchive): Java로 작성된 코드, manifest 등 포함하는 압축 파일 형식, 주로 안드 라이브러리 개발 시 사용
	(AAR(Android ARchive)는 안드로이드 라이브러리를 더 효과적으로 관리, 리소스 등 포함 )
JNI(Java Native Interface): java와 네이티브 언어(c, c++) 간의 상호 운용성을 위한 인터페이스
	(자바 에플리케이션에서 네이티브 코드를 호출하거나 네이티브 에플리케이션에서 자바 코드 호출 가능)
