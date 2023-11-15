에러 
```
C:\Users\6pizz\PycharmProjects\flaskProject\mvenv\Scripts\python.exe -m flask run 
Fatal Python error: init_stdio_encoding: failed to get the Python codec name of the stdio encoding
Python runtime state: core initialized
LookupError: unknown encoding: x-windows-949

Current thread 0x00007200 (most recent call first):
  <no Python frame>
```

해결방법 : encoding 문제 , settings -> File Encodings에서 UTF-8로 맞추어줌.

![[Pasted image 20231114135939.png]]

위에 방식으로는 해결 안됨.

![[Pasted image 20231114140909.png]]

settings -> editor -> General -> Console의 Default Encoding을 UTF-8로 만들어 해결.
앞에 부분은 그대로 Default고 이 부분이 변경되야 해결되는 것 같음.


프레임 워크 공부 시 가장 먼저 알아야 할 것은 라우팅은 어떻게 하는지?
라우팅 : 어떤 요청을 어떤 함수가 응답할지 연결해주는 작업
그런 작업을 기술하는 것 라우터

라우터가 허용하는 methods (기본값 GET)
