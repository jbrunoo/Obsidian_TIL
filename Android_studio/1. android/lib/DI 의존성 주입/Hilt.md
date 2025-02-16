# Hilt 사용법

[유튜브 참고](https://www.youtube.com/watch?v=wZn-zpwvxCU)from 8:22 글로만 적음.
1. 반드시 @HiltAndroidApp annotation이 지정된 Application 클래스가 있어야 함.
	Hilt가 안드로이드 생명주기에 맞춰 동작하게 되므로 Application 객체가 필요.
2. 안드로이드 component에 주입을 요청.
3. MainViewModel 객체 생성 방법을 알려주고 RemoteRepository 주입을 요청.
4. RemoteRepository 객체 생성 방법을 알려줌.

생성자 앞에 `@Inject`를 달아줌으로써 객체 생성 방법을 알려줄 수 있음.
activity나 fragment에서 viewModel을 주입받을 때, activity ktx, fragment ktx 기능으로 (?)

사용 방법 따로 익히기.

# dagger hilt - compose에서 적용
[공식문서](https://developer.android.com/training/dependency-injection/hilt-android?hl=ko#kotlin)
[유튜브](https://www.youtube.com/watch?v=bbMsuI2p1DQ)

![[Pasted image 20231123123625.png]]

  
의존성 주입(Dependency Injection)은 소프트웨어 디자인 패턴 중 하나로, 클래스가 직접 자신의 의존성을 생성하지 않고 외부에서 주입받는 것. 안드로이드에서 Hilt와 같은 의존성 주입 프레임워크를 사용하여 구현

"의존성"이란 다른 클래스나 모듈에 대한 의존 관계를 의미
클래스 A가 클래스 B에 의존한다면, A는 B의 기능이나 객체를 사용하거나 확장하는 것

의존성 주입 주로 세 가지 형태:

1. **생성자 주입(Constructor Injection):** 클래스의 생성자를 통해 의존성을 주입하는 방식. 클래스가 인스턴스화될 때 외부에서 필요한 의존성을 전달받습니다. Hilt에서는 `@Inject` 어노테이션을 사용하여 생성자에 주입할 의존성을 표시합니다.
    
    javaCopy code
    
    `// 예시: Hilt를 사용한 생성자 주입 class MyClass @Inject constructor(private val dependency: Dependency) {     // ... }`
    
2. **메소드 주입(Method Injection):** 클래스의 메소드를 통해 의존성을 주입하는 방식. 특정 메소드에 의존성을 전달받아 사용.
    
    javaCopy code
    
    `// 예시: Hilt를 사용한 메소드 주입 class MyClass {     @Inject     fun setDependency(dependency: Dependency) {         // ...     } }`
    
3. **필드 주입(Field Injection):** 클래스의 필드를 통해 의존성을 주입하는 방식. 클래스의 필드에 `@Inject` 어노테이션을 사용하여 주입할 의존성을 표시.
    
    javaCopy code
    
    `// 예시: Hilt를 사용한 필드 주입 class MyClass {     @Inject     private lateinit var dependency: Dependency }`


ps. 메소드나 필드 주입은 프로젝트 내에서 클래스의 인스턴스를 바로 제공해줄 수 있어야 사용할 수 있음.
즉, 추상 클래스, 인터페이스(어느 구현체가 객체로 사용될지 알 수 없기 때문)로 구현되거나 room, retrofit 같은 외부 라이브러리는 hilt module을 추가하고 생성자 주입을 통해 의존성을 전달함. 이 과정에서 constructor를 가질 수 없는 추상 클래스, 인터페이스는 @Binds로, 프로젝트 내에서 소유하지 않거나 빌드 패턴으로 인스턴스를 생성해야 하는 경우 @Provides를 사용함.
++ 특히 @Provides만 포함되는 모듈의 경우 object로 생성 시, providers get optimized and almost in-lined in generated code 라고 적혀있음, @Binds는 class, interface, abstract class로 생성함
+++ 

- - -
이러한 의존성 주입은 코드의 유지보수성을 향상시키고 테스트 가능한 코드를 작성하는데 도움
또한 객체 간의 결합도를 낮춰 시스템을 확장하거나 변경하기 쉬움


MVVM  + DI 예제 코드
[compose, retrofit, MVVM](https://medium.com/@jecky999/building-an-android-app-with-jetpack-compose-retrofit-and-mvvm-architecture-12a5e03eb03a)
[retrofit with MVVM](https://saurabhjadhavblogs.com/retrofit-with-mvvm-in-jetpack-compose) 
[roomdb with Flow & DI](https://saurabhjadhavblogs.com/compose-mvvm-roomdb-with-flow-and-di)


```kotlin

// DI 라이브러리 X
// ViewModel 
class MyViewModel(private val repository: MyRepository) : ViewModel() { 
	// ViewModel의 로직 구현 
} 
// Repository 
class MyRepository(private val apiService: ApiService) { 
	// Repository의 로직 구현 
} 
// ApiService 
class ApiService { 
	// 네트워크 호출 등의 로직 구현 
}

// hilt 이용
// ViewModel 
class MyViewModel @Inject constructor(private val repository: MyRepository) : ViewModel() { // ViewModel의 로직 구현 
} 
// Repository 
class MyRepository @Inject constructor(private val apiService: ApiService) { 
	// Repository의 로직 구현 
} 
// ApiService class 
ApiService @Inject constructor() { 
	// 네트워크 호출 등의 로직 구현 
}
```


- - -
사용법 추가

Hilt를 사용하면 앱의 구성 요소에 대한 의존성을 자동으로 주입 가능.
Android에서는 아래 클래스들을 지원
- `Application`(`@HiltAndroidApp`을 사용하여)
- `ViewModel`(`@HiltViewModel`을 사용하여)
- `Activity`
- `Fragment`
- `View`
- `Service`
- `BroadcastReceiver`

1.
위 제공하는 클래스의 경우 생성자 주입 등으로 간편히 사용 가능
	viewmodel.factory 같은 보일러플레이트 코드 제거

2.
인터페이스나 외부 라이브러리 같은 경우는 생성자 삽입을 할 수 없음
hilt 모듈을 사용하여 hilt에 결합 정보 알림(@Module)
또한, @InstallIn 주석으로 각 모듈을 사용하거나 설치할 android 클래스를 hilt에 알려야 함
[InstallIn에 참조 가능한 구성요소](https://developer.android.com/training/dependency-injection/hilt-android?hl=ko#generated-components) [구성요소의 수명 주기](https://developer.android.com/training/dependency-injection/hilt-android?hl=ko#component-lifetimes)

3-1.
인터페이스의 경우, @Binds로 추상 함수 생성하여 hilt에 결합 정보 제공
3-2.
retrofit, okhttpclient, room 등 외부 라이브러리에서 제공되므로 클래스를 소유하지 않은 경우,
또는 빌더 패턴으로 인스턴스를 생성해야 하는 경우,
@Provides로 함수 생성하여 hilt에 알림

ps. 주석이 지정된 함수가 제공하는 것 : 함수 반환 유형, 함수 매개변수는 제공할 구현을 hilt에 알림


- - -
hilt 에러
1. \[Hilt]
	project와 app gradle 버전이 일치하지 않았음
2. java.lang.RuntimeException: Unable to instantiate application com.jbrunoo.proverbquiz.MainApplication package com.jbrunoo.proverbquiz: java.lang.ClassNotFoundException: Didn't find class "com.jbrunoo.proverbquiz.MainApplication" on path: DexPathList[[zip file "/data/app/~~GxufCpyH8VymYjXgZoUwYQ==/com.jbrunoo.proverbquiz-rfIOCZvb5IO0SqOQct76oQ==/base.apk"],nativeLibraryDirectories=[/data/app/~~GxufCpyH8VymYjXgZoUwYQ==/com.jbrunoo.proverbquiz-rfIOCZvb5IO0SqOQct76oQ==/lib/arm64, /system/lib64, /system_ext/lib64]]
	annotationProcessor -> kapt로 해결(ksp는 안되었음 hilt에서 ksp 지원 알파단계라 지원안되는 버전이 있음)


screen에서 viewmodel 을 주입할 때 차이 ? [벨로그](https://velog.io/@wlsrhkd4023/Compose-hiltViewModel%EA%B3%BC-viewModel-%EC%B0%A8%EC%9D%B4)
```kotlin
// 보통 아래처럼 쓰는데 연습 코드를 작성해보다 화면 전환도 없고 굳이 더 추가할 필요 없어보여서 위로 구현하고 든 궁금증
viewModel: todoViewModel = viewModel()
viewModel: todoViewModel = hiltViewModel() // hilt-navigation-compose 디펜던시 추가


```


- - -
@Assisted 관련 어노테이션
[공식문서](https://dagger.dev/hilt/view-model.html)
[참고 블로그](https://medium.com/@alexander.michaud/hiltviewmodel-assisted-injection-with-compose-a800723165bf)

- history
	기존 dagger에만 있던 기능. 1.2 이후 추가됨
	hilt에 추가되었으나 hiltViewModel과 함께 사용 안되고 직접 di용 @EntryPoint interface를 선언하여 사용.
	현재는 `@hiltViewModel(assistedFactory = MyViewModel.MyViewModelFactory::class)` 가능.


- 언제 사용 하는지?
	런타임에 동적인 값과 함께 vm을 생성할 경우.
	compose-navigation을 활용하니 navigate하면서 값을 넘겨주는 것이 더 간결하고 보편적이라 사용할 경우가 거의 없었다.
	navigation을 사용하지 않고 vm을 생성하는 경우나 navigate할 때 값을 넘겨주지 못하고 네트워크 호출로 가져온다거나 ..

- 구현:
@AssistedInject constructor 내에서 @Assisted 파라미터 추가 하고
@AssistedFactory interface에서 create를 구현.
```kotlin
@HiltViewModel(assistedFactory = MyViewModel.MyViewModelFactory::class)
class MyViewModel @AssistedInject constructor(
    @Assisted private val myParam: Int,
    private var repository: MyRepository
): ViewModel() {
    @AssistedFactory
    interface MyViewModelFactory {
        fun create(myParam: Int): MyViewModel
    }
    ...
}
```

```kotlin
val viewModel: MyViewModel = hiltViewModel(
        creationCallback = { factory: MyViewModel.MyViewModelFactory -> 
          factory.create(myParamValue) 
        }
    )
```


- - -
같은 자료형의 di를 주입할 때
ex) Retrofit을 주입하는데 다른 baseUrl이라면? 다른 okHttpClient를 가진다면? 
Service를 만들기 위해 어떤 Retrofit을 사용할지 구분해주어야 한다.

기존 Dagger에서는 @Named를 이용했다고 한다. (지금도 가능하다)
Hilt에서는 @Qualifier를 활용한다. [공식문서](https://developer.android.com/training/dependency-injection/hilt-android?hl=ko#multiple-bindings)

```kotlin
@Qualifier
@Retention(AnnotationRetention.BINARY)
annotation class AuthInterceptorOkHttpClient
```

hilt 자체가 런타임에 주입하는 것이 아니여서 그런지
Retention의 scope로 BINARY 외에 SOURCE, RUNTIME도 있지만 공식문서에도 BINARY 경우만 작성되어 있다.