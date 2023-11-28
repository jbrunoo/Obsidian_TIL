
1. **Activity (액티비티)**
    - 앱의 사용자 인터페이스를 표시하고 관리하는 데 사용됩니다.
    - 사용자와 상호작용하면서 앱의 주된 화면이 됩니다.
    - 주로 하나의 액티비티가 하나의 화면을 나타냅니다.

2. **Service (서비스)**
    - 백그라운드에서 실행되는 컴포넌트로, UI 없이도 오래 걸릴 수 있는 작업을 수행할 수 있습니다.
    - 다른 구성 요소와 통신하거나, 장기 실행되는 작업을 처리하는 데 사용됩니다.

3. **Broadcast Receiver (브로드캐스트 수신자)**  
    - Android 시스템이나 다른 앱으로부터 방송되는 이벤트를 수신하고 처리하는 데 사용됩니다.
    - 예를 들면 배터리 상태 변화, 디바이스 부팅 완료 등의 이벤트를 감지할 수 있습니다.

4. **Content Provider (컨텐트 프로바이더)**
    - 데이터를 관리하고 다른 앱과 데이터를 공유하는 데 사용됩니다.
    - 데이터베이스나 파일 시스템과 같은 영속적인 데이터를 제공하고, 다른 앱이 이 데이터에 접근할 수 있도록 합니다.


이러한 app component들은 AndroidManifest.xml 파일에 선언되어 있으며, Android 시스템이 앱을 시작하고 관리할 때 상호작용합니다. 또한, 이러한 컴포넌트들은 서로를 인텐트(Intent) 등을 통해 호출하고 통신하여 전체 앱의 동작을 결정합니다. 개발자는 이러한 컴포넌트들을 조합하여 앱의 동작을 설계하고 구현합니다.