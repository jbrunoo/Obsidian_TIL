ktlint의 codeStyle이 official, intellij, android_studio로 나눠져 있긴한데 잘 안 맞는듯..

android 적용하기에 보편적인 이슈로는 
- import 정렬이 IDE에서 정렬이랑 차이남
- Composable 함수 시작 소문자로 뜸
- @Inject 등 어노테이션 개행 시켜줌
- underscore로 시작하는 backing field 변수명 에러 뜸
	수정되어 private으로 작성하고 public으로 underscore 제거한 같은 변수명을 정의하면 괜찮은데,
	(internal로 써도 안됨)
	지금 프로젝트에서 backing field들을 flow로 combine(\_xx, \_xx) 쓰고 있어서 에러 뜸

detekt가 나을지도(초기 작업이 복잡하지만 어짜피 ktlint도 설정하게되어서)

그리고 brew install ktlint(pinterest) 말고 dependa 추가하는 ktlint gradle plugin으로 사용하면
build gradle에서 ktlint version 등 추가 구성 하게됨

- - - 
## ex) .editorconfig
```plain
root = true  
  
[*.{kt,kts}]  
end_of_line = lf  
insert_final_newline = true  
max_line_length = 150  
  
ktlint_code_style = android_studio  
  
ktlint_standard_import-ordering = disabled  
ktlint_function_naming_ignore_when_annotated_with = Composable  
ktlint_standard_backing-property-naming = disabled  
ktlint_standard_class-signature = disabled  
ktlint_standard_function-signature = disabled  
ktlint_standard_function-literal = disabled  
  
ij_kotlin_allow_trailing_comma = true  
ij_kotlin_allow_trailing_comma_on_call_site = true
```

일단 적용해본 설정 파일