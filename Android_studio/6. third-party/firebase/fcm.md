[공식문서](https://firebase.google.com/docs/cloud-messaging/android/client?hl=ko&_gl=1*1jn1sz6*_up*MQ..*_ga*NTA0NDAxNTgzLjE3MzM2NDAyNzk.*_ga_CW55HF8NVT*MTczMzY0MDI3OS4xLjAuMTczMzY0MDI3OS4wLjAuMA..)
Firebase Cloud Messaging

why use?
각 플랫폼 환경 별로 개발하여야 했음 -> 플랫폼 종속되지 않음.

직접 어플리케이션 서버에서 push를 보내주면 계속 연결이 지속되어야 함. -> 클라우드 메시징 서버를 경유.


message type. [공식문서](https://firebase.google.com/docs/cloud-messaging/concept-options?hl=ko)
알림 메시지 vs 데이터 메시지


impl.
manifest에 fcm service 추가
	onNewToken에서 토큰 생성 후 저장 및 서버 전송 관리
	onMessageReceived에서 메시지 처리

FirebaseMessaging.getInstance().token을 이용해서 명시적으로 현재 token 가져오기.
ex) 클라이언트에서 서버로 메시지 보내기, 알림 on/off 등

api 13 이상부터 알림 권한 설정 필요

- - -
onNewToken 메서드 불리는 경우
- 앱 설치
- 앱 데이터 삭제 후 재설치
- 토큰 갱신
- 앱 보안 관련 설정 변경
- 기기 설정 변경(구글 계정 추가/변경)

- - - 
[android 공식문서]([공식문서](https://developer.android.com/develop/ui/views/notifications?hl=ko&_gl=1*1e4gv8i*_up*MQ..*_ga*MTYyNDA0NTE4MC4xNzMzODUwMDA1*_ga_6HH9YJMN9M*MTczMzg1MDAwNC4xLjAuMTczMzg1MDAwNC4wLjAuNzQxNDU0MjEz))
알림 관련

