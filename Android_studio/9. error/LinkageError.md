이 에러는 우테코 프리코스 도중 발생한 에러인데, JDK 관련 에러이다.
3주차 미션 중 발생했는데 그 이유는 [[Gradle's dependency cache may be corrupt]] 이거 작성하면서
JDK 버전을 21에서 17로 낮추었기 때문이었다. 물론, 저것도 JDK와 관련 없었는데 낮춘 것..
생각해보면 특별 케이스에 맞추지 않는 이상 높아서 문제가 되는 경우가 이상하지 않은가 ..

하여튼 JDK 버전을 이미 변경해보아서 IDE와 환경변수를 편집해주었으나 해결되지 않았다.
그 이유는 IDE 상단 Configuration 내용의 edit를 눌러보면 여전히 JDK가 이전 버전으로 설정되어 있었다.
이 부분을 직접 바꿔서 실행하여도 되지만 다른 파일이나 새로운 Configuration도 계속 이전 버전으로 생성되기 때문에 불편하다.

IDE 상에서 설정해주었다고 생각한 부분은 `cmd + ,` 로 들어가는 세팅창에서 
`build tools - gradle - gradle JVM`을 설정한 것이 였는데 
`cmd + ;` 또는 `file - project structure`에 있는 SDK를 21로 설정해주어야 프로젝트 전반에 적용된다.

결론은 SDK, gradle JVM에 JDK 버전을 맞춰주자.