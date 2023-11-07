[Ktor 공식문서](https://ktor.io/docs/intellij-idea.html)

Ktor는 JetBrains에서 만든 server framework입니다. 사실 서버용이라기보다는 각 application 간 connection을 쉽게 만들어주기 위한 framework으로 web applications, HTTP service, mobile application, browser application 등을 모두를 지원합니다.
End-to-End 간 multiplatform application framework을 지원하는 게 최종 목표이며, Kotlin을 기반으로 동작하며 비동기 서비스를 지원하기 위해 내부적으로 coroutine을 사용합니다

1. 먼저 구글 공식 문서에서는 Retrofit과 Ktor를 다음과 같이 설명하고 있다.

- **Retrofit**: Square의 **JVM용 유형 안전 HTTP 클라이언트**로, OkHttp를 기반으로 빌드됩니다. Retrofit을 사용하면 클라이언트 인터페이스를 선언적으로 만들 수 있으며 여러 직렬화 라이브러리를 지원합니다.
- **Ktor**: JetBrains의 HTTP 클라이언트로, **Kotlin 전용으로 빌드**되었으며 코루틴을 기반으로 합니다. Ktor는 다양한 엔진, 직렬 변환기, 플랫폼을 지원합니다.

### 특징
- 라우팅  
- 요청, 응답 처리  
- 템플릿(Templating)  
- 컨텐츠 협상(negotiation) 및 직렬화  
- 인증 및 권한 부여  
- ions  
- HTTP  
- 소켓  
- 모니터링  
- 관리


