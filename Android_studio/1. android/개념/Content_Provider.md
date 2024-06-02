[youtube](https://www.youtube.com/watch?v=IVHZpTyVOxU)
[blog](https://velog.io/@cksgodl/AndroidCompose-%EC%BB%B4%ED%8F%AC%EC%A6%88%EC%97%90%EC%84%9C-%ED%8E%98%EC%9D%B4%EC%A7%95-%EA%B0%80%EB%8A%A5%ED%95%9C-%EC%BB%A4%EC%8A%A4%ED%85%80-%EA%B0%A4%EB%9F%AC%EB%A6%AC-%EB%A7%8C%EB%93%A4%EA%B8%B0)

안드로이드에서 ContentProvider와 ContentResolver는 데이터 공유 및 데이터 액세스를 관리하는 데 사용되는 두 가지 주요 컴포넌트입니다. 이 둘은 서로 다른 역할을 수행하며 데이터 공유 및 액세스를 위해 함께 작동합니다.

1. **ContentProvider:**
    
    - **목적:** 데이터 공유 및 액세스의 중앙 관리자로 작동합니다.
    - **구성 요소:** ContentProvider는 데이터베이스, 파일 또는 기타 데이터 소스에 대한 추상화 계층을 제공합니다.
    - **역할:** 다른 애플리케이션에서 데이터를 공유하려면 ContentProvider를 사용하여 데이터를 제공하고, ContentProvider를 통해 다른 애플리케이션에서 데이터를 쿼리하고 업데이트할 수 있습니다.
    - **접근 권한:** ContentProvider는 데이터에 대한 액세스 권한을 정의하고, 다른 애플리케이션이 해당 권한이 있는 경우에만 데이터에 액세스할 수 있도록 합니다.
2. **ContentResolver:**
    
    - **목적:** ContentProvider에 대한 데이터 액세스를 중개합니다. 다른 애플리케이션에서 ContentProvider에 액세스하는 데 사용됩니다.
    - **구성 요소:** ContentResolver는 컨텐트 프로바이더와 통신하는 데 사용되는 인터페이스입니다.
    - **역할:** ContentResolver는 데이터베이스나 기타 데이터 소스에 직접 액세스하지 않고, 대신 ContentProvider를 통해 데이터에 액세스합니다. ContentResolver는 다른 애플리케이션의 ContentProvider에 쿼리를 보내고, 결과를 받아오며, 필요한 경우 데이터를 업데이트할 수 있습니다.

간단히 말하면, ContentProvider는 데이터를 관리하고 외부에서의 액세스를 제어하며, ContentResolver는 다른 애플리케이션에서 ContentProvider로의 데이터 액세스를 중개합니다.



