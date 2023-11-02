

### 데이터 가져오기
### 학습 용 데이터/ 레이블 데이터 만들기
### 특징 값 찾기
### 특징 값을 사용하여 KNN 알고리즘으로 예측
### 평가하기 (Classification Report 사용)
### 결과 해석하기


```python
import cv2
import numpy as np
import warnings
warnings.filterwarnings('ignore')

# 이미지를 메모리로 읽어 보겠습니다.
red_card = cv2.imread("[Dataset] Module 21 images/cardred_close.png")
green_card = cv2.imread("[Dataset] Module 21 images/cardgreen_close.png")
black_card = cv2.imread("[Dataset] Module 21 images/cardblack_close.png")
background = cv2.imread("[Dataset] Module 21 images/cardnone.png")

# 특성을 추출하는 함수 정의(평균 색상)
def averagecolor(image):
    return np.mean(image, axis=(0, 1))

print (averagecolor(red_card))
print (averagecolor(green_card))
print (red_card.shape)
print (green_card.shape)

# 분류를 위해 특성(평균 색상) 및 해당 레이블(빨간색/녹색/검정색/없음) 저장
trainX = []
trainY = []

# 카드들에 대해 반복하고 평균 색상 출력
labels = ("red", "green", "black", "none")
for (card,label) in zip((red_card,green_card,black_card,background), labels):
    print((label, averagecolor(card)))
    trainX.append(averagecolor(card))
    trainY.append(label)

new_card = cv2.imread("[Dataset] Module 21 images/test/16.png")
new_card_features = averagecolor(new_card)

calculated_distances = []
for card in (trainX):
    calculated_distances.append(np.linalg.norm(new_card_features - card)) # 유클리드 거리
    
print (calculated_distances)
print(trainY[np.argmin(calculated_distances)])
print(calculated_distances)
print(np.argmin(calculated_distances)) # 리스트 가장 작은 값 인덱스 반환, new_card_features와 가장 가까운 인덱스
print(trainY)
print(trainY[np.argmin(calculated_distances)])

# 먼저 새로운 이미지를 메모리로 읽습니다.
new_card = cv2.imread("[Dataset] Module 21 images/test/36.png")
new_card_features = averagecolor(new_card)

# 새 이미지의 특성(평균 색상)과 알려진 이미지 특성 사이의 거리를 계산합니다.
calculated_distances = []
for card in (trainX):
    calculated_distances.append(np.linalg.norm(new_card_features-card))

# 다음은 가장 유사한 카드의 결과입니다:
print(trainY[np.argmin(calculated_distances)])

# 먼저 새로운 이미지를 메모리로 읽습니다.
new_card = cv2.imread("[Dataset] Module 21 images/test/56.png")
new_card_features = averagecolor(new_card)

# 새 이미지의 특성(평균 색상)과 알려진 이미지 특성 사이의 거리를 계산합니다.
calculated_distances = []
for card in (trainX):
    calculated_distances.append(np.linalg.norm(new_card_features-card))

# 다음은 가장 유사한 카드의 결과입니다:
print(trainY[np.argmin(calculated_distances)])

from sklearn.metrics import classification_report
# 테스트 이미지에 대한 진리표입니다. 이미지를 보려면 컴퓨터의 폴더를 여십시오.
realtestY = np.array(["black","black","black","black","black",
                     "red","red","red","red","red",
                     "green","green","green","green","green",
                     "none","none","none","none","none"])

def evaluateaccuracy(filenames,predictedY):
    predictedY = np.array(predictedY)
    if (np.sum(realtestY!=predictedY)>0):
        print ("Wrong Predictions: (filename, labelled, predicted) ")
        print (np.dstack([filenames,realtestY,predictedY]).squeeze()[(realtestY!=predictedY)])
    # 전체 예측의 백분율로 일치하는 (정확한) 예측을 계산합니다.
    return "Correct :"+ str(np.sum(realtestY==predictedY)) + ". Wrong: "+str(np.sum(realtestY!=predictedY)) + ". Correctly Classified: " + str(np.sum(realtestY==predictedY)*100/len(predictedY))+"%"

import os
path = "[Dataset] Module 21 images/test/"
predictedY = []
filenames = []
for filename in os.listdir(path):
    img = cv2.imread(path+filename)
    img_features = averagecolor(img)
    calculated_distances = []
    for card in (trainX):
        calculated_distances.append(np.linalg.norm(img_features-card))
    prediction = trainY[np.argmin(calculated_distances)]
    
    print (filename + ": " + prediction) # 추론을 출력합니다.
    filenames.append(filename)
    predictedY.append(prediction)

# 정확도 평가(sklearn 패키지는 유용한 보고서를 제공합니다)
print ()
print(classification_report(realtestY, predictedY))
# 정확도 평가(잘못 분류된 항목의 파일 이름을 출력하기 위한 자체 사용자 정의 메소드)
print ()
print (evaluateaccuracy(filenames,predictedY))
```
균등할 때 정확도, 불균등 조화 평균


np.dstack, hstack(horizontal), vstack(vertical)
ex) a[1, 2, 3], b[4, 5, 6] 
dstack은 3차원



svm 이용해보기
svm support vector machine
support vector 간의 거리가 멀수록 신규 데이터에 대한 확장성이 좋음. 거리 값은 마진
svm은 거리 기반이라 label은 항상 숫자 값이 들어가야 함.


