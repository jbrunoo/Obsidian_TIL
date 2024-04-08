# MVVM
[참고블로그](https://velog.io/@haero_kim/Android-%EA%B9%94%EC%8C%88%ED%95%98%EA%B2%8C-MVVM-%ED%8C%A8%ED%84%B4%EA%B3%BC-AAC-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0)  [참고 깃헙](https://github.com/mitchtabian/MVVMRecipeApp) [참고 유튜브](https://www.youtube.com/watch?v=LFlobqW8Sy8)

![[Pasted image 20231030140317.png]]

view 
- activity, fragment 역할
- 사용자의 action을 받음(text, button etc)
- viewModel의 데이터를 관찰하여 UI 갱신

viewModel
- view가 요청한 데이터를 Model로 요청함
- Model 로부터 요청한 데이터를 받음

Model
- viewModel이 요청한 데이터를 반환함
- Room, Realm과 같은 DB 사용, Retrofit을 통한 백엔드 API 호출(네트워킹)이 보편적

view가 필요한 데이터는 viewModel이 쥐고있음.
그래서 view는 viewModel이 쥐고 있는 데이터를 관찰(observing)
MVC 패턴과 다름. view가 DB에 직접 접근 x, UI 업데이트에만 집중
관찰하고 있기에 데이터 변화에 더욱 능동적으로 움직임.

view가 viewModel Data를 관찰하기에 UI 업데이트 간편
viewModel이 데이터를 홀드해서 memory leak 가능성 배제
(view가 직접 model에 접근하지 않아 activity, fragment lifecycle에 의존하지 않으므로)
기능별 모듈화 - 유지보수 용이(viewModel 재사용, DB 교체 등)

MVVM 패턴을 간편하게 적용하기 위해 구글에서 AAC 제공(잘못 얻은 정보라는 것을 [[viewModel]] 공부하면서 배움.)
(Android Architecture Component)

![[Pasted image 20231031091557.png]]

Observer
viewModel
- 화면 변화 시(가로화면 등)에도 사라지지 않는 데이터를 가짐.
Live Data
- view가 viewModel을 관찰할 때, 관찰 대상이 되는 데이터 홀더 클래스. activity & fragment의 lifecycle을 인지하지 못하므로, 화면이 활성화될 때만 동작하여 memory leak을 줄여줌.
Repository
- viewModel과 데이터를 주고받기 위해 데이터 API를 포함하는 클래스. 사용자 동작에 따라 외부 백엔드 서버 등에서 데이터 가져옴. viewModel이 데이터를 관리할 필요x
RoomDatabase
- local DB


#### DAO vs Repository [#1](https://stackoverflow.com/questions/8550124/what-is-the-difference-between-dao-and-repository-patterns) [#2](https://stackoverflow.com/questions/71400416/why-do-we-need-to-use-dao-and-repository-in-the-same-time-on-android-project)
	DAO와 Repository 둘 다 interface로 추상화하는데 역할이나 코드가 거의 같음.
	왜 Repository 패턴을 사용하는지?
	1. 의미론적 구분(database와 repository의 역할을 구분하기 위해)
	2. Database가 여러 개인 경우 많은 DAO를 생성해야하므로 코드 중복 및 오류 발생 가능성

