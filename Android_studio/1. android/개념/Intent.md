액티비티 화면 전환할 때 썼음.
app component 간의 데이터 전달에 씀.(service, broadcast 전송 등)


#### android 에서 컴포넌트 간 통신을 위해 사용되는 메시지 객체.


주요 속성과 메서드
Action : intent의 주요 목적 또는 동작을 나타내는 문자열.
ex) Intent.ACTION_DIAL - 앱에서 전화 걸기 액션 수행.

Data : 액티비티나 서비스로 전달되는 데이터. 
ex) setData() 메서드 사용. 

Type : 데이터의 MIME 유형.
ex) setType() 메서드 사용.
ps. MIME 이란? - Multipurpose Internet Mail Extensions 유래는 이메일과 관련
**MIME 유형**은 **데이터 유형을 식별하는데 사용되는 형식**
http 헤더에서 Context-Type 등으로 지정됨.
ex) text/html, image/jpeg

Category : 추가 속성으로 액티비티가 어떤 종류의 동작을 지원하는지 나타냄.
ex) Intent.CATEGORY_LAUNCHER 는 앱의 런처 카테코리를 나타냄.

Flag : 액티비티나 task의 동작을 제어하는데 사용. (android task, activity stack)
ex) FLAG_ACTIVITY_NEW_TASK, FLAG_ACTIVITY_CLEAR_TOP, FLAG_ACTIVITY_SINGLE_TOP
