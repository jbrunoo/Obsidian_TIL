Android Not Responding

ANR의 발생 원인 ?
UI를 처리하는 메인 스레드에서 긴 작업이 일어날 경우
파일 IO, 네트워크 요청, 복잡한 계산, db 쿼리, 동기화, 무한 루프, 대기 상태(sleep, wait 등), 과도한 UI 작업(복잡 레이아웃, 무거운 애니메이션), 데드락(스레드 간의 잠금 문제로 메인 스레드 잠김)

발생하는 시간 기준은 ?
Input Dispatching Timeout : 사용자가 터치 이벤트나 키 이벤트를 보낸 이후 5초 이내에 응답이 없을 경우
Broadcast Timeout : BroadcastReceiver가 10초 이내에 실행을 완료하지 못한 경우
Service Timeout : 포그라운드 서비스는 20초, 백그라운드 서비스는 200초 이내에 실행 완료 못한 경우

