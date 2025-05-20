[Compose 레이아웃 테스트](https://developer.android.com/jetpack/compose/testing?hl=ko#setup)

[codelab](https://developer.android.com/codelabs/jetpack-compose-testing?hl=ko#0)

격리 테스트, 디버깅 테스트, 시맨틱 트리, 동기화

자동테스트, 로컬 테스트(Test), 계측 테스트(android test) Compose Test Rule



테스트 코드 작성 규칙
테스트 파일 이름은 테스트 대상 클래스 + Test


- - -
로컬 - JVM 위 테스트  
계측 - Framework 의존 기능 테스트 (실제 기기 or 에뮬에서 수행)
통합 - 모듈 간 상호작용 테스트
end to end - 앱 전체 UI 테스트
