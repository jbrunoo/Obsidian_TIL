1일차
모듈화 중
ui 꾸미는 중

2일차 예정
navigation 스택 쌓이지 않게 관리하기
firebase 연동?
카카오 & 구글 로그인 구현
이미지 및 텍스트 데이터 저장

java.lang.IllegalStateException: Internal error: Unexpected failure when preparing tensor allocations: Regular TensorFlow ops are not supported by this interpreter. Make sure you apply/link the Flex delegate before inference. Node number 2 (FlexConv2D) failed to prepare.

오류 원인 :  [tensorflow ops](https://www.tensorflow.org/lite/guide/ops_select?hl=ko) 와 이러한 오류는 TensorFlow Lite 모델이 Flex delegate를 사용하지 않고 일반 TensorFlow 연산을 요구할 때 발생할 수 있습니다



3일차 
ktor?
서버 구현 해보기 
서버에서 모델 돌리기