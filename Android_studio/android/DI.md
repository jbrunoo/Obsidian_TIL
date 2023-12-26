[유튜브 참고](https://www.youtube.com/watch?v=wZn-zpwvxCU)
[의존성 주입 공식문서](https://developer.android.com/training/dependency-injection?hl=ko)
[Hilt 문서](https://developer.android.com/training/dependency-injection/hilt-android?hl=ko)
# 의존성 주입

의존 - A클래스 안에서 B클래스 객체를 생성해서 사용한다 그럼 A는 B에 의존
ex) 영상 초반 코드는 viewModel이 firebaseRepository 객체를 사용함.

의존성 - A클래스가 B클래스가 아닌 다른 C클래스를 참고해야한다면? A클래스 안에서도 수정 해야함.  ex) 예제는 firebaseRepository가 아닌 remoteRepository라는 C클래스를 참고하려고 함.

의존 관계 해결방안?
	interface를 이용해 같은 모양의 메서드를 구현하도록 강제할 수 있음. 또한, 다형성을 통해 안전하고 편하게 수정할 수 있음.
	ex) Repository interface를 만들어 사용.
	또한 기존 코드는 A클래스(viewModel 클래스) 안에서 객체를 직접 생성했는데 생성자에서 repository를 받도록 수정하면 직접 viewModel 수정 안해도 됨. 


기존코드
```kotlin
class ViewModel {
	private val firebaseRepository = firebaseRepository()

	private fun loadLanguages() {
		firebaseRepository.getLanguages({ list -> println(list.toString()) })
	}
}

class FirebaseRepository : Repository {
	override fun getLanguages(callback: (List<string>) -> Unit) {
		// firebase에서 언어 목록을 가져온 뒤 callback 실행
	}
}
```

의존성 해결 이런 느낌.
```kotlin
// // 각기 다른 클래스
fun main() {
	var viewModel = ViewModel(FirebaseRepository())
	viewModel = ViewModel(RemoteRepository()) // viewModel 고치지 않고 변경
}

class ViewModel constructor(private val repository: Repository) { // 생성자로
	private fun loadLanguage(){
		repository.getLanguages({ list -> println(list.toString()) })
	}
}

interface Repository { // interface로
	fun getLangugage(callback: (List<string>) -> Unit)
}

class FirebaseRepository : Repository {
	override fun getLanguages(callback: (List<string>) -> Unit) {
		// firebase에서 언어 목록을 가져온 뒤 callback 실행
	}
}

class RemoteRepository : Repository {
	override fun getLanguages(callback : (List<string>) -> Unit) {
		// REST API 호출을 통해 언어 목록을 받아온 뒤 callback을 실행
	}
}
```


SOLID 원칙 중 D에 해당하는 의존 역전 원칙.
플러그인 형태로 쉽게 다른 객체로 갈아 끼울 수 있게 됨.

- 변동성이 큰 클래스를 참조하는 것을 지양하자.
- 비교적 안정적인 인터페이스를 참조하자.

## 의존성 주입 프레임워크(Hilt)가 필요한 이유

```kotlin
fun main() {
	var viewModel = ViewModel(FirebaseRepository())
	viewModel = ViewModel(RemoteRepository())
}
```

이 코드에서는 main 함수에서 viewModel을 생성할 때, Repository 구현체를 건네 주게 됨.
그때 그때 상황에 맞게 달라지므로 귀찮고 오류를 발생할 수 있음.
또한, 프레임워크를 사용하는 경우, 내 마음대로 객체를 넘겨주지 못하는 코드를 짤 수 있음.
=> 필요 이유

## Dagger

java에서 사용할 수 있는 의존성 주입 라이브러리.
장점 : compile time에 주입 -> 런타임 성능, 컴파일 에러(컴파일 타임에 에러 알 수 있음)
단점 : 학습 매우 어려움.



## Hilt

Dagger 바탕으로  발전. dagger 뜻 : 단검. hilt 뜻 : 칼자루. 
즉 dagger를 더 편하고 안전하게 사용하기 위해 탄생.

안드로이드에서 dagger를 사용하기 위해 android component 생명주기에 맞춰 객체를 주입하는 코드를 개발자가 작성해야 함.
Hilt는 이 작업을 대신 수행.

### Dagger & Hilt 기본 개념

Inject - 주입 요청하고 객체 생성 방법 Hilt에게 안내.
Module - 주입할 객체 생성하여 component에게 전달.
Component - Module이 생성한 객체를 Inject에 주입.(android component와 다름. container로 부르기도 함)

# Hilt 사용법

영상 참고 from 8:22 글로만 적음.
1. 반드시 @HiltAndroidApp annotation이 지정된 Application 클래스가 있어야 함.
	Hilt가 안드로이드 생명주기에 맞춰 동작하게 되므로 Application 객체가 필요.
2. 안드로이드 component에 주입을 요청.
3. MainViewModel 객체 생성 방법을 알려주고 RemoteRepository 주입을 요청.
4. RemoteRepository 객체 생성 방법을 알려줌.

생성자 앞에 `@Inject`를 달아줌으로써 객체 생성 방법을 알려줄 수 있음.
activity나 fragment에서 viewModel을 주입받을 때, activity ktx, fragment ktx 기능으로 (?)

사용 방법 따로 익히기.



# dagger hilt - compose에서 적용

[유튜브](https://www.youtube.com/watch?v=bbMsuI2p1DQ)

![[Pasted image 20231123123625.png]]

  
의존성 주입(Dependency Injection)은 소프트웨어 디자인 패턴 중 하나로, 클래스가 직접 자신의 의존성을 생성하지 않고 외부에서 주입받는 것을 말합니다. 안드로이드에서는 Hilt와 같은 의존성 주입 프레임워크를 사용하여 이를 구현할 수 있습니다.

여기서 "의존성"이란 다른 클래스나 모듈에 대한 의존 관계를 의미합니다. 예를 들어, 클래스 A가 클래스 B에 의존한다면, A는 B의 기능이나 객체를 사용하거나 확장하는 것입니다.

의존성 주입은 주로 세 가지 형태로 나타납니다:

1. **생성자 주입(Constructor Injection):** 클래스의 생성자를 통해 의존성을 주입하는 방식입니다. 클래스가 인스턴스화될 때 외부에서 필요한 의존성을 전달받습니다. Hilt에서는 `@Inject` 어노테이션을 사용하여 생성자에 주입할 의존성을 표시합니다.
    
    javaCopy code
    
    `// 예시: Hilt를 사용한 생성자 주입 class MyClass @Inject constructor(private val dependency: Dependency) {     // ... }`
    
2. **메소드 주입(Method Injection):** 클래스의 메소드를 통해 의존성을 주입하는 방식입니다. 특정 메소드에 의존성을 전달받아 사용합니다.
    
    javaCopy code
    
    `// 예시: Hilt를 사용한 메소드 주입 class MyClass {     @Inject     fun setDependency(dependency: Dependency) {         // ...     } }`
    
3. **필드 주입(Field Injection):** 클래스의 필드를 통해 의존성을 주입하는 방식입니다. 클래스의 필드에 `@Inject` 어노테이션을 사용하여 주입할 의존성을 표시합니다.
    
    javaCopy code
    
    `// 예시: Hilt를 사용한 필드 주입 class MyClass {     @Inject     private lateinit var dependency: Dependency }`
    

Hilt를 사용하면 앱의 구성 요소에 대한 의존성을 자동으로 주입할 수 있습니다. 예를 들어, Android에서는 액티비티, 프래그먼트, 서비스 등의 구성 요소에 대한 의존성을 주입할 수 있습니다.

이러한 의존성 주입은 코드의 유지보수성을 향상시키고 테스트 가능한 코드를 작성하는 데 도움이 됩니다. 또한 객체 간의 결합도를 낮춰 시스템을 확장하거나 변경하기 쉽게 만듭니다.


