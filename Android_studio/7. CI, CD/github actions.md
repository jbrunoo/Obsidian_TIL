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
