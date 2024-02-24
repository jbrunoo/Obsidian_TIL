```python
from selenium import webdriver  
from selenium.webdriver.common.by import By  
from selenium.webdriver.common.keys import Keys  
from selenium.webdriver.chrome.service import Service as ChromeService  
from sklearn.metrics import accuracy_score, classification_report  
from sklearn.model_selection import train_test_split  
from sklearn.neighbors import KNeighborsClassifier  
from sklearn.svm import SVC  
from webdriver_manager.chrome import ChromeDriverManager  
import time  
import os  
import urllib.request  
import cv2  
import numpy as np  
  
# 크롤링 selenium# 세 가지 키워드 설정  
'''  
keywords = ["banana_ripe", "banana_unripe", "banana overripe"]  
  
# 크롬 드라이버 생성  
driver = webdriver.Chrome(service=ChromeService(ChromeDriverManager().install()))  
  
for keyword in keywords:  
    # 검색어에 따른 폴더 생성  
    folder_name = keyword.replace(" ", "_")    os.makedirs(folder_name, exist_ok=True)  
    driver.get("https://www.google.co.kr/imghp?hl=ko&ogbl")    elem = driver.find_element(By.NAME, "q")    elem.send_keys(keyword)    elem.send_keys(Keys.RETURN)  
    SCROLL_PAUSE_TIME = 1    last_height = driver.execute_script("return document.body.scrollHeight")  
    while True:        driver.execute_script("window.scrollTo(0, document.body.scrollHeight);")        time.sleep(SCROLL_PAUSE_TIME)        new_height = driver.execute_script("return document.body.scrollHeight")        if new_height == last_height:            try:                driver.find_element(By.CSS_SELECTOR, ".mye4qd").click()            except:                break        last_height = new_height  
    images = driver.find_elements(By.CSS_SELECTOR, ".rg_i.Q4LuWd")    count = 1  
    for image in images:        try:            image.click()            time.sleep(2)            imgUrl = driver.find_element(By.XPATH,                                         '//*[@id="Sva75c"]/div[2]/div[2]/div[2]/div[2]/c-wiz/div/div/div/div[3]/div['                                         '1]/a/img[1]').get_attribute(                "src")            # 이미지 저장 경로 설정  
            save_path = f'{os.getcwd()}/{folder_name}/{str(count)}.jpg'            urllib.request.urlretrieve(imgUrl, save_path)            count += 1        except Exception as e:            print(f"An error occurred: {e}")  
driver.close()  
'''  
  
# gpt - train_test split  
'''  
# 바나나 이미지를 로드하고 특징 벡터로 변환  
def load_images(folder_path, label):  
    images = []    labels = []    for filename in os.listdir(folder_path):        if filename.endswith(".jpg"):            img = cv2.imread(os.path.join(folder_path, filename), cv2.IMREAD_GRAYSCALE)            img = cv2.resize(img, (50, 50))  # 이미지 크기를 조정 (원하는 크기로 조정 가능)  
            images.append(img.flatten())  # 이미지를 1차원 벡터로 펼치기  
            labels.append(label)    return images, labels  
  
# 학습 데이터 로드  
ripe_images, ripe_labels = load_images("banana_ripe", 0)  
unripe_images, unripe_labels = load_images("banana_unripe", 1)  
overripe_images, overripe_labels = load_images("banana_overripe", 2)  
  
# 전체 학습 데이터 생성  
X = ripe_images + unripe_images + overripe_images  
y = ripe_labels + unripe_labels + overripe_labels  
  
# 학습 데이터와 테스트 데이터로 분할  
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)  
  
# SVM 모델 학습  
svm_model = SVC(kernel='linear')  
svm_model.fit(X_train, y_train)  
  
# KNN 모델 학습  
knn_model = KNeighborsClassifier(n_neighbors=3)  
knn_model.fit(X_train, y_train)  
  
# 테스트 데이터로 모델 평가  
svm_predictions = svm_model.predict(X_test)  
knn_predictions = knn_model.predict(X_test)  
  
svm_accuracy = accuracy_score(y_test, svm_predictions)  
knn_accuracy = accuracy_score(y_test, knn_predictions)  
  
print("SVM Accuracy:", svm_accuracy)  
print("KNN Accuracy:", knn_accuracy)  
  
# banana_unknown 폴더의 이미지를 분류해보기  
test_images, _ = load_images("banana_unknown", -1)  
for idx, test_image in enumerate(test_images):  
    test_image = np.array(test_image).reshape(1, -1)    svm_prediction = svm_model.predict(test_image)    knn_prediction = knn_model.predict(test_image)    print(f"Test Image {idx + 1}: SVM Prediction: {svm_prediction[0]}, KNN Prediction: {knn_prediction[0]}")'''  
  
# np.mean 이용 knn 처럼  
def averagecolor(image):  
    return np.mean(image, axis=(0, 1))  
  
  
trainX = []  
trainY = []  
  
print(os.getcwd())  
  
# 카드들에 대해 반복하고 평균 색상 출력  
labels = ("ripe", "unripe", "overripe")  
for label in labels:  
    folder_path = f"banana_{label}"  
    for filename in os.listdir(folder_path):  
        if filename.endswith(".jpg"):  
            # 이미지 파일 읽기  
            img1 = cv2.imread(os.path.join(folder_path, filename))  
            # 이미지의 평균 색상 계산  
            average_color = averagecolor(img1)  
            # 평균 색상 값을 학습 데이터에 추가  
            trainX.append(average_color)  
            # 레이블을 학습 데이터에 추가  
            trainY.append(label)  
  
print(trainX)  
print(np.array(trainY).shape)  
  
  
# 평가할 이미지들이 있는 폴더 경로  
test_folder_path = "banana_test"  
  
predictedY = []  
filenames = []  
  
# 폴더에 있는 각 이미지에 대해 예측을 수행합니다.  
for filename in os.listdir(test_folder_path):  
    if filename.endswith(".jpg"):  
        # 이미지 파일 읽기  
        img2 = cv2.imread(os.path.join(test_folder_path, filename))  
        # 이미지의 평균 색상 계산  
        img2_features = averagecolor(img2)  
  
        # 새 이미지와 학습 데이터 간의 거리 계산  
        calculated_distances = []  
        for banana in trainX:  
            calculated_distances.append(np.linalg.norm(img2_features - banana))  
  
        # 예측 결과를 predictedY 리스트에 추가  
        print(f"{filename}: {np.argmin(calculated_distances)}")  
        # prediction = labels[np.argmin(calculated_distances)]  
        # predictedY.append(prediction)        # filenames.append(filename)        # print(f"{filename}: {prediction}")  
# 실제 레이블 (수동으로 레이블을 입력해야 합니다.)  
realtestY = ["overripe", "overripe", "overripe", "overripe", "ripe", "ripe", "ripe", "ripe", "unripe", "unripe", "unripe", "unripe"]  
  
# 정확도 평가 및 보고서 출력  
print()  
print(classification_report(realtestY, predictedY))
```

indexError tuple lable 살펴봐야 함.
train_test_split 으로 나눠하는게 편함.. 

