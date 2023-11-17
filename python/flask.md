에러 
```
C:\Users\6pizz\PycharmProjects\flaskProject\mvenv\Scripts\python.exe -m flask run 
Fatal Python error: init_stdio_encoding: failed to get the Python codec name of the stdio encoding
Python runtime state: core initialized
LookupError: unknown encoding: x-windows-949

Current thread 0x00007200 (most recent call first):
  <no Python frame>
```

해결방법 : encoding 문제 , settings -> File Encodings에서 UTF-8로 맞추어줌.

![[Pasted image 20231114135939.png]]

위에 방식으로는 해결 안됨.

![[Pasted image 20231114140909.png]]

settings -> editor -> General -> Console의 Default Encoding을 UTF-8로 만들어 해결.
앞에 부분은 그대로 Default고 이 부분이 변경되야 해결되는 것 같음.


프레임 워크 공부 시 가장 먼저 알아야 할 것은 라우팅은 어떻게 하는지?
라우팅 : 어떤 요청을 어떤 함수가 응답할지 연결해주는 작업
그런 작업을 기술하는 것 라우터

라우터가 허용하는 methods (기본값 GET)


[openvino 예제 코드](https://da2so.tistory.com/64?category=1077352)

```python
import cv2  
import numpy as np  
from flask import Flask, request, jsonify  
from openvino import Core  
from scipy.special import softmax
  
app = Flask(__name__)  
  
model_xml = "model.xml"  
model_bin = "model.bin"  
ie = Core()  
net = ie.read_model(model=model_xml, weights=model_bin)  
exec_net = ie.compile_model(model=net, device_name="CPU")  
image = cv2.imread("ant8.jpeg")  
image = cv2.resize(image, (224, 224))  
  
# 모델에 전달할 입력 형태로 변환 (batch_size=1 추가)  
input_blob = image.transpose((2, 0, 1)).reshape(1, 3, 224, 224)  
  
@app.route('/')  
def model():  
    try:  
        # Get image from Android Studio  
        # image = request.files["image"].read()  
  
        output_layer = next(iter(exec_net.outputs))  
  
        result = exec_net([input_blob])[output_layer]  
  
        logits = result[0]  
        probabilities = softmax(logits, axis=0)  
  
        # 최대 확률 값과 해당 클래스 인덱스 찾기  
        max_prob = np.max(probabilities)  
        predicted_class = np.argmax(probabilities)  
  
        print("확률:", probabilities)  
        print("최대 확률:", max_prob)  
        print("예측된 클래스:", predicted_class)  
  
        return jsonify({"predicted_class": predicted_class, "confidence": float(max_prob)})  
  
        print(result)  
        return jsonify({"result": result})  
  
  
  
    except Exception as e:  
        return jsonify({"error": str(e)})  
  
  
if __name__ == '__main__':  
    app.run(debug=True)
```

일단 이미지를 서버에 넣어두고 결과 값을 어떻게 가져오는지 정확도를 뽑아올 수 있을지 보고 있음.