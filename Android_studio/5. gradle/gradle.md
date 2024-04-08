안드로이드 용 자동화 빌드 도구
groovy에서 gradle로 바꼈다고만 배움

[medium](https://medium.com/@renovatio0424/%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C-%EA%B0%9C%EB%B0%9C%EC%9E%90%EB%9D%BC%EB%A9%B4-%EC%95%8C%EC%95%84%EC%95%BC-%ED%95%98%EB%8A%94-gradle-%EC%9B%90%EB%A6%AC-c39a3299ba6d)

gradle의 빌드 프로세스는 어떻게 되는가?

빌드는 코드가 적힌 파일을 컴파일하여 실행 가능한 실행 파일로 만드는 과정
윈도우에서 .exe를 만들 듯, 안드로이드에서 .apk 파일을 만드는 과정.
ps. apk파일은 실행 파일이 아닌 설치 파일

gradle은 setting.gradle과 build.gradle 파일을 통해 원하는 소프트웨어를 빌드할 수 있음
setting.gradle은 프로젝트의 구성 정보를 기록하는 파일. 어떠 하위 프로젝트들이 어떤 관계로 구성되어있는지 기술
build.gradle은 의존성, 플러그인을 설정하는 파일. 빌드 작업에 필요한 기본 설정, 동작 등 기술



gradle은 task를 기반으로 빌드를 진행
task는 build.gradle에 정의된 가장 작은 작업 단위
task라는 개념은 빌드 속도 향상을 위해 도입되었음

task에 종속성을 연결했을 때 특정 task에만 변경점이 생겼다면 종속성이 없는 task까지 빌드하지 않아도 되기 떄문 => task dependency mechanism


실제 gradle 빌드 단계
1. 초기화 (Initialiation)
	프로젝트 인스턴스를 만듦 (프로젝트 내의 모듈들의 빌드를 준비하는 과정)
2. 구성 (Configuration)
	
3. 실행 (Execution)
