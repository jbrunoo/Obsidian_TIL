[드로이드 나이츠](https://www.youtube.com/watch?v=4hZNTL8kHPw)

Software Development Kit  vs  Library

의미가 서로 비슷함. 라이브러리는 소스 코드 모음이라면 sdk는 좀 더 문서, 샘플 코드, 기술 지원 등 종합 솔루션을 제공.


AAR(Android Archive)  vs  JAR(Java Archive)
aar
	Android 라이브러리
	Android 앱 모듈과 구조적으로 동일
	소스 코드, 리소스, 매니페스트 등 모두 포함

jar
	Java 클래스 파일과 텍스트, 이미지 등을 압축한 패키지
	다양한 플랫폼에서 사용할 수 있음



모듈 수준의 build gradle 파일의 plugin 부분의 com.android.application -> com.android.library 로 바꾸면 앱이 아니라 라이브러리가 됨.

즉, sdk 개발은 앱 개발과 코드 작성 부분에서 거의 차이가 없음
단지, 어떤 앱에서 실행될지 모름

라이브러리 충돌은 그 라이브러리도 또 다른 라이브러리를 참조하고 있기 때문에 충돌이 나게 됨
앱에서는 보통 버전을 고정시키거나 바꾸거나 라이브러리를 쓰지 않는 등 해결할 수 있음

sdk 에서는 ?
의존성 관리와 버전 충돌 해결 방법
	내제화 하기
		원본 소스를 (그대로 or 조금 바꾸고) 패키지 이름만 바꿔서 sdk 내부에 포함
		외부 종속성을 내부 종속성처럼 관리 가능
		용량 증가 및 가져온 소스코드 직접 유지보수 + 오픈소스 라이브러리가 아니면 힘듦
	exclude 할 수 있도록 하기
		문제가 생기는 라이브러리만 exclude 가능케 하기
		일종의 기능 탈부착(Opt-In : 원할 때 기능 추가 / Opt-Out : 원할 때 기능 제외)
		디폴트는 비즈니스에 따라서 다름. 동시 사용도 가능. (ex. 경량화 + 유저 자유 -> Opt-In)
	주(major) 버전 올리기
		유의적 버전(Semantic Versioning)에 따라 하위 호환성이 보장되지 않음을 명시
		- 해결방법이 아니라 어쩔 수 없이 버전이 맞지 않을 때 도입하는 커뮤니케이션 정책
		즉, 사용 유저가 버전에 맞게 마이그레이션 해야하고 제공자도 자료를 준비해야 함 (최대한 지양)


Public API 관리
API 레퍼런스, 개발자 가이드

하이럼(Hyrum)의 법칙 : 사용자들이 많아질 수록 API 제작 의도와 상관없이 APi로 할 수 있는 것들을 함

Public API의 공개 범위를 제한하는 방법
	언어의 접근 제어자 적절하게 사용
		public, protected, default(java), internal(kotlin), private
		유저에게만 public 아니게 하는게 쉽지 않음
		public 제외. 각 접근자들이 java/kotlin 의 범위가 조금씩 차이가 있음
		internal이 byte code로 컴파일 되면 java에서 public으로 접근 가능한 이슈도 있다고 함
	가이드에 없으면 비공식 API
		사람의 양심에 맡기는 방식.. 제휴사가 아닌 불특정 다수에게 오픈되는 sdk는 어려움
	어노테이션 활용(@VisibleForTesting, @RestrictTo)
		사용하려고 하면 안드로이드 스튜디오에서 경고를 주지만 실제 빌드에는 아무 영향 없음 (반쪽 해결법)
	난독화
		클래스/메서드 이름을 알아볼 수 없게 바꿔줌 (꽤나 효과적)
		난독화 범위 설정이 까다로움
		위에서 말한 난독화가 풀리는 kotlin internal 이슈


직접 운영하지 않는 앱에서 디버깅
	크래시 로그 수집 툴 (Sentry) 사용하여 원인 파악 디버깅
	고객사로부터 전달받은 시나리오로 버그 재현
	고객사로부터 연동 코드와 디버그 빌드 된 앱을 받아서 분석
	예측 디버깅(이메일, 전화 등)
	찾아가는 디버거 서비스

직접 운영하지 않는 앱에서 라이프 사이클 관리
	Application.ActivityLifeCycleCallbacks 등록
	LifecycleOwner 받기
	onResume, onPause 등을 호출해서 하기
	Activity를 직접 생성해서 사용

