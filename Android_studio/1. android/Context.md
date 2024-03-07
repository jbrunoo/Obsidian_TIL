`context`는 안드로이드 앱에서 중요한 개념으로, 앱의 실행 환경에 대한 정보를 제공합니다. 안드로이드에서 `context`는 다양한 용도로 사용됩니다. 여기에는 리소스 액세스, 액티비티 및 서비스의 실행, 뷰 생성 등이 포함됩니다.

Jetpack Compose에서 `LocalContext.current`은 현재 컴포저블이나 컴포넌트가 실행되고 있는 컨텍스트(액티비티 또는 애플리케이션 컨텍스트)를 제공합니다. 이것은 Jetpack Compose 환경에서 일반적으로 사용되는 방법입니다.

`LocalContext.current.applicationContext`는 현재 컨텍스트의 애플리케이션 컨텍스트를 반환합니다. 이것은 안전한 컨텍스트를 사용하여 다양한 작업(예: 리소스 액세스, 데이터베이스 액세스 등)을 수행할 때 사용됩니다. 예를 들어, 리소스에 액세스할 때 사용할 수 있습니다.


Context는 애플리케이션 환경에 대해 전역적인 정보를 얻을 수 있는 추상 클래스
1. 애플리케이션의 현재 상태를 가짐.
2. activity나 애플리케이션 전역에 대한 정보를 얻을 때 사용.
3. 애플리케이션 리소스, local db, datastore 같은 로컬 저장소 접근할 때 사용.
4. activity, 애플리케이션 클래스는 기본적으로 Context 상속.


Context type
Application Context - 애플리케이션 전역, 프로세스에서 한 개만 존재. 
Activity Context - 액티비티 내에서 액티비티와 동일한 생명주기.
localContext.current - Compose 