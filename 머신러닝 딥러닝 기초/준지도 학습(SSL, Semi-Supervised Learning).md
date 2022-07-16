# 준지도학습(SSL, Semi-Supervised Learning)

머신러닝의 학습 방법으로는 크게 지도학습과 비지도학습으로 나뉜다.
<br>
<br>
그 중에서도 지도학습을 하기 위해서는 레이블이 존재하는(답이 정해져있는) 데이터가 필요하다.
<br>
<br>
하지만 실제 데이터들을 수집하여 사용할 때는 레이블이 존재하는 데이터보다 레이블이 존재하지 않는 데이터들이 훨씬 많다.
<br>
<br>
이런 경우에 사용할 수 있는 학습 방법이 준지도학습이다.
준지도학습은 지도학습과 비지도학습을 섞은 것이라고 생각하면 편하다.
<br>
<br>
우선 지도학습을 시킨 다음 분류 모델을 만든다.
이 모델로 레이블링이 안된(Unlabeld) 데이터를 분류한다.
분류가된 Unlabeld 데이터와 기존에 학습데이터로 쓰던 데이터를 합쳐서 다시 모델을 학습시키는 것이 준지도학습 방법이다.
<br>
<br>
[링크](https://jayhey.github.io/semi-supervised%20learning/2017/12/04/semisupervised_overview/)에서 준지도학습을 간단하게 잘 설명해주고 있다.
<br>
<br>
이 문서에서는 준지도학습 예제를 통해서 어떻게 모델을 학습시키는지 알아본다.

---

예제로는 준지도학습 중에 하나인 Label Propagation Algorithm을 사용한다.
<br>
<br>

### 분류 데이터셋 만들기
<br>

```python
X,  y  =  make_classification(n_samples=1000,  n_features=2,  n_informative=2,  n_redundant=0,  random_state=1)
```
우선 1000개의 데이터를 만들어 준다.
<br>
<br>

```python
X_train,  X_test,  y_train,  y_test  =  train_test_split(X,  y,  test_size=0.50,  random_state=1,  stratify=y)
```
50:50 비율로 학습용, 테스트용 데이터로 분리한다.
<br>
<br>
```python
X_train_lab, X_test_unlab, y_train_lab, y_test_unlab = train_test_split(X_train, y_train, test_size=0.50, random_state=1, stratify=y_train)
```
학습용으로 나눈 데이터를 다시 레이블링된 데이터(labeld), 그렇지 않은 데이터(unlabeld)로 50:50 비율로 나눈다.
<br>
<br>

```python
# prepare semi-supervised learning dataset
from sklearn.datasets import make_classification
from sklearn.model_selection import train_test_split

# define dataset
X,  y  =  make_classification(n_samples=1000,  n_features=2,  n_informative=2,  n_redundant=0,  random_state=1)

# split into train and test
X_train,  X_test,  y_train,  y_test  =  train_test_split(X,  y,  test_size=0.50,  random_state=1,  stratify=y)

# split train into labeled and unlabeled
X_train_lab,  X_test_unlab,  y_train_lab,  y_test_unlab  =  train_test_split(X_train,  y_train,  test_size=0.50,  random_state=1,  stratify=y_train)

# summarize training set size
print('Labeled Train Set:',  X_train_lab.shape,  y_train_lab.shape)

print('Unlabeled Train Set:',  X_test_unlab.shape,  y_test_unlab.shape)
# summarize test set size

print('Test Set:',  X_test.shape,  y_test.shape)
```
위 과정을 모아 정리한 코드이다.
<br>
<br>
```python
Labeled Train Set: (250, 2) (250,)
Unlabeled Train Set: (250, 2) (250,)
Test Set: (500, 2) (500,)
```
출력 결과는 위와 같다
<br>
<br>
지도 학습에 250개의 데이터를 사용하고
준지도 학습에 500개(labled:250 / unlabeld:250)의 데이터를 사용하여 두 모델의 성능을 확인해본다.
<br>
<br>
```python
# define model
model = LogisticRegression()
# fit model on labeled dataset
model.fit(X_train_lab, y_train_lab)
```
로지스틱 회귀 모델을 훈련 데이터로 학습시킨다.
<br>
<br>
```python
# make predictions on hold out test set
yhat = model.predict(X_test)
# calculate score for test set
score = accuracy_score(y_test, yhat)
# summarize score
print('Accuracy: %.3f' % (score*100))
```
모델의 정확도를 계산한다.
<br>
<br>
```python
# baseline performance on the semi-supervised learning dataset
from sklearn.datasets import make_classification
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score
from sklearn.linear_model import LogisticRegression

# define dataset
X,  y  =  make_classification(n_samples=1000,  n_features=2,  n_informative=2,  n_redundant=0,  random_state=1)

# split into train and test
X_train,  X_test,  y_train,  y_test  =  train_test_split(X,  y,  test_size=0.50,  random_state=1,  stratify=y)

# split train into labeled and unlabeled
X_train_lab,  X_test_unlab,  y_train_lab,  y_test_unlab  =  train_test_split(X_train,  y_train,  test_size=0.50,  random_state=1,  stratify=y_train)

# define model
model  =  LogisticRegression()

# fit model on labeled dataset
model.fit(X_train_lab,  y_train_lab)

# make predictions on hold out test set
yhat  =  model.predict(X_test)

# calculate score for test set
score  =  accuracy_score(y_test,  yhat)

# summarize score
print('Accuracy: %.3f'  %  (score*100))
```
위 과정을 합친 코드이다.
<br>
<br>
```python
Accuracy: 84.800
```
모델의 정확도는 위와 같다
<br>
<br>
다음으로 label propagation 알고리즘을 사용해본다.
<br>
<br>
### Label Propagation을 통한 준지도 학습

label propagation 알고리즘을 사용하기 위해 sklearn에서 제공하는 LabelPropagation 클래스를 사용할 것이다.
<br>
<br>
```python
# define model
model = LabelPropagation()
# fit model on training dataset
model.fit(..., ...)
# make predictions on hold out test set
yhat = model.predict(...)
```
다른 예측 모델들과 동일하게 fit, predict 메소드를 사용할 수 있다.
주의해야 할 것은 unlabeld 데이터는 -1의 값을 주어 레이블링이 되어 있지 않다는 표시를 해주어야 한다.
<br>
<br>
```python
# get labels for entire training dataset data
tran_labels = model.transduction_
```
그 후 transduction_을 통해 입력값에 따른 예측값을 뽑아낼 수 있다.
<br>
<br>
이제 sklearn을 통해 LabelPropagtion 클래스를 사용하는 법을 알았으니 직접 사용해본다.
<br>
<br>
```python
# create the training dataset input
X_train_mixed = concatenate((X_train_lab, X_test_unlab))
```
우선 데이터셋을 만들어 줄건데, 레이블링된 데이터와 그렇지 않은 데이터를 concat해준다.
<br>
<br>
```python
# create "no label" for unlabeled data
nolabel = [-1 for _ in range(len(y_test_unlab))]
```
앞에서 말했듯이 레이블링되지 않은 데이터의 레이블값은 -1로 주어 모델이 인식하게끔 해준다.
<br>
<br>
```python
# recombine training dataset labels
y_train_mixed  =  concatenate((y_train_lab,  nolabel))
```
위에서 만든 nolabel과 기존의 레이블을 concat해준다.
<br>
<br>
```python
# define model
model = LabelPropagation()
# fit model on training dataset
model.fit(X_train_mixed, y_train_mixed)
```
이제 모델을 생성한다.
생성한 모델에 concat한 데이터들을 넣어 학습시킨다.
<br>
<br>
```python
# make predictions on hold out test set
yhat = model.predict(X_test)
# calculate score for test set
score = accuracy_score(y_test, yhat)
# summarize score
print('Accuracy: %.3f' % (score*100))
```
학습한 모델로 정확도를 출력해본다.
<br>
<br>
```python
# evaluate label propagation on the semi-supervised learning dataset

from numpy import concatenate
from sklearn.datasets import make_classification
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score
from sklearn.semi_supervised import LabelPropagation

# define dataset
X,  y  =  make_classification(n_samples=1000,  n_features=2,  n_informative=2,  n_redundant=0,  random_state=1)

# split into train and test
X_train,  X_test,  y_train,  y_test  =  train_test_split(X,  y,  test_size=0.50,  random_state=1,  stratify=y)

# split train into labeled and unlabeled
X_train_lab,  X_test_unlab,  y_train_lab,  y_test_unlab  =  train_test_split(X_train,  y_train,  test_size=0.50,  random_state=1,  stratify=y_train)

# create the training dataset input
X_train_mixed  =  concatenate((X_train_lab,  X_test_unlab))

# create "no label" for unlabeled data
nolabel  =  [-1  for  _  in  range(len(y_test_unlab))]

# recombine training dataset labels
y_train_mixed  =  concatenate((y_train_lab,  nolabel))

# define model
model  =  LabelPropagation()

# fit model on training dataset
model.fit(X_train_mixed,  y_train_mixed)

# make predictions on hold out test set
yhat  =  model.predict(X_test)

# calculate score for test set
score  =  accuracy_score(y_test,  yhat)

# summarize score
print('Accuracy: %.3f'  %  (score*100))
```
위 과정을 모두 합친 코드이다.
<br>
<br>
```python
Accuracy: 85.600
```
출력값은 위와 같다
<br>
<br>
이제 준지도학습으로 레이블링한 데이터로 다시 지도학습을 해줄 것이다.
<br>
<br>
```python
# get labels for entire training dataset data
tran_labels = model.transduction_
```
준지도학습을 통해 만들어진 레이블을 transduction_ 속성으로 가져온다.
<br>
<br>
이제 준지도학습을 통해서 레이블된 데이터 500개를 얻었다.
이 데이터로 다시 학습을 진행해본다.
<br>
<br>
```python
# define supervised learning model
model2 = LogisticRegression()
# fit supervised learning model on entire training dataset
model2.fit(X_train_mixed, tran_labels)
# make predictions on hold out test set
yhat = model2.predict(X_test)
# calculate score for test set
score = accuracy_score(y_test, yhat)
# summarize score
print('Accuracy: %.3f' % (score*100))
```
모델은 동일하게 로지스틱 회귀 모델을 사용한다.
학습시킨 모델의 정확도를 확인해본다.
<br>
<br>
```python
# evaluate logistic regression fit on label propagation for semi-supervised learning
from numpy import concatenate
from sklearn.datasets import make_classification
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score
from sklearn.semi_supervised import LabelPropagation
from sklearn.linear_model import LogisticRegression

# define dataset
X,  y  =  make_classification(n_samples=1000,  n_features=2,  n_informative=2,  n_redundant=0,  random_state=1)

# split into train and test
X_train,  X_test,  y_train,  y_test  =  train_test_split(X,  y,  test_size=0.50,  random_state=1,  stratify=y)

# split train into labeled and unlabeled
X_train_lab,  X_test_unlab,  y_train_lab,  y_test_unlab  =  train_test_split(X_train,  y_train,  test_size=0.50,  random_state=1,  stratify=y_train)

# create the training dataset input
X_train_mixed  =  concatenate((X_train_lab,  X_test_unlab))

# create "no label" for unlabeled data
nolabel  =  [-1  for  _  in  range(len(y_test_unlab))]

# recombine training dataset labels
y_train_mixed  =  concatenate((y_train_lab,  nolabel))

# define model
model  =  LabelPropagation()

# fit model on training dataset
model.fit(X_train_mixed,  y_train_mixed)

# get labels for entire training dataset data
tran_labels  =  model.transduction_

# define supervised learning model
model2  =  LogisticRegression()

# fit supervised learning model on entire training dataset
model2.fit(X_train_mixed,  tran_labels)

# make predictions on hold out test set
yhat  =  model2.predict(X_test)

# calculate score for test set
score  =  accuracy_score(y_test,  yhat)

# summarize score
print('Accuracy: %.3f'  %  (score*100))
```
지금까지의 과정을 모아놓은 코드이다.
<br>
<br>
```python
Accuracy: 86.200
```
출력은 위와 같다
지금까지의 모든 모델 중에서 가장 정확도가 높게 나온 것을 알 수 있다.

---

이 문서의 처음에 말했듯이 실제 데이터는 레이블링이 안된 데이터들이 대부분이다.

물론 사람이 수작업으로 레이블링한 데이터가 더 정확하겠지만
레이블링을 할 때 발생하는 시간과 비용이 아주 많다는 것은 수작업의 가장 큰 문제점일 것이다.

위 예제를 통해 준지도학습을 통해서 모델을 학습시켰을 때 어느정도의 성능 향상을 기대할 수 있다는 것을 알 수 있었다.
레이블 데이터가 없을 때, 시간적 여유가 없을 때 고민해볼 수 있는 학습 방법인 것 같다.

---

참고 링크 : [https://machinelearningmastery.com/semi-supervised-learning-with-label-propagation/](https://machinelearningmastery.com/semi-supervised-learning-with-label-propagation/)