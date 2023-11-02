[skydoves github](https://github.com/skydoves/android-developer-roadmap)
[안드로이드 codelab](https://developer.android.com/courses)

![[Pasted image 20231030141001.png]]

### APK, AAB(스마트 폰 앱의 확장자)
21.08 구글은 apk -> aab 형식으로 스토어 정책을 바꿈.
APK(Android Package)는 이미 완성된 안드로이드 앱 파일
AAB(Android App Bundle)는 APK를 완성해주는 요소를 담은 패키지

WHY?
하나의 앱을 설치할 때, 한국어 기반 S21과 독일어 기반 S10은 같은 APK를 설치하는 것이 현재 방식.
AAB로 배포되면 사용자 기기에 필요한 파일로만 구성된 APK 파일 설치 가능.(ex) 한국어 기기에는 독일어 파일 필요없음. 최신 기능은 S10에서 필요 없음 )
즉, 개발자가 스토어에 AAB 패키지를 올리면 스토어가 사용자 기기에 필요한 내용을 확인하여 그에 맞춘 APK 파일을 만들어 배포

WHAT?
물리적으로 개발자가 각 사용자의 환경에 맞는 APK 즉, 모든 버전을 최신화하기 불가능. 
AAB 활용시, 앱의 크기가 줄어듬(평균 15%)

단점?
보안에 대한 우려. 모든 안드로이드 앱에는 개발자 서명이 들어감.
누군가가 앱을 변형해 배포하려면 개발자의 서명이 없기 때문에 공식 앱이 아님을 알 수 있음.
AAB는 완성된 앱이 아닌 완성품을 만들 수 있는 레고 블록이며, 그것을 조합하는 것은 구글 플레이. 즉, 구글 플레이에서 엉성한 조합이나 없던 코드를 집어넣게 되면 자신의 컨트롤 범위를 벗어남(구글은 대리서명에 최고의 보안을 약속했지만)

구글의 강제 이유?
사용자는 앱 용량을 줄이고 개발자는 여러 개의 빌드를 준비할 필요 없다가 공식적인 입장. 사업적인 측면에서도 구글은 이득.
안드로이드 앱은 여러 스토어가 있는데 AAB 형식을 강제하면 구글 스토어를 써야 되니까.

### APP Component(in app manifest)
- activity
- fragment
- services
- content providers
- broadcast receivers

### lifecycle
### UI - compose, material3
### DI - Dagger, Hilt
### coroutine
