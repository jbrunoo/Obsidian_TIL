###### 루틴
	컴퓨터 프로그램에서 하나의 정리된 일 (작업을 정의한 명령어의 집합)
	프로그램은 보통 크고 작은 여러가지 루틴을 조합시켜 성립됨
	루틴은 메인 루틴과 서브 루틴으로 나뉨
###### 메인 루틴
	프로그램 전체의 개괄적인 동작 절차를 표시하도록 만드는 핵심 역할 수행
###### 서브 루틴
	반복되는 특정 기능을 모아 별도로 묶어 놓아 이름을 붙이고 사용하는 하위 루틴
	서브루틴은 별도의 메모리에 해당 기능을 모아 놓고 있어, 서브루틴이 호출될 때마다
	저장된 메모리로 이동했다가 return을 통해 원래 호출자의 위치로 돌아옴
	함수와 비슷한 개념
###### 서브 루틴과 코루틴
	코루틴도 루틴의 일종
		
	서브루틴과의 차이
		코루틴에서는 메인-서브의 개념이 없어 모든 루틴들이 서로를 호출할 수 있음
		메인 루틴에서 특정 서브루틴의 공간으로 이동한 후에 return 의해 호출자로 돌아와
		다시 프로세스를 진행하는 서브루틴과는 다르게, 코루틴은 루틴을 진행하는
		중간에 멈추어서 특정 위치로 돌아갔다가 다시 원래 위치로 돌아와 나머지 루틴을 수행할 수 있음
		서브루틴은 진입점과 반환점이 단 하나밖에 없어 메인루틴에 종속적
		코루틴은 진입지점이 여러 개이기 때문에 메인루틴에 종속적이지 않아 
		대등하게 데이터를 주고 받음

###### ps
	thread 자체는 runnable, running, blocked로만 관리되는데
	코루틴을 이용하면 진입 지점을 다양하게 처리할 수 있다는 의미
	코루틴
		Entry Point를 여러 개 허용하는 subroutine
		언제든 일시 정지하고 다시 실행 가능
		event loops, iterators, 무한 리스트, 파이프 같은 것을 구현하는데 적합
		여러 언어에서 사용되는 개념
			C#, ECMAScript: async/await
			GO: channels, select
			C#, Python: generators/yield
	쉽게 이해하기
		- 스레드는 한명의 점원이 한 줄의 손님을 계산
		- 스레드 풀은 마트처럼 계산대 여러 개 두기
		- 코루틴은 계산대마다 줄을 잘 서는 손님들
	    - 손님 줄 바꾸는 건 가볍지만, 계산대 자체를 바꾸는 건 무거움
		즉, thread는 OS가 context switching 전환 비용
		코루틴은 코틀린 런타임에서 전환 관리
		실제 실행은 스레드 풀의 워커 스레드에서 동작하지만
		- suspend/resume 시 레지스터/스택 전체를 저장하지 않고 최소 상태만 기록하고
		- 사용자 레벨에서 전환이 이루어진다는 점이 가벼움 light-weighting
---
#### Coroutine Basics
###### blocking
	호출된 함수가 자신의 할 일을 마칠 때까지 제어권을 호출한 함수에게 바로 돌려주지 않음
	호출된 함수가 return 되기 전까지 호출한 함수는 다른 일 못함
	Thread 관점에서, 요청한 작업을 마칠 때까지 계속 대기하며 return 값을 받을 때까지
	한 Thread를 계속 사용/대기
###### non-blocking
	호출된 함수가 자신이 할 일을 채 마치지 않았더라도 바로 제어권을 돌려줘(return)
	호출한 함수가 다른 일을 진행할 수 있도록 해줌
	호출된 함수는 할 일을 다 마치고서 callback 호출
	Thread 관점으로 본다면, 하나의 Thread가 여러 개의 IO를 처리 가능
###### CoroutineScope(Dispathcers.IO).launch {...}
	코루틴을 생성하기 위한 코루틴 빌더
	코루틴은 Thread를 Block하지 않음
		fun main()에 코루틴을 구현하면 메인 함수가 종료되기 때문에 메인 스레드 종료와 함께 코루틴 역시 종료
###### runBlocking {...}
	주어진 블록이 완료될 때까지 현재 스레드를 점유(블록 하위 수행하지 않고 대기)하는
	코루틴을 생성하고 실행하는 코루틴 빌더
	연습용. 일반적인 함수 코드 블록에서 suspend fun 호출위한 존재
###### delay
	중단 함수(Blocking function)
	모든 중단 함수들은 코루틴 안에서만 호출 가능
	수행중인 코루틴을 blocking 가능
		ps. thread.sleep()은 thread를 blocked로 보냄
###### Structed concurrency (구조적 동시성)
	상위 코루틴과 하위 코루틴의 구조를 설정하여 자연스럽게 join 효과를 볼 수 있음
```kotlin
fun main() = runBlocking {
	launch {
		delay(300L)
		println("World")
	}
	
	println("Hello, ")
}
```
###### suspend function
	extract function. 코루틴 내에서 호출하고자 하는 함수
###### Scope builder and concurrency
	coroutineScope function은 suspend function 내에서 여러 작업을 동시에 수행 가능
	자식 코루틴 모두 실행이 완료되어야 종료(async/await 동작) (구조적 동시성 보장)
		ps. CoroutineScope랑 다름. 이건 코루틴 실행 범위 나타내는 인터페이스/객체
```kotlin
// CoroutineScope (대문자)
val scope = CoroutineScope(Dispatchers.IO)
scope.launch {
    // IO 스레드에서 실행
}
// coroutineScope (소문자)
suspend fun loadData() = coroutineScope {
    val data1 = async { fetchData1() }
    val data2 = async { fetchData2() }
    // 모든 async가 끝날 때까지 기다림
    data1.await() + data2.await()
}
```
	runBlocking이랑 coroutineScope 모두 현재 block을 수행한 후에 다음이 진행됨
	runBlocking은 현재 Thread에서 코루틴을 만들어서 종료까지 join으로 대기하는 방식,
	호출하는 Thread가 코루틴 내부에서 대기하는 저수준의 blocking
	
	coroutineScope는 async/await 방식으로 대기.
	현재 수행중인 Thread는 언제든 탈출했다가 다른 작업 수행 가능
	
	그래서 runBlocking은 일반 함수, coroutineScope는 suspend fun
	Coroutines ARE light-weight
		코루틴과 thread로 10만개 "." 찍어보면 훨씬 코루틴이 빠름
---
#### Coroutine 만들기
###### CoroutineScope
	코루틴이 실행되는 범위
	GlobalScope: Android Application과 라이프 사이클이 동일한 코루틴
	CoroutineScope: 생성/종료에 맞게 만들고 소멸시킬 수 있는 코루틴 (ex. Activity)
###### CoroutineContext
	코루틴을 실행하기 위한 다양한 설정 정보 값을 가진 객체
		Coroutine, Dispatcher, Job, Exception Handler etc..
	코루틴 컨텍스트는 + 연산을 통해 조합 가능
		val someContext = Dispatcher.IO, aJob, bCoroutine + cExceptionHandler
###### Dispatcher
	어떤 Thread를 사용하여 동작할 것인가를 정의
	Dispatchers.Main
		메인 스레드 동작
	Dispatchers.IO
		Network, Disk IO 등 작업 사용하는 Thread 동작
	Dispatchers.Default
		Main 이외의 worker thread 작업. CPU 사용이 많거나 오랜 시간이 걸리는 작업.
	
	ps. Main Dispatcher은 메인 스레드에서 EventLoop를 이용해 코루틴 실행 스케쥴링
	ps. Unconfined는 코루틴이 재게(suspend -> resume)되는 스레드에서 바로 실행
	ps. Deafault, IO는 디스패처를 통해 코루틴 스케줄러 내부에 진입하고
	스케줄러는 작업 큐(Task Queue)의 형태로 각 코루틴을 관리하고 있음
	ps. Default는 CPU 코어 수와 동일한 스레드 풀을 사용하고 
	이는 최적으로 동작하는데 Blocking한 작업(IO 등)은 스레드를 이용못하게 하니까
	CPU-Intensive한 작업을 처리할 때만 사용하는 것을 권장
	ps. IO는 최소 64개의 스레드 제한. 다른 코루틴을 starve시키거나 성능 병목 현상
	걱정 없이 여러 개의 Blocking IO 작업 실행 가능
	
https://jtm0609.tistory.com/263 참고

###### Coroutine Builder
	코루틴을 생성하는 방법
	launch(): Job
		Job 객체로 코루틴을 제어
		실행하고 잊어버리는 (fire-and-forget) 형태의 코루틴 실행
		즉시 실행되고, 실행 결과는 반환하지 않음
			join을 통해서 완료 대기 가능
			
	async(): Deferred<T>
		await()로 결과 수신 및 작업이 완료될 때까지 기다림
		결과나 예외를 반환, 실행 결과는 Deferred<T>를 통해서 반환됨
			
	runBlocking: T

	ps. coroutineScope는 scope를 생성하는 거라 실행은 scope에서 또 launch 등을 해야 함
		이는 빌더보다는 coroutine scope function으로 소개됨
	
	launch는 관리할 수 있는 Job을 리턴
		start()
			scope가 아직 시작하지 않을 경우 start, scope의 상태를 확인
		join()
			start()와 동일하게 실행 시킴 + scope 동작 끝날 때까지 결과 대기까지
			suspend에서 호출 가능
		cancel()
			동작을 종료하도록 호출
	async
		await로 결과를 받을 수 있음
		
```kotlin
fun main() = runBlocking {
    doSomething()
    println("Done")
}
private suspend fun doSomething() = coroutineScope {
    val one = async {
        delay(1000L)
        "result ONE"
    }
    val two = async {
        delay(2000L)
        "result TWO"
    }
    launch {
        println("test")
        val result = "one : ${one.await()} , two : ${ two.await()}"
        println(result)
    }
}
```
	즉시 실행, 지연 실행
		기본 코루틴은 즉시 실행
		CoroutineStart.LAZY로 설정하면 코루틴 block의 실행을 호출 시점까지 대기 가능
		
```kotlin
import kotlinx.coroutines.*
fun main(): Unit = runBlocking {
    val job = CoroutineScope(Dispatchers.Default).launch(start = CoroutineStart.LAZY) {
        println("CoroutineScope.launch Lazy Start")
    }
    val deferred = async(start = CoroutineStart.LAZY) {
        println("async Lazy Start")
        "async Lazy Result"
    }
    println("Start")
    
// launch는 start() 또는 join()으로 해당 코루틴을 실행
//    job.start()
     job.join()	// start()와 동일하게 실행시킴. join은 결과 대기까지
// async는 start() 또는 await()으로 해당 코루틴을 실행
// start()는 결과를 대기하지 않으므로 실행결과인 true를 리턴
//    val result = deferred.start()	
   val result =  deferred.await()	// await는 실행시키고 대기까지
    println("result : $result")
}
```
###### GlobalScope
	CoroutineScope를 상속받아 구현되어 있으며,
	Android Application과 동일한 라이프사이클을 갖는 코루틴(싱글톤)으로 설계됨
	시스템 전체에 영향이 미치므로 초기부터 메모리 문제 없이 잘 구현하도록 권고.
	실행 후 취소가 어려워 memory leak 이슈로 1.5부터 Delicate API로 표기됨
	
	Application 전체 lifecycle에 사용해야 하는 경우는
	@Optin(DelicateCoroutinesApi::class) annotation 명시적 작성 필요
---
#### coroutine scope functions
	build와 scope function을 다같이 builder로 표현하기도 하지만,
	scope function은 builder(launch, async/await)로 구현된 function
	suspend function으로 정의되어 있는 추가적인 function 의미
	
	ex) withContext, withTimeout, coroutineScope, viewModelScope, lifecycleScope ..
###### withContext
	실행되는 Coroutine Context 바꿀 수 있음
	block 실행 결과를 받을 수 있음
	async는 await를 호출하지만 withContext는 결과를 대기
```kotlin
fun main() = runBlocking{
    println("Start : before withContext :$coroutineContext")
    val time = withContext(Dispatchers.Default) {
        println("Start withContext : $coroutineContext")
        delay(3000)
        "after 3 seconds"
    }
    println("Start : after withContext")
    println("Result : $time")
}
```
###### withTimeout
	정해진 시간만큼 수행하고 시간이 지나면 TimeoutCancellationException 발생
###### coroutineScope
	일시 중단 함수. 호출한 곳(continuation)부터 coroutine context를 받음
	모든 자식들의 종료를 기다린 후 scope를 종료
	parent가 cancel되면 모든 child도 cancel
```kotlin
fun main() = runBlocking{
    launch {
        delay(200L)
        println("2. Task from runBlocking")
    }
    coroutineScope {
        launch {
            delay(500L)
            println("3. Task from nested launch")
        }
        delay(100L)
        println("1. Task from coroutine scope")
    }
    println("4. Coroutine scope is over")
}
```
---
#### Thread
	프로세스 내에서 순차적으로 실행되는 실행 흐름의 최소 단위
	프로세스
		운영체제에 의해 메모리에 적재되어 실행 중인 프로그램
		프로세스는 프로그램에 사용되는 데이터와 메모리 등의 자원 그리고 스레드로 구성
	
	모든 프로세스에는 한 개 이상의 스레드가 작업을 수행
	하나의 프로세스 내에서 두 개 이상의 스레드가 동작하도록 하는 것을 멀티 스레드 프로그래밍
	각 스레드가 서로 교체될 때 스레드 간의 문맥 교환 (context switching) 발생
	context switching은 현재까지 작업 상태와 다음 작업 필요한 데이터를 저장하고 읽어오는 작업
###### Main Thread
	프로세스가 시작될 때 최초의 실행 시작점부터 순차적으로 진행되는 실행 흐름
	ps. java에서는 일반 스레드. Android에서는 UI Thread 의미
	프로세스 실행 중 언제든지, 어느 스레드에서나 새로운 스레드를 만들 수 있음
```kotlin
class Exam1Activity : AppCompatActivity() {
    private var isRunning = true
    private val binding: ActivityExam1Binding by lazy {
        ActivityExam1Binding.inflate(layoutInflater)
    }
    // Thread 생성 방법
    // 1. Thread 상속
    inner class ThreadClass : Thread() {
        // 실제 수행할 코드
        override fun run() {
            Log.d(TAG, "run: currentThread-${Thread.currentThread().name}")
            super.run()
            while (isRunning) {
                sleep(1000)
                Log.d(TAG, "run: ${System.currentTimeMillis()}")
                // 스레드에서 UI 작업 시 에러 발생
                binding.tvTime.text = System.currentTimeMillis().toString()
            }
        }
    }
    // 2. Runnable 함수 작성 -> Thread 객체에 전달
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContentView(binding.root)
        ViewCompat.setOnApplyWindowInsetsListener(binding.root) { v, insets ->
            val systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars())
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom)
            insets
        }
        Log.d(TAG, "onCreate: currentThread-${Thread.currentThread().name}")
        val firstThread = ThreadClass("내가만든스레드")
        // Thread 실행 주제는 운영 체제 (안드로이드 운영체제)
        // run 호출은 안드로이드 운영 체제
        // start(): 운영 체제한테 이 쓰레드가 준비 됐음을 알려 주는 역할
        // 실행 시점은 안드로이드 운영 체제 마음
        firstThread.start()
    }
    override fun onPause() {
        isRunning = false
        super.onPause()
    }
}
```
---
#### Android UI Thread와 Handler
	Main Thread는 메시지 큐 수신을 대기하는 Looper를 가지며, 사용자 입력과 시스템 이벤트,
	화면 그리기 등의 메시지가 수신되면 각 메시지에 매핑된 핸들러의 메서드를 실행
	Main이 아닌 Thread에서 UI 관련 작업을 수행하려면, Handler를 통해 메인 스레드로
	메시지를 전송하고 해당 작업을 Main Thread에서 처리해야 함
	일반 스레드는 Looper를 갖지 않고, HandlerThread를 생성하면 Looper를 갖는 Thread 만들 수 있음
	Thread는 Thread 외부에서 전달된 message 등이 인입되는 Queue를 가짐
	Looper는 Queue에 message 혹은 Runnable 객체가 있다면, Handler로 전달
		ps. looper는 처리할 메시지가 있는지 계속 for문을 돌리고 있는 것과 비슷
	Handler는 Looper가 보낸 message 혹은 Runnable 객체를 처리
```kotlin
class Exam1Activity : AppCompatActivity() {
    private val dogs = arrayOf(R.drawable.dog1, R.drawable.dog2, R.drawable.dog3)
    private var currentDogIndex = 0
    private val handler = Handler(Looper.getMainLooper())
    private var isRunning = true
    private val binding: ActivityExam1Binding by lazy {
        ActivityExam1Binding.inflate(layoutInflater)
    }
    // Thread 생성 방법
    // 1. Thread 상속
    inner class ThreadClass(threadName: String) : Thread(threadName) {
        // 실제 수행할 코드
        override fun run() {
            Log.d(TAG, "run: currentThread-${Thread.currentThread().name}")
            super.run()
            while (isRunning) {
                sleep(1000)
                Log.d(TAG, "run: ${System.currentTimeMillis()}")
                handler.post {
                    binding.tvTime.text = System.currentTimeMillis().toString()
                    binding.ivDogs.setImageResource(dogs[currentDogIndex])
                }
                currentDogIndex = (++currentDogIndex % 3)
            }
        }
    }
    // 2. Runnable 함수 작성 -> Thread 객체에 전달
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContentView(binding.root)
        ViewCompat.setOnApplyWindowInsetsListener(binding.root) { v, insets ->
            val systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars())
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom)
            insets
        }
        Log.d(TAG, "onCreate: currentThread-${Thread.currentThread().name}")
        val firstThread = ThreadClass("내가만든스레드")
        // Thread 실행 주제는 운영 체제 (안드로이드 운영체제)
        // run 호출은 안드로이드 운영 체제
        // start(): 운영 체제한테 이 쓰레드가 준비 됐음을 알려 주는 역할
        // 실행 시점은 안드로이드 운영 체제 마음
        firstThread.start()
    }
    override fun onPause() {
        isRunning = false
        super.onPause()
    }
}
```
###### runOnUiThread
	수행코자 하는 코드를 Main Thread가 처리하도록 하는 메서드
	별도 생성한 Thread에서 UI 작업이 필요한 경우에 사용
	개발자가 만든 Runnable 객체를 메인 스레드에서 실행되도록 만드는 메서드
	현재 스레드가 메인 스레드인지 검사해서 아니라면 post() 실행
	메인 스레드라면 Runnable의 run() 메서드를 직접 실행
	Runnable 객체에 구현된 코드를 반드시 메인 스레드에서 실행해야할 때 사용하는 메서드