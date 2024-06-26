hilt 등의 라이브러리 없이 수동으로 구현 방법
[공식문서 - 수동 삽입](https://developer.android.com/training/dependency-injection/manual?hl=ko#kotlin)
DI없이 수동 구성한다면 상용구 코드가 늘어남, 종속 항목 순서대로 선언, 객체 재사용 어려움, 싱글톤 패턴 사용시 테스트 더 어려움
app container class 만들어서 Application class에 배치하여 사용해야 함.



- - -
 
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

## 의존성 주입 프레임워크가 필요한 이유

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

