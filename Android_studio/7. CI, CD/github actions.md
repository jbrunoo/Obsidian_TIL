repository 단위로 작업됨. (repository 내 event trigger 존재)

Event : Workflow - Job(s) - Runner(s) - Step(s)

job은 직/병렬 수행 가능
step은 순차적으로만 수행

settings - actions -secret 으로 gitignore 파일 및 value 입력 가능
settings - branches - protected 기능, build 성공 시만 merge 설정 가능 

- - -
## ex
```yaml
name: Android CI

on:
  pull_request:
    branches: [ "main" ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: set up JDK 17
        uses: actions/setup-java@v4
        with:
          java-version: '17'
          distribution: 'temurin'
          cache: gradle

      - name: Grant execute permission for gradlew
        run: chmod +x gradlew

      - name: Decode Keystore Base64 & Create Keystore Properties
        env:
          KEYSTORE_PROPERTIES: ${{ secrets.KEYSTORE_PROPERTIES }}
        run: |
          mkdir -p keystore
          echo "${{ secrets.KEYSTORE_BASE64 }}" | base64 --decode > keystore/keystore
          echo "$KEYSTORE_PROPERTIES" > keystore.properties

      - name: Create Local Properties
        env:
          LOCAL_PROPERTIES: ${{secrets.LOCAL_PROPERTIES}}
        run:
          echo "$LOCAL_PROPERTIES" > local.properties

      - name: Create Google Services JSON
        env:
          GOOGLE_SERVICES_JSON: ${{secrets.GOOGLE_SERVICES_JSON}}
        run:
          echo "$GOOGLE_SERVICES_JSON" > app/google-services.json

      - name: Build with Gradle
        run: ./gradlew build --no-daemon

      - name: Run ktlintCheck
        run: ./gradlew ktlintCheck --no-daemon
```

clean, --no-daemon은 CI 환경에서 필요없다는 의견이 현재는 많음
첫 빌드 타임은 같지만 추가적인 새 커밋이 올라오면 계속 새로 빌드될 수 있음

```
# 터미널에서 base 64 인코딩 (txt 파일로 읽을 수 있게)
openssl base64 -in [keystore file path]  -out [base64-file.txt]
```


- - -
###  detekt report 파일 sarif 형식 병합

```kotlin
// 현재 root gradle.kts 일부
... 

val detektReportMergeSarif by tasks.registering(ReportMergeTask::class) {  
    output.set(layout.buildDirectory.file("reports/detekt/merge.sarif"))  
}  

allprojects {  
    apply {  
        plugin(rootProject.libs.plugins.kotlin.detekt.get().pluginId)  
        plugin(rootProject.libs.plugins.kotlin.ktlint.get().pluginId)  
        plugin(rootProject.libs.plugins.gradle.dependency.handler.extensions.get().pluginId)  
    }  
  
    afterEvaluate {  
        extensions.configure<DetektExtension> {  
            parallel = true  
            buildUponDefaultConfig = true  
            toolVersion = libs.versions.kotlin.detekt.get()  
            config.setFrom(files("$rootDir/detekt-config.yml"))  
        }  
  
        extensions.configure<KtlintExtension> {  
            version.set(rootProject.libs.versions.kotlin.ktlint.source.get())  
            android.set(true)  
            verbose.set(true)  
        }  

		// https://ncorti.com/detekt/docs/introduction/changelog/#migration
        // Merge detekt report to 'root/build/reports/detekt/marge.sarif'  
        tasks.withType<Detekt>().configureEach {  
            reports {  
                sarif.required.set(true)  
            }  
            basePath = rootDir.absolutePath  
            finalizedBy(detektReportMergeSarif)  
        }  
  
        detektReportMergeSarif {  
            input.from(tasks.withType<Detekt>().map { it.sarifReportFile })  
        }  
    }} 
...
```


참고 
gradle 코드
https://github.com/detekt/detekt/blob/main/build.gradle.kts

sarif 파일 병합 및 올리기
detekt
https://detekt.dev/docs/introduction/reporting/#merging-reports
github 
https://docs.github.com/ko/code-security/code-scanning/integrating-with-code-scanning/uploading-a-sarif-file-to-github
