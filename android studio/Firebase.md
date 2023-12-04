[firebase DB 설계](https://www.youtube.com/watch?v=haMOUb3KVSo&list=RDCMUCP4bf6IHJJQehibu6ai__cg&index=4)



firebase analytics or google analytics
웹이나 앱에서 쓰는 동작들에 대해 통계를 제공해주는
사용자가 적을 때 무료
[https://firebase.google.com/docs/analytics/get-started?platform=android&hl=ko](https://firebase.google.com/docs/analytics/get-started?platform=android&hl=ko)

문서 읽고 firebase 붙이기

‘**파이어베이스’는 구글(Google)이 소유하고 있는 모바일 애플리케이션 개발 플랫폼**
[](https://blog.wishket.com/%ED%8C%8C%EC%9D%B4%EC%96%B4%EB%B2%A0%EC%9D%B4%EC%8A%A4firebase%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80-%ED%8C%8C%EC%9D%B4%EC%96%B4%EB%B2%A0%EC%9D%B4%EC%8A%A4-%EC%8B%AC%EC%B8%B5-%ED%83%90/)[https://blog.wishket.com/파이어베이스firebase란-무엇인가-파이어베이스-심층-탐/](https://blog.wishket.com/%ED%8C%8C%EC%9D%B4%EC%96%B4%EB%B2%A0%EC%9D%B4%EC%8A%A4firebase%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80-%ED%8C%8C%EC%9D%B4%EC%96%B4%EB%B2%A0%EC%9D%B4%EC%8A%A4-%EC%8B%AC%EC%B8%B5-%ED%83%90/)

android app에 firebase 추가 시,
구글 로그인 등 이용하려면 디버그 서명 인증서 SHA-1 필요한데,

window 에서는
```kotlin
keytool -list -v -keystore "%USERPROFILE%₩.android₩debug.keystore" -alias androiddebugkey -storepass android -keypass android
```

```kotlin
// 공식문서에는 이렇게 나와 있음
keytool -list -v \\
-alias androiddebugkey -keystore %USERPROFILE%\\.android\\debug.keystore
```

'keytool'은(는) 내부 또는 외부 명령, 실행할 수 있는 프로그램, 또는 배치 파일이 아닙니다. 이라고 뜸.
이유 : jdk 설치와 환경변수 설정 안되어 있어서.

해결:
debug 서명 인증서 SHA-1 (keytool 쓰려면 jdk, 환경변수 지정 / 하려는데 gradle 활용 ctrl + enter 로 해결)

[](https://modelmaker.tistory.com/entry/%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C-Debug-SHA-Key-%EC%B6%94%EC%B6%9C-%EB%B0%A9%EB%B2%95)[https://modelmaker.tistory.com/entry/안드로이드-Debug-SHA-Key-추출-방법](https://modelmaker.tistory.com/entry/%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C-Debug-SHA-Key-%EC%B6%94%EC%B6%9C-%EB%B0%A9%EB%B2%95)
```
./gradlew signingReport //사용할 수 없는 프로그램이라고 뜬다면

gradlew signingReport // 안스 터미널에 친 후 ctrl + enter하면 됨-> start commands execution 기능
```




cf.
음성인식 추가하기
Mainifest.xml - 오디오 사용 권한 줘야함
```kotlin
<uses-permission android:name="android.permission.RECORD_AUDIO" />
```

디지털 잉크 인식
[https://developers.google.com/ml-kit/vision/digital-ink-recognition/android?hl=ko](https://developers.google.com/ml-kit/vision/digital-ink-recognition/android?hl=ko)

