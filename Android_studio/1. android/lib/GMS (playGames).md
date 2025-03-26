[공식문서 - 체크리스트](https://developer.android.com/games/pgs/quality?_gl=1*1yua260*_up*MQ..*_ga*MTkxNjY3Mzg2MS4xNzQyNjMzOTcy*_ga_6HH9YJMN9M*MTc0MjYzMzk3MS4xLjAuMTc0MjYzMzk3MS4wLjAuNTc5NDc4OTc1)


digitInk 앱에 리더보드를 적용시켜 보려고 한다.

basic.
SDK 설정
플레이 콘솔 - 사용자 늘리기 - Play 게임즈 서비스 - 설정 및 관리 - 리더보드
(중간에 OAuth 2.0 동의화면, 사용자 인증 정보 등 설정 필수)
클라이언트 구현

- - - 
변경점 games v1 -> games v2
변경되면서 기존 GoogleSignInOption 활용하여 로그인을 진행하고 Games SDK를 이용한 방식에서
PlayGames SDK 하나로 통합되었음.

```kotlin
@Singleton
class PlayGamesManager {
    private lateinit var gamesSignInClient: GamesSignInClient
    private lateinit var leaderboardsClient: LeaderboardsClient
    private val _isAuthenticated = MutableStateFlow(false)
    val isAuthenticated: StateFlow<Boolean> = _isAuthenticated.asStateFlow()

    fun initialize(activity: Activity) {
        gamesSignInClient = PlayGames.getGamesSignInClient(activity)
        leaderboardsClient = PlayGames.getLeaderboardsClient(activity)
    }

    fun checkAuth() {
        gamesSignInClient.run {
            isAuthenticated().addOnCompleteListener { isAuthenticatedTask: Task<AuthenticationResult> ->
                val authResult =
                    (isAuthenticatedTask.isSuccessful && isAuthenticatedTask.result.isAuthenticated)

                _isAuthenticated.value = authResult

                if (authResult) {
                    Timber.d("complete authenticated")
                } else {
                    Timber.d("not authenticated - need signIn")
                    signIn()
                }
            }
        }
    }

    fun showLeaderBoard(activity: Activity, leaderBoardKey: String) {
        checkAuth()

        if (!_isAuthenticated.value) {
            Timber.d("showLeaderBoard - not Authenticated")
            return
        }

//        leaderboardsClient.allLeaderboardsIntent.addOnSuccessListener { intent ->
//            ActivityCompat.startActivityForResult(
//                activity,
//                intent,
//                RC_LEADERBOARD_UI,
//                null
//            )
//        }

        leaderboardsClient.getLeaderboardIntent(leaderBoardKey)
            .addOnSuccessListener { intent ->
                ActivityCompat.startActivityForResult(
                    activity,
                    intent,
                    RC_LEADERBOARD_UI,
                    null
                )
            }
    }

    fun submitScore(leaderBoardKey: String, score: Long) {

        if (!_isAuthenticated.value) {
            Timber.d("submitScore - not Authenticated")
            return
        }

        leaderboardsClient.submitScore(leaderBoardKey, score)
    }

    companion object {
        private const val RC_LEADERBOARD_UI = 9004
    }
}
```
위와 같은 형태로 작성해보았다.
일단 기본적인 코드를 차용하여 checkAuth 부분이 비동기 처리가 되어있긴한데, 
isAuthenticated()에서 인증 결과를 가져와서 분기 처리해도 될 것으로 보이긴 한다.

gameSignInClient에서 signIn 메서드를 제공하고 있다. (auth와 같은 추가 라이브러리 필요 x)

공식문서에는 각 leaderBoardId를 받아 개별 리더보드 띄우는 코드만 있는데,
leaderboardsClient.allLeaderboardsIntent를 활용하면 모든 리더보드를 한번에 띄울 수 있을 것 같다.
(아직 테스트 x)

기존 문서에 startActivityForResult 코드가 적혀있는데
이는 이후에 onActivityResult()에서 결과를 처리하는 과정에서 acitivty가 종료될 수 있기 때문에 
deprecated 되었음.
registerForActivityResult()를 공식문서에서 권장하는데 callback이 내장되어있다.
이를 launcher로 사용한다.

현재는 activity에 코드를 작성한게 아니라 아직 변경하지 않았다.

- - -
playGames에서 리더보드가 연결되지 않는 경우 문제는 대부분 sha-1 key.
debug, release 키말고 배포 시에는 플레이 콘솔에 있는 앱 서명 키를 gcp에 등록하여 콘솔에서 사용.

리더보드 submitScore가 동작을 안해서 살펴보는 중 - 소수점 2자리 때문에 착각한 것 같은데 최대 점수 제한을 소수점 2자리 낮게 설정해서 그런 것 같다. 100.00 이면 최대 100으로 설정했는데 10000이야 되어야 하는 상황.

추가로 저장된 게임 사용하려면 v2 라이브러리 기준으로 `SnapshotsClient`에 구현되어있는 메서드를 뒤져보자.