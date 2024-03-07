## activity lifecycle
- activity는 사용자와 상호작용하는 진입점.
- Created - Started - Resumed - activity is running - Paused - Stopped - Destroyed
- 앱에서 발생해야하는 동작을 맞춤설정하기 위해 activity에서 재정의할 수 있는 콜백 메서드 존재
- onCreate(), onStart(), onResume(), onPause(), onStop(), onDestroy()
- activity에서 모든 정보를 재정의할 필요는 없지만 activity에서 로드하거나 표시하려는 정보에 따라 주요 정보를 재정의할 수도 있음.
- 목표는 앱이 activity 수명 주기를 존중하고 수명 주기의 적절한 시점에 적절한 설정 및 정리 작업을 수행하여 리소스를 효율적으로 사용하고 사용자 상태를 보존하는 것.

- activity가 소멸될 때 운영체제(OS)에서 메모리를 회수함.
- 일반 프로그램이 main() 시작하듯, android는 onCreate() 메서드 시작.
- 사용자가 앱 시작 후 activity 간에 이동하고 앱 안팎으로 이동할 때 activity는 상태 변경.

### 사용할 수 있는 재정의 가능한 콜백과 수명 주기 상태
![[Pasted image 20231101115709.png]]
ps. `onRestart()`는 Created, Started 간에 상태가 전환할 때마다 이 메서드가 호출되지 않고 `onStop()`이 호출된 후, activity가 다시 시작되는 경우만 호출됨.

activity 수명 주기 상태가 변경될 때, 일부 동작을 변경하거나 코드를 실행하려고 하는 경우 많음. 따라서 activity 클래스 자체와 모든 서브 클래스(ComponentActivity 등)는 일련의 수명 주기 콜백 메서드 구현.

앱이 완전히 화면에 표시되고 사용자 포커스를 보유하는 수명 주기 부분을 포그라운드 전체 기간.
앱이 백그라운드로 이동하면 `onPause()` 후에 포커스가 상실되고 `onStop()` 후에 더 이상 앱이 표시되지 않습니다.

포커스와 가시성의 차이. Resumed 에서는 포커스와 가시성을 가지지만 Started는 가시성만 가짐.

활동이 화면에 _부분적으로_ 표시되지만 사용자 포커스는 없을 수 있습니다. 이 단계에서는 활동이 부분적으로 표시되지만 사용자 포커스가 없는 한 가지 사례를 살펴봅니다.
아래 경우
![[Pasted image 20231101160220.png]]



![[Pasted image 20231101155954.png]]

구성 변경 시 `onDestroy()`까지 호출됨.
ex) 사용자 기기 언어 변경(텍스트 방향, 문자열 길이 수용하도록 레이아웃 변경), 기기 도크 연결, 물리적 키보드 추가, 방향 변경(가로, 세로 회전 - 모든 수명 주기 호출됨)

## composable 수명 주기
앱 상태 변경시 recomposition 예약. 
recomposition : composable 함수를 Compose에서 다시 실행하고 업데이트한 UI를 만드는 것
composition을 만들거나 업데이트 하는 유일한 방법은 초기 컴포지션 or 후속 리컴포지션.
activity 수명 주기와 별개로 composable은 자체 수명 주기를 가짐.
Compose가 recomposition을 추적하려면 상태(state)가 변경된 시점 필요.
객체 상태를 추적해야 하므로 객체 유형 `state`(읽) or `mutablestate`(읽, 쓰).
리컴포지션 중 값을 유지하고 재사용하도록 remember API를 사용.
```kotlin
var revenue by remember { mutableStateOf(0) }
```
= 쓰면 getter. = 이여도 구조 분해로 getter, setter 가져오거나 by 쓰기.

but, recomposition 중에 상태 기억하지만 구성 변경 중에는 상태 유지x 
유지하려면 rememberSaveable 써야 함. (아니면 `viewModel`)

ps. Android는 시스템이 스트레스 받고 시각적 지연 위험이 있을 때 앱 전체 프로세스(+앱과 관련된 모든 activity 포함) 종료함.
콜백이나 코드가 추가로 실행x. 백그라운드에서 자동으로 간단히 종료됨.
사용자에게는 앱이 닫힌 것처럼 보이지 않음. 누르면 앱 다시 시작.
사용자가 데이터 손실을 겪지 않게 주의.


## 로그 찍어보기
```kotlin
override fun onResume() {    super.onResume()    Log.d(TAG, "onResume Called")}
override fun onRestart() {    super.onRestart()    Log.d(TAG, "onRestart Called")}
override fun onPause() {    super.onPause()    Log.d(TAG, "onPause Called")}
override fun onStop() {    super.onStop()    Log.d(TAG, "onStop Called")}
override fun onDestroy() {    super.onDestroy()    Log.d(TAG, "onDestroy Called")}
```
각 lifecycle에서 log를 찍고 안드로이드 앱을 동작시키면서 logcat을 통해 확인 가능. log.d(태그(일반적으로 클래스 이름), 로그메시지(짧은 문자열))

- 로그 메시지의 **_우선순위_**, 즉 메시지의 중요도. 여기서는 [`Log.v()`](https://developer.android.com/reference/kotlin/android/util/Log?hl=ko#v_1)가 상세 메시지를 기록합니다. [`Log.d()`](https://developer.android.com/reference/kotlin/android/util/Log?hl=ko#d) 메서드는 디버그 메시지를 작성합니다. `Log` 클래스의 다른 메서드에는 정보 메시지의 경우 [`Log.i()`](https://developer.android.com/reference/kotlin/android/util/Log?hl=ko#i_1), 경고의 경우 [`Log.w()`](https://developer.android.com/reference/kotlin/android/util/Log?hl=ko#w(kotlin.String,%20kotlin.String)), 오류 메시지의 경우 [`Log.e()`](https://developer.android.com/reference/kotlin/android/util/Log?hl=ko#e(kotlin.String,%20kotlin.String))가 있습니다.
- 
- 스켈레톤 재정의 메서드를 클래스에 추가하려면.. 1. Android 스튜디오에서 `MainActivity.kt`가 열려 있고 `MainActivity` 클래스 내 커서가 있는 상태에서 **Code > Override Methods...**를 선택하거나 `Control+O`를 누릅니다. 이 클래스에서 재정의할 수 있는 모든 메서드의 긴 목록이 있는 대화상자가 표시됩니다.
