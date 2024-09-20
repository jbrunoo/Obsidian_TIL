[공식문서](https://ktor.io/docs/server-custom-plugins.html)

내용을 읽어보면 
[handling requests and responses](https://ktor.io/docs/server-custom-plugins.html#call-handling) using the `onCall`, `onCallReceive`, and `onCallRespond` handlers.

즉, request, response가 Router에 도달하거나 나갈 때 onCallReceive, onCallRespond가 가로채서 data transform, add header 등의 기능을 수행할 수 있도록 함.
ps. android Okhttp의 intercepter 기능 같은?

