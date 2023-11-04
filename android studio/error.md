
```
Duplicate class org.intellij.lang.annotations.Identifier found in modules annotations-12.0 (com.intellij:annotations:12.0) and annotations-13.0 (org.jetbrains:annotations:13.0)
Duplicate class org.intellij.lang.annotations.JdkConstants found in modules annotations-12.0 (com.intellij:annotations:12.0) and annotations-13.0 (org.jetbrains:annotations:13.0)
Duplicate class org.intellij.lang.annotations.JdkConstants$AdjustableOrientation found in modules annotations-12.0 (com.intellij:annotations:12.0) and annotations-13.0 (org.jetbrains:annotations:13.0)
Duplicate class org.intellij.lang.annotations.JdkConstants$BoxLayoutAxis found in modules annotations-12.0 (com.intellij:annotations:12.0) and annotations-13.0 (org.jetbrains:annotations:13.0)
Duplicate class org.intellij.lang.annotations.JdkConstants$CalendarMonth found in modules annotations-12.0 (com.intellij:annotations:12.0) and annotations-13.0 (org.jetbrains:annotations:13.0)
Duplicate class org.intellij.lang.annotations.JdkConstants$CursorType found in modules annotations-12.0 (com.intellij:annotations:12.0) and annotations-13.0 (org.jetbrains:annotations:13.0)
Duplicate class org.intellij.lang.annotations.JdkConstants$FlowLayoutAlignment found in modules annotations-12.0 (com.intellij:annotations:12.0) and annotations-13.0 (org.jetbrains:annotations:13.0)
Duplicate class org.intellij.lang.annotations.JdkConstants$FontStyle found in modules annotations-12.0 (com.intellij:annotations:12.0) and annotations-13.0 (org.jetbrains:annotations:13.0)
Duplicate class org.intellij.lang.annotations.JdkConstants$HorizontalAlignment found in modules annotations-12.0 (com.intellij:annotations:12.0) and annotations-13.0 (org.jetbrains:annotations:13.0)
Duplicate class org.intellij.lang.annotations.JdkConstants$InputEventMask found in modules annotations-12.0 (com.intellij:annotations:12.0) and annotations-13.0 (org.jetbrains:annotations:13.0)
Duplicate class org.intellij.lang.annotations.JdkConstants$ListSelectionMode found in modules annotations-12.0 (com.intellij:annotations:12.0) and annotations-13.0 (org.jetbrains:annotations:13.0)
Duplicate class org.intellij.lang.annotations.JdkConstants$PatternFlags found in modules annotations-12.0 (com.intellij:annotations:12.0) and annotations-13.0 (org.jetbrains:annotations:13.0)
Duplicate class org.intellij.lang.annotations.JdkConstants$TabLayoutPolicy found in modules annotations-12.0 (com.intellij:annotations:12.0) and annotations-13.0 (org.jetbrains:annotations:13.0)
Duplicate class org.intellij.lang.annotations.JdkConstants$TabPlacement found in modules annotations-12.0 (com.intellij:annotations:12.0) and annotations-13.0 (org.jetbrains:annotations:13.0)
Duplicate class org.intellij.lang.annotations.JdkConstants$TitledBorderJustification found in modules annotations-12.0 (com.intellij:annotations:12.0) and annotations-13.0 (org.jetbrains:annotations:13.0)
Duplicate class org.intellij.lang.annotations.JdkConstants$TitledBorderTitlePosition found in modules annotations-12.0 (com.intellij:annotations:12.0) and annotations-13.0 (org.jetbrains:annotations:13.0)
Duplicate class org.intellij.lang.annotations.JdkConstants$TreeSelectionMode found in modules annotations-12.0 (com.intellij:annotations:12.0) and annotations-13.0 (org.jetbrains:annotations:13.0)
Duplicate class org.intellij.lang.annotations.Language found in modules annotations-12.0 (com.intellij:annotations:12.0) and annotations-13.0 (org.jetbrains:annotations:13.0)
Duplicate class org.intellij.lang.annotations.MagicConstant found in modules annotations-12.0 (com.intellij:annotations:12.0) and annotations-13.0 (org.jetbrains:annotations:13.0)
Duplicate class org.intellij.lang.annotations.Pattern found in modules annotations-12.0 (com.intellij:annotations:12.0) and annotations-13.0 (org.jetbrains:annotations:13.0)
Duplicate class org.intellij.lang.annotations.PrintFormat found in modules annotations-12.0 (com.intellij:annotations:12.0) and annotations-13.0 (org.jetbrains:annotations:13.0)
Duplicate class org.intellij.lang.annotations.PrintFormatPattern found in modules annotations-12.0 (com.intellij:annotations:12.0) and annotations-13.0 (org.jetbrains:annotations:13.0)
Duplicate class org.intellij.lang.annotations.RegExp found in modules annotations-12.0 (com.intellij:annotations:12.0) and annotations-13.0 (org.jetbrains:annotations:13.0)
Duplicate class org.intellij.lang.annotations.Subst found in modules annotations-12.0 (com.intellij:annotations:12.0) and annotations-13.0 (org.jetbrains:annotations:13.0)
Duplicate class org.jetbrains.annotations.Nls found in modules annotations-12.0 (com.intellij:annotations:12.0) and annotations-13.0 (org.jetbrains:annotations:13.0)
Duplicate class org.jetbrains.annotations.NonNls found in modules annotations-12.0 (com.intellij:annotations:12.0) and annotations-13.0 (org.jetbrains:annotations:13.0)
Duplicate class org.jetbrains.annotations.NotNull found in modules annotations-12.0 (com.intellij:annotations:12.0) and annotations-13.0 (org.jetbrains:annotations:13.0)
Duplicate class org.jetbrains.annotations.Nullable found in modules annotations-12.0 (com.intellij:annotations:12.0) and annotations-13.0 (org.jetbrains:annotations:13.0)
Duplicate class org.jetbrains.annotations.PropertyKey found in modules annotations-12.0 (com.intellij:annotations:12.0) and annotations-13.0 (org.jetbrains:annotations:13.0)
Duplicate class org.jetbrains.annotations.TestOnly found in modules annotations-12.0 (com.intellij:annotations:12.0) and annotations-13.0 (org.jetbrains:annotations:13.0)

Go to the documentation to learn how to Fix dependency resolution errors.
```

dependency 간 충돌이 문제인 듯. 특히, org 적힌 라이브러리들. 여러가지 방법 중인데 해결 안됨.
1. gradle.properties 안에 `android.enableJetifier=true` 추가해보기

	자세히 보면 충돌나는 모듈이 적혀있음.
- Duplicate class org.intellij.lang.annotations.Identifier found in modules jetified-annotations-12.0 (com.intellij:annotations:12.0) and jetified-annotations-13.0 (org.jetbrains:annotations:13.0)

2.  implementation("com.example:your-dependency:version") { exclude(group = "com.intellij", module = "annotations") } 추가하기 

3. **File -> 'Invalidate Caches and Restart' Click** 캐시 삭제 중.
