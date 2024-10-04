백그라운드 작업 방식 - 공식문서 상 가벼운 작업에 추천하고 있음. 
[블로그](https://medium.com/@limgyumin/%EC%83%88%EB%A1%9C%EC%9A%B4-%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C-%EB%B0%B1%EA%B7%B8%EB%9D%BC%EC%9A%B4%EB%93%9C-%EC%9E%91%EC%97%85-%EC%B2%98%EB%A6%AC%EB%B2%95-workmanager-f625e07b384c)

WorkManager는 매우 유연한 라이브러리로, 다음과 같은 이점.

- 비동기 일회성 작업과 주기적인 작업 모두 지원
- 네트워크 상태, 저장공간, 충전 상태와 같은 제약 조건 지원
- 동시 작업 실행과 같은 복잡한 작업 요청 체이닝
- 한 작업 요청의 출력이 다음 작업 요청의 입력으로 사용됨
- API 수준 14까지 호환됨(참고 확인)
- Google Play 서비스를 사용하거나 사용하지 않고 작업
- 시스템 상태 권장사항 준수
- 앱 UI에 작업 요청의 상태를 쉽게 표시할 수 있도록 지원

하려는 작업 주기적으로 공공 api 데이터 업데이트된 정보를 주기적으로 업데이트해서 홈 화면 업데이트된 이미지 5개씩 보여주려고 함


알아야 할 몇 가지 WorkManager 클래스.

- [**`Worker`**](https://developer.android.com/reference/androidx/work/Worker?hl=ko)/[**`CoroutineWorker`**](https://developer.android.com/reference/androidx/work/CoroutineWorker?hl=ko): worker는 백그라운드 스레드에서 동기식으로 작업을 실행하는 클래스입니다. 우리는 비동기 작업에 관심이 있으므로 Kotlin 코루틴과 상호 운용되는 CoroutineWorker를 사용할 수 있습니다. 이 앱에서는 CoroutineWorker 클래스에서 확장하고 [`doWork()`](https://developer.android.com/reference/androidx/work/CoroutineWorker?hl=ko#doWork()) 메서드를 재정의합니다. 백그라운드에서 실행하고자 하는 실제 작업의 코드를 이 메서드에 입력합니다.
- [**`WorkRequest`**](https://developer.android.com/reference/androidx/work/WorkRequest.html?hl=ko): 이 클래스는 작업 실행 요청을 나타냅니다. `WorkRequest`에서는 worker를 한 번 또는 주기적으로 실행해야 하는지 정의합니다. [제약 조건](https://developer.android.com/reference/androidx/work/Constraints.html?hl=ko)은 작업 실행 전에 특정 조건 충족을 요구하는 `WorkRequest`에 배치될 수도 있습니다. 한 가지 예는 요청된 작업을 시작하기 전에 기기를 충전하는 것입니다. `WorkRequest`를 만드는 과정에서 `CoroutineWorker`를 전달합니다.
- [**`WorkManager`**](https://developer.android.com/reference/androidx/work/WorkManager.html?hl=ko): 이 클래스는 실제로 `WorkRequest`를 예약하고 실행합니다. 지정된 제약 조건을 준수하면서 시스템 리소스에 부하를 분산하는 방식으로 `WorkRequest`를 예약합니다.


[work Manager](https://developer.android.com/topic/architecture/data-layer?hl=ko#workmanager)

