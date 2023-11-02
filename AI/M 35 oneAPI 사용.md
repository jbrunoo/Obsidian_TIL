가상환경 설정
```python
conda create -n DAAL4PY python=3.8 impi-devel daal daal-include cython jinja2 numpy clang-tools -c intel -c conda-forge
```


# 35-1 oneAPI 사용 회귀
```python
# data = load_boston()

url = 'https://archive.ics.uci.edu/ml/machine-learning-databases/housing/housing.data'
feature_names = ['CRIM', 'ZN', 'INDUS', 'CHAS', 'NOX', 'RM', 'AGE', 'DIS', 'RAD', 'TAX', 'PTRATIO', 'B', 'LSTAT', 'target']
# 데이터 파일 읽기
data = pd.read_csv(url, header=None, delimiter=r'\s+', names=feature_names)
data.head()

import matplotlib.pyplot as plt
plt.figure(figsize=(4, 3))
plt.hist(data.target)
plt.xlabel('price ($1000s)')
plt.ylabel('price')
plt.tight_layout()

# feature 에 대해 sactter 로 x: 집의 특성 , y : 집값 에 대해 그래프 그리기
# your code here
for feature in feature_names:
    plt.figure(figsize=(6, 4))
    plt.scatter(data[feature], data['target'])
    plt.xlabel(feature)
    plt.ylabel('Price')
    plt.show()
```

### 학습
```python
# 예측을 위해 모델에 사용된 변수 구성

X = data.drop(columns=['target'], axis=1)  # ... your code -  집의 특성 
y = data['target']  # ... your code -  집의 가격 y = data.target

# 25% 테스트 데이터 세트로 학습 및 테스트 데이터 분할
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.25, random_state =1693)

# 예측을 위한 모델 학습
# Intel OneAPI의 데이터 분석 라이브러리 사용
train_result = d4p.linear_regression_training().compute(X_train, y_train)


# 훈련된 모델을 사용하여 목표 특성을 예측
y_pred = d4p.linear_regression_prediction().compute(X_test, train_result.model).prediction

# 결과
print("Predicted prices:")
print(y_pred)

# 결과 시각화
expected = y_test

plt.figure(figsize=(4, 3))
plt.scatter(expected, y_pred)
plt.plot([0, 50], [0, 50], '--k')
plt.axis('tight')
plt.xlabel('True price ($1000s)')
plt.ylabel('Predicted price ($1000s)')
plt.tight_layout()

print(expected)

# error 발생 시 코드 수정하세요.
print("RMS: %r " % np.sqrt(np.mean((y_pred.ravel() - expected.ravel()) ** 2))) # .ravel() 함수를 이용해 2차원을 평탄화함
plt.show()
```


# 35-2 oneAPI 사용 KNN
```python
import daal4py as d4p
import numpy as np

import pandas

readcsv = lambda f, c, t=np.float64: pandas.read_csv(f, usecols=c, delimiter=',', header=None, dtype=t)
# 입력 데이터 파일
train_file = '[Dataset]_Module35_Train_(knn).csv'
predict_file  = '[Dataset]_Module35_Test_(knn).csv'

nFeatures = 5
nClasses = 5
train_data   = readcsv(train_file, range(nFeatures))
train_labels = readcsv(train_file, range(nFeatures, nFeatures+1))

### 학습
# 알고리즘 객체를 생성하고 compute를 호출합니다.
train_algo = d4p.kdtree_knn_classification_training(nClasses=nClasses)
# 'weights'는 선택적 인수입니다. 동일한 가중치(1)를 사용합니다.
# 이 경우 결과는 가중치가 없는 것과 동일해야 합니다.
weights = np.ones((train_data.shape[0], 1))
train_result = train_algo.compute(train_data, train_labels, weights)

### 예측
predict_data = readcsv(predict_file, range(nFeatures))
predict_labels = readcsv(predict_file, range(nFeatures, nFeatures+1))

# 알고리즘 객체를 생성하고 compute를 호출합니다.
predict_algo = d4p.kdtree_knn_classification_prediction(nClasses=nClasses)
predict_result = predict_algo.compute(predict_data, train_result.model)

### 결과
print("kNN classification results:")
print("Ground truth(observations #13-14):\n", predict_labels[13:15])
print("Classification results(observations #13-14):\n", predict_result.prediction[13:15])
```

# 35-3 oneAPI 사용 랜덤 포레스트
```python
import daal4py as d4p
import numpy as np

import pandas

readcsv = lambda f, c, t=np.float64: pandas.read_csv(f, usecols=c, delimiter=',', header=None, dtype=t)
# 입력 데이터 파일
infile = "[Dataset]_Module35_Train_(Random_forest).csv"
testfile = "[Dataset]_Module35_Test_(Random_forest).csv"

# 훈련 개체를 구성합니다(5개 클래스).
train_algo = d4p.decision_forest_classification_training(5, nTrees=10, minObservationsInLeafNode=8, featuresPerNode=3, engine = d4p.engines_mt19937(seed=777), varImportance='MDI', bootstrap=True, resultsToCompute='computeOutOfBagError')

# 이제 데이터를 읽어봅시다. 관측치당 3개의 특성(features)을 사용합니다.
data   = readcsv(infile, range(3), t=np.float32)
labels = readcsv(infile, range(3,4), t=np.float32)
train_result = train_algo.compute(data, labels)

### 예측
# 함수에 대한 올바른 매개변수를 선택합니다.
predict_algo = d4p.decision_forest_classification_prediction(nClasses=5)

# 테스트 데이터 읽기(동일한 특성 사용)
pdata = readcsv(testfile, range(3), t=np.float32)
plabels = readcsv(testfile, range(3,4), t=np.float32)
# 위의 훈련 모델을 사용하여 예측합니다.
predict_result = predict_algo.compute(pdata, train_result.model)

# Prediction_result는 예측 값을 제공합니다.
assert(predict_result.prediction.shape == (pdata.shape[0], 1))

### 결과
print("\nRandom forest prediction results (first 10 rows):\n", predict_result.prediction[0:10])
print("\nGround truth (first 10 rows):\n", plabels[0:10])
```

