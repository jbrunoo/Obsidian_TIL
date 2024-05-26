#### 커서 이동
command + 좌/우 : 현재 줄 앞 뒤로 커서 이동(+shift 선택)
option + 좌/우 : 단어 단위로 이동(+shift 선택)
shift + enter : 다음 줄 생성 후 커서 이동
command + shift + 위/아래 : 현재 커서 줄 위/아래 변경

#### 코드 이동
command + B(or 클릭) : 코드 선언부 조회, 이동
command + shift + B : 코드 구현부 조회, 이동
command + option + L : 코드 정렬

#### 동시 변경
opt + 클릭 : 여러 줄 동시 변경
shift + F6 : 같은 단어 동시 변경 (m1에서 fn도 같이 눌러야 함) (윈도우처럼 ctrl + w 누르면 탭 닫힘)

#### 코드 보기
command + , : 설정 창
command + ; : 프로젝트 구조
control + tab(+shift 역방향) : 도구 창 전환

#### 찾기
cmd + F : 현재 파일 내 찾기
cmd + shift + F : 전체 경로에서 찾기
cmd + E : 최근 파일 열기
shift + shift : 전체 파일에서 검색


dock 숨긴 뒤 마우스 올리면 빠르게 뜸 (float뒤 값 수정하면 됨)
```shell
defaults write com.apple.dock autohide -bool true && defaults write com.apple.dock autohide-delay -float 0.01 && defaults write com.apple.dock autohide-time-modifier -float 0.01 && killall Dock
```
원래대로
```shell
defaults delete com.apple.dock autohide && defaults delete com.apple.dock autohide-delay && defaults delete com.apple.dock autohide-time-modifier && killall Dock
```
