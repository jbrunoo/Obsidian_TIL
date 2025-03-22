현재 구글 로그인 API가 Credential Manager로 migration 하라고 안내한다.

기존에도 찾아 보았지만 차이 나는 부분이 존재한다.

일단 로그인 함수가 suspend로 구현되어 있다.

패스키를 포함한 여러 자격 증명 관련 key를 이용하여 로그인을 지원한다.

기존과 다르게 id_token 외의 토큰 값들을 얻을 수 없다.

`androidx.credentials.exceptions.NoCredentialException: No credentials available` 에러를
마주할 수 있는데, 기존 디바이스에 연결된 계정이 없는 경우 발생한다. 

```kotlin
val googleIdOption = GetGoogleIdOption.Builder()
    .setFilterByAuthorizedAccounts(true)//first pass true
    .setFilterByAuthorizedAccounts(false)//then false
    .setServerClientId(K.CLIENT_ID)
    .setNonce(hashedNonce)
    .build()
```

`.setFilterByAuthorizedAccounts(false)` 해당 값을 false로 두면 연결된 계정이 모두 뜨고 true로 두면 이전에 연결된 계정 중에서 어떤 계정으로 로그인할지 알림을 준다. stackoverflow에서는 저렇게 2가지를 동시에 적어두면 순차적으로 체크를 한다고 하는데 아직 시도는 안해보았다.

위처럼 true로 두고 연결된 계정이 없을 때도 뜨는 에러지만, 에뮬레이터처럼 애초에 연결된 계정이 없으면 추가 안내 없이 해당 에러를 발생시킨다. 이 경우 로그인을 하는 방법이 마땅치 않다. 기존 로그인을 사용해야 한다.
ps. 사용자가 자동 연결을 막아둔 경우, 직접 해제해야 한다는 내용을 전달하기가 복잡하다.


위 내용을 제외하고 연결된 계정이 있으면 바텀 시트 UI와 함께 로그인을 바로 볼 수는 있다.

연결된 계정이 없는 경우 + 서버와 작업할 때 처리하는게 조금 복잡해진듯 하다.