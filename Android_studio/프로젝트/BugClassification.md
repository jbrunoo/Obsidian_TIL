시간 남으면 ktor 서버 구현, custom 색상, 글꼴, 모양 등 적용해보기, firebase 연동

지금은 ui 이쁘게 꾸미고 tflite 모델 앱 내에서 돌리고 room db로 저장된 데이터 가져오기.

1일차
모듈화 중
ui 꾸미는 중

2일차 예정
navigation 스택 쌓이지 않게 관리하기
firebase 연동?
이미지 및 텍스트 데이터 저장

java.lang.IllegalStateException: Internal error: Unexpected failure when preparing tensor allocations: Regular TensorFlow ops are not supported by this interpreter. Make sure you apply/link the Flex delegate before inference. Node number 2 (FlexConv2D) failed to prepare.

오류 원인 :  [tensorflow ops](https://www.tensorflow.org/lite/guide/ops_select?hl=ko) 와 이러한 오류는 TensorFlow Lite 모델이 Flex delegate를 사용하지 않고 일반 TensorFlow 연산을 요구할 때 발생할 수 있습니다


3일차
서버 이용해서 띄움
1. cmd ipconfig의 ipv4 주소가 로컬 주소다.
2. app manifest - xml파일, 서버 주소, app 안에 api class 주소 다 같이 바뀌어야 함.


4일차
navigation stack관리 화면 쌓임. 바텀 app bar 통해서 navigate 되는 것은 어떻게 관리할지.
viewModel(savedStateHandle) 통해서 서버 값 받아서 정확도와 결과 값 다른 화면에 뿌려줌. 
값을 두 화면에서 쓰고 있어서 초기화되지 않음. 생명주기가 끝까지 감. 불필요해 보임.

5일차
db 어떻게 할지, local DB room 쓰려고 생각.
