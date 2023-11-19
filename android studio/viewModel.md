
ViewModelFactory는 ViewModel을 단일 객체로 생성해 사용하기 위해(= 싱글톤 패턴이라나) 구현하는 클래스


navigation과 viewModel
복잡한 객체는 단일 정보 소스(예: 데이터 영역)에 데이터로 저장해야 합니다. 탐색 후 대상에 도달하면 전달된 ID를 사용하여 단일 정보 소스에서 필요한 정보를 로드할 수 있습니다. 데이터 영역 액세스를 담당하는 ViewModel의 인수를 검색하려면 `ViewModel’s` [`SavedStateHandle`](https://developer.android.com/topic/libraries/architecture/viewmodel-savedstate?hl=ko#savedstatehandle)을 사용하면 됩니다.

[viewModel 공식 문서](https://developer.android.com/topic/libraries/architecture/viewmodel?hl=ko#kotlin_1)

kotlin 식
```
val viewModel: DiceRollViewModel by viewModels()
```
java 식
```
DiceRollViewModel model = new ViewModelProvider(this).get(DiceRollViewModel.class);
```
compose 식
``` 
viewModel: DiceRollViewModel = viewModel()
```


