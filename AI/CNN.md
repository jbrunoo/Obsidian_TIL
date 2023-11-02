Convolution Neural Network 합성곱 
음성 신호처리 등

이미지 선명 효과

필터 : edge, 잡음

```python
import cv2
import numpy as np

# 이미지를 읽기 위한 코드
image = cv2.imread("[Dataset]_Module22_images/pcb.png")

#  2D 컨볼루션을 실행하기 위해 회색조로 변환합니다.
gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)

kernel1 = np.ones((19,19),np.float32)/361      # 19x19 컨볼루션 필터
filter1 = cv2.filter2D(gray,-1,kernel1) # 가우시안 필터할 때도 이런 형식

kernel2 = np.array((                        # 3x3 컨볼루션 필터 # 외곽선 선명
    [0, -1, 0],
    [-1, 4, -1],
    [0, -1, 0]), np.float32)*2
filter2 = cv2.filter2D(gray,-1,kernel2)

cv2.imshow("Original Greyscale Image",gray)
cv2.imshow("Filter1",filter1)
cv2.imshow("Filter2",filter2)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

[CNN 모델 구축](https://colab.research.google.com/drive/1b29fKRX5J5h3AWtLUFobSX_iNJ146DVR?usp=sharing)
[CNN 튜토리얼](https://www.tensorflow.org/tutorials/images/cnn?hl=ko)
[tf.keras.layers.Conv2D](https://www.tensorflow.org/api_docs/python/tf/keras/layers/Conv2D)


Convolution Neural Network를 구성하고 있는 매개변수(Arguments)는
- filters부터 bias_contraint까지 총 16개로 구성되어 있음
- 중점적으로 봐야 하는 메소드 : filters, kernel_size, strides, padding,input_shape, activation 총 5가지
> **filters**는 출력되는 결과의 차원수 의미



![[Pasted image 20231011154704.png]]
output = input - filter + 1
6x6 이미지를 3x3 filter를 이용해 convolution 연산이 진행되고 결과값 보라색 4x4 이미

input_shape - 4차원 텐서 (4, 28, 28, 3)
batch size, height, width, channels

```python
input_shape = (4, 28, 28, 3)

x = tf.random.normal(input_shape)

y = tf.keras.layers.Conv2D(filters = 2, kernel_size = 3, activation = 'relu')(x)

print(y.shape)
# (4, 26, 26, 2)
```
filter는 channel에 영향을 줌(차원 수가 바뀜)
필터 마스크 커널 채널 다 혼합해서 씀.
kernal_size는 height, width에 영향을 줌 (사진 참고)
output = input - filter + 1 -> 28 - 3 + 1
strides - filter 이동이 1개씩 아니라 n개씩 가도록 설정하는 매개변수
```python
input_shape = (4, 28, 28, 3)

x = tf.random.normal(input_shape)

y = tf.keras.layers.Conv2D(filters = 2, kernel_size = 3, strides = 2, activation = 'relu')(x)

print(y.shape)
# (4, 13, 13, 2)
```
output = (input + filter) / stride + 1

padding은 edge 잘 볼 수 있게 신경쓰는 것. filter보면 중간 부분은 여러번 겹치지만 edge는 한번 계산됨. edge 외곽을 다 0으로 채워서 계산.
padding = (default 자동 적용) valid(적용x) same(적용o)

```python
input_shape = (4, 28, 28, 3)

x = tf.random.normal(input_shape)

y = tf.keras.layers.Conv2D(filters = 2, kernel_size = 3, activation = 'relu', padding = 'same')(x)

print(y.shape)
# (4, 28, 28, 2)
```

output = (input - filter + 2 * padding) / stride + 1

input_shape 처음 layer를 쌓을 때 설정해주는 값
activation - convolution 이후 활성화 함수를 고르는 매개변수


DNN이랑 비슷함 sequencial() 부분만 살짝 다름

DNN에서 dropout() 을 통해 과적합 방지 (연결 끊기)

pooling - 입력 데이터의 차원을 줄이고 계산량을 감소시키기 위해 사용, 풀링 층은 이미지나 특징 맵의 크기를 축소하는 역할

input image
convolution layers(convolution layer + pooling layer)
fully-connected layer(=Dense Layer)
output class


Model: "sequential_9"
_________________________________________________________________
 Layer (type)                Output Shape              Param #   
=================================================================
 conv2d_36 (Conv2D)          (None, 28, 28, 23)        230       
                                                                 
 batch_normalization_45 (Ba  (None, 28, 28, 23)        92        
 tchNormalization)                                               
                                                                 
 conv2d_37 (Conv2D)          (None, 28, 28, 32)        6656      
                                                                 
 batch_normalization_46 (Ba  (None, 28, 28, 32)        128       
 tchNormalization)                                               
                                                                 
 dropout_18 (Dropout)        (None, 28, 28, 32)        0         
                                                                 
 conv2d_38 (Conv2D)          (None, 28, 28, 64)        18496     
                                                                 
 batch_normalization_47 (Ba  (None, 28, 28, 64)        256       
 tchNormalization)                                               
                                                                 
 conv2d_39 (Conv2D)          (None, 28, 28, 64)        36928     
                                                                 
 batch_normalization_48 (Ba  (None, 28, 28, 64)        256       
 tchNormalization)                                               
                                                                 
 max_pooling2d_9 (MaxPoolin  (None, 14, 14, 64)        0         
 g2D)                                                            
                                                                 
 flatten_9 (Flatten)         (None, 12544)             0         
                                                                 
 dense_18 (Dense)            (None, 512)               6423040   
                                                                 
 batch_normalization_49 (Ba  (None, 512)               2048      
 tchNormalization)                                               
                                                                 
 dropout_19 (Dropout)        (None, 512)               0         
                                                                 
 dense_19 (Dense)            (None, 10)                5130      
                                                                 
=================================================================
Total params: 6493260 (24.77 MB)
Trainable params: 6491870 (24.76 MB)
Non-trainable params: 1390 (5.43 KB)
____________________________________________________________

```python
# Summary를 보고 모델 만들기
# 마지막 Dense는 softmax가 활성화함수이며 나머지 layer들은 relu입니다.
# 모든 CNN은 padding을 갖고 있습니다.
# 참고자료 : (https://keras.io/api/layers/normalization_layers/batch_normalization/)
# 참고자료 : (https://keras.io/api/layers/regularization_layers/dropout/)
from tensorflow import keras
from tensorflow.keras.layers import Conv2D, MaxPooling2D, Flatten, Dense, Dropout, BatchNormalization

num_classes = 10

model = keras.models.Sequential()

model.add(Conv2D(23, kernel_size=(3, 3), activation='relu', padding = 'same', input_shape=(28, 28, 1)))
model.add(BatchNormalization())

model.add(Conv2D(32, kernel_size=(3, 3), padding = 'same', activation='relu'))
model.add(BatchNormalization())
model.add(Dropout(0.25))

model.add(Conv2D(64, kernel_size=(3, 3), padding = 'same', activation='relu'))
model.add(BatchNormalization())

model.add(Conv2D(64, kernel_size=(3, 3), padding = 'same', activation='relu'))
model.add(BatchNormalization())

model.add(MaxPooling2D(pool_size=(2, 2)))
model.add(Flatten())

model.add(Dense(512, activation='relu'))
model.add(BatchNormalization())
model.add(Dropout(0.5))
  
model.add(Dense(10, activation='softmax'))
```

param을 보고 kernel_size 예측. 
230 -> 23(filter) * ((3 * 3 * 1) +1) /  6656 = 32 * ((3 * 3 * 23) +1)

padding = 'same'으로 28, 28 유지

과적합 방지 - Dropout, BatchNormalization


Intel OpenVINO 추론 엔진
OpenVINO toolit, cmake, vs 2019 설치
실습이 python 3.6에 나머지 프로그램도 이전 버전이라 env 환경 생성 anaconda prompt, navigator 이용
```
# 관리자 실행 시,
cd C:\Users\6pizz

>>>conda create -n ncs python=3.6

# To activate this environment, use
#
#     $ conda activate ncs
#
# To deactivate an active environment, use
#
#     $ conda deactivate
```
직접 activate ncs or anaconda navigator 이용해서 ncs로 channel 전환


squeezeNet 사용
```python
!python classification_sample.py -i [Dataset]_Module22_images/cat.jpeg -m models/squeezenet1.1/FP32/squeezenet1.1.xml --labels models/squeezenet1.1/FP32/squeezenet1.1.labels #한줄 코드임
```

```python
import cv2
import numpy as np
from utils.opv import OpvModel      # OpenVINO 2021.2 버젼 지원 클래스

mymodel = OpvModel("squeezenet1.1", device="CPU", fp="FP16", ncs=1, debug=True) 

predictions = mymodel.Predict(cv2.imread("[Dataset]_Module22_images/cat.jpeg"))

def displayresults(predictions, labels=None):
    if (labels is None):                         # 레이블이 제공되지 않는 경우 숫자 레이블을 사용합니다.
        labels = np.arange(0,1000).astype("str")
    predictions = predictions.reshape(1000)
    top5 = predictions.argsort()[-5:][::-1] # 예측이 오름차순이라 뒤에서 5개를 뒤집어서 보여줘라 
    top5results = np.column_stack([predictions[top5], labels[top5]])
    for (conf, label) in top5results:
        print(str(round(float(conf)*100,1))+"% chance: "+label)

displayresults(predictions,mymodel.labels)
```


##### 네트워크(모델)는 [1, 1, N, 7] 형태로 블롭을 출력하며, 여기서 N은 검출된 경계 상자의 수입니다. 각 탐지에 대해 설명은 [image_id, label, conf, x_min, y_min, x_max, y_max] 형식을 갖습니다.(7 개의 변수를 참조하십시오. 이는 [1,1,N,7]의 7을 설명합니다.)

```python
import cv2
import numpy as np
from utils.opv import OpvModel

mymodel2 = OpvModel("face-detection-adas-0001", device="CPU", fp="FP16", ncs=1) 
# NCS2를 사용하지 않는 경우 "MYRIAD"를 "CPU"로 대체하십시오.

friends = cv2.imread("[Dataset]_Module22_images/friends.jpeg")
friends = cv2.resize(friends,(1200,710))     # 원본이 상당히 크기 때문에 이미지 크기를 줄입니다.
predictions = mymodel2.Predict(friends)

print(predictions.shape)
print(predictions[:,:,0:3,:])#your code here

# 경계 상자 그리기
def DrawBoundingBoxes(predictions, image, conf=0): # conf 값을 조정하여 정확도 높은 값만 그리기 가능 0은 낮은 것도 다 찾아
    canvas = image.copy()                             # 원본 이미지를 수정하는 대신 복사합니다
    predictions_1 = predictions[0][0]                 # 하위 집합 데이터 프레임
    confidence = predictions_1[:,2]                   # conf 값 가져오기 [image_id, label, conf, x_min, y_min, x_max, y_max]
    topresults = predictions_1[(confidence>conf)]     # 임계값보다 큰 conf 값을 가진 예측만 선택
    (h,w) = canvas.shape[:2]                         
    for detection in topresults:
        box = detection[3:7] * np.array([w, h, w, h]) # 상자 위치 결정
        (xmin, ymin, xmax, ymax) = box.astype("int")  # xmin, ymin, xmax, ymax에 상자 위치 값 지정

        cv2.rectangle(canvas, (xmin, ymin), (xmax, ymax), (0, 0, 255), 4)   # 사각형 만들기
        cv2.putText(canvas, str(round(detection[2]*100,1))+"%", (xmin, ymin), # 텍스트 포함
            cv2.FONT_HERSHEY_SIMPLEX, 0.6, (255, 0,0), 2)
    cv2.putText(canvas, str(len(topresults))+" face(s) detected", (50,50),    # 텍스트 포함
            cv2.FONT_HERSHEY_SIMPLEX, 0.6, (255, 0,0), 2)
    return canvas

cv2.imshow("Friends2",DrawBoundingBoxes(predictions,friends))
cv2.waitKey(0)
cv2.destroyAllWindows()
```

```python
# 상자 색상, 두께, 글꼴까지 변경
def DrawBoundingBoxes(predictions, image, conf=0.5):
    canvas = image.copy()                              # 원본 이미지를 수정하는 대신 복사합니다
    predictions_1 = predictions[0][0]                  # 하위 집합 데이터 프레임
    confidence = predictions_1[:,2]                   # conf 값 가져오기 [image_id, label, conf, x_min, y_min, x_max, y_max]
    topresults = predictions_1[(confidence>conf)]     # 임계값보다 큰 conf 값을 가진 예측만 선택
    (h,w) = canvas.shape[:2]                          # 이미지 높이에 따라 변수 h 및 w 설정
    
    for detection in topresults:
        box = detection[3:7] * np.array([w, h, w, h]) # 상자 위치 결정
        (xmin, ymin, xmax, ymax) = box.astype("int")  # xmin, ymin, xmax, ymax에 상자 위치 값 지정

        cv2.rectangle(canvas, (xmin, ymin), (xmax, ymax), (100, 150, 255), 2)   # 사각형 만들기
        cv2.putText(canvas, str(round(detection[2]*100,1))+"%", (xmin, ymin), # 텍스트 포함
            cv2.FONT_HERSHEY_TRIPLEX, 0.8, (255, 230,40), 2)
    cv2.putText(canvas, str(len(topresults))+" friend(s) detected. It's time to party!", (50,50), # 텍스트 포함
            cv2.FONT_HERSHEY_COMPLEX, 0.6, (255, 10,110), 2)
    return predictions_1, canvas

predictions_1,x = DrawBoundingBoxes(predictions,friends)
cv2.imshow("Friends2",x)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

```python
# confidence 변수 출력
confidence = predictions_1[:, 2]

# [image_id, label, conf, x_min, y_min, x_max, y_max] 를 다시 식별
topresult = predictions_1[(confidence > 0.5)]
print(predictions_1[(confidence > 0.5)])

# detection[3:7]
detect = topresult[0]
detect[3:7]

# variable box
(h,w) = x.shape[:2]
box = detection[3:7] * np.array([w, h, w, h])
print(box)
```


```python
# 예측 수행 걸린 시간
import time
def wastetime():
    for i in range(0,10000000):
        testing = 1
        
# 시간차를 계산하기위한 샘플 코드
a = time.time()
wastetime()
b = time.time()
print(str(b-a) + " seconds")
```

```python
# NCS2를 해제하려면 해당 메모리를 해제해야합니다. MyModel이 여전히 정의되고, 주피터 노트북 세션이 종료되지 않았기 때문에 자동 [garbage collection](https://docs.python.org/3/library/gc.html) 이 OpvModel에서 발생하지 않습니다. 그리고 OpvModel은 이전에 로드한 모델을 보유하고 있는 NCS2에 대한 참조를 보유하고 있습니다.
mymodel.ClearMachine()
```


