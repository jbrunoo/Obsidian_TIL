[코드랩](https://developer.android.com/codelabs/basic-android-kotlin-compose-classes-and-objects?hl=ko&continue=https%3A%2F%2Fdeveloper.android.com%2Fcourses%2Fpathways%2Fandroid-basics-compose-unit-2-pathway-1%3Fhl%3Dko%23codelab-https%3A%2F%2Fdeveloper.android.com%2Fcodelabs%2Fbasic-android-kotlin-compose-classes-and-objects#4) [github 참고](https://github.com/jyheo/kotlin-tutorial/blob/master/kotlin-OOP.md)

클래스 이름은 PascalCase(대문자 시작, 공백 X)

세 가지 구성
- 속성 : 클래스 객체의 속성을 지정하는 변수.
- 메서드 : 클래스의 동작과 작업이 포함된 함수.
- 생성자 : 클래스가 정의된 프로그램 전체에서 클래스의 인스턴스를 만드는 특수 멤버 함수.

코틀린에서 class는 기본적으로 final이므로 상속을 허용하기 위해 open 키워드 필요. ex) BaseViewModel
오버라이딩 허용도 마찬가지 open 키워드.