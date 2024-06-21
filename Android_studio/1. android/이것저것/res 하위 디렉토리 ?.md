하위 디렉토리를 지원하지 않음. 만들어도 하위 폴더가 있으면 리소스 컴파일러가 실패함.

플러그인으로 가상 디렉토리 만들기 [#1](https://blog.naver.com/bjh7007/221487928476) [#2](https://antilog.tistory.com/43)
애초에 네이밍을 잘해두자 [stackoverflow](https://stackoverflow.com/questions/1077357/can-the-android-drawable-directory-contain-subdirectories)

figma에서 png 파일을 svg로 export해서 vector assets 통해 xml로 변환 했는데
`url(#pattern0_193_244)' is incompatible with attribute fillColor` 오류
=> png 파일이나 text 등 shape으로 지정되어 있지 않은 속성이 있으면 배경이 아닌 모든 컨텐츠 요소를 fillcolor에 넣기 위해 `단순 색상 값(#000000)`이 아닌 `url(#pattern0_193_244)`와 같은 값을 쓰는데 인지하지 못함.
figma에서 png 같은 파일이 아니라 여러 컴포넌트가 합쳐진 형태의 이미지로 애초에 만들어 주어야 함.