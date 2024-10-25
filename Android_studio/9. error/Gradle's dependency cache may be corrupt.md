사이드 프로젝트 레포 clone 후 빌드 시 발생하였다.

구체적인 내용은
```
* What went wrong:
An exception occurred applying plugin request [id: 'com.captures2024.soongan.application']
> Failed to apply plugin 'com.captures2024.soongan.application'.
   > null cannot be cast to non-null type kotlin.String
```

단순 빌드이기 때문에 gitigonore 파일과는 상관 없어 보이고 gradle 버전 문제일 가능성이 제일 높다.
gradle-wrapper.properties 파일을 보면
`distributionUrl=https\://services.gradle.org/distributions/gradle-8.6-bin.zip` 을 사용중.
저기서 변경도 되지만 command + ; 누르면 쉽게 접근 가능.

![[Pasted image 20241025213054.png]]
$versions.agp : 8.4.2로 되어 있다. 
공식문서에 따라 업그레이드를 해주려면 tools - AGP Upgrade Assistant...
빌드가 되지 않은 프로젝트에서는 창이 열리지 않는다..

사람들의 경우 jdk 버전 문제던데 `null cannot be cast to non-null type kotlin.String` 이런 에러가..

`./gradlew clean build --refresh-dependencies` 했을 시 에러 메시지
```
* What went wrong:
Unable to start the daemon process.
This problem might be caused by incorrect configuration of the daemon.
For example, an unrecognized jvm option is used.For more details on the daemon, please refer to https://docs.gradle.org/8.6/userguide/gradle_daemon.html in the Gradle documentation.

Process command line: /Users/jbrunoo/Library/Java/JavaVirtualMachines/openjdk-21.0.2/Contents/Home/bin/java -XX:MaxPermSize=512m -XX:+HeapDumpOnOutOfMemoryError --add-opens=java.base/java.util=ALL-UNNAMED --add-opens=java.base/java.lang=ALL-UNNAMED --add-opens=java.base/java.lang.invoke=ALL-UNNAMED --add-opens=java.prefs/java.util.prefs=ALL-UNNAMED --add-opens=java.base/java.nio.charset=ALL-UNNAMED --add-opens=java.base/java.net=ALL-UNNAMED --add-opens=java.base/java.util.concurrent.atomic=ALL-UNNAMED -Xmx4096m -Dfile.encoding=UTF-8 -Duser.country=KR -Duser.language=ko -Duser.variant -cp /Users/jbrunoo/.gradle/wrapper/dists/gradle-8.6-bin/afr5mpiioh2wthjmwnkmdsd5w/gradle-8.6/lib/gradle-launcher-8.6.jar -javaagent:/Users/jbrunoo/.gradle/wrapper/dists/gradle-8.6-bin/afr5mpiioh2wthjmwnkmdsd5w/gradle-8.6/lib/agents/gradle-instrumentation-agent-8.6.jar org.gradle.launcher.daemon.bootstrap.GradleDaemon 8.6

```

클린 빌드를 수행하자 `/Users/jbrunoo/Library/Java/JavaVirtualMachines/openjdk-21.0.2/Contents/Home/bin/java ` 메시지가 보이기 시작했다.
클론한 프로젝트는 JAVA_17 환경인데 21을 사용했기 때문에 그 이상이라 문제가 없을 줄 알았다.
어쨌든 충돌나는 부분이 있겠거니 17로 변경 중..

setting - gradle 에서 17로 내리는 것은 동작하지 않았다.
설정을 만진 기억은 없지만 아마 설치되어있는 jdk 버전 적용하는 우선순위가 존재하는 것 같다.

그래서 시스템 환경변수 편집 중..
```
/usr/libexec/java_home -V   // 설치된 jdk 버전 확인 가능
java -version   // 현재 자바 버전
```

mac os는 shell에 맞게끔 환경변수를 편집해주어야 한다. 
zsh를 이용중인데 bash로 업데이트 해서 적용이 잘안됐음. [참고 블로그](https://velog.io/@may_yun/Mac-M1-Java-%EB%B2%84%EC%A0%84-%EB%B3%80%EA%B2%BD)

그리고 결론은 jdk 버전과 상관없이 해결되지 않았음.