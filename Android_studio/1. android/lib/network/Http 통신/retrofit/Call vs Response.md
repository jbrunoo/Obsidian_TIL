
Call vs Response
응답 값으로 Call과 Response를 받을 수 있는데, 
Call을 사용하여 서버에 요청을 보내면 각각의 Call은 자체적으로 HTTP 요청과 응답 쌍을 생성.
execute()나 enqueue()을 통해 실행.
execute()는 동기적인데 메인스레드가 차단되기 때문에 권장 x
enqueue()는 비동기적 방식.
enqueue()는 콜백 메서드를 통해 onResponse, onFailure로 핸들링 가능.

Response는 요청 이후 응답 결과만 반환.
Response는 try ~ catch 등을 이용하여 response.isSuccessful 을 통해 각 케이스 핸들링 해주어야 함.

Call은 응답 결과의 성공 유무에 직관적인 처리. Response는 조금 더 간결한 코드 작성 가능.
RxJava 또는 Coroutine을 사용하여 처리하면 앞에 내용들을 작성할 필요 없긴 함.