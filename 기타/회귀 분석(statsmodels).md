# statsmodels를 활용한 회귀 분석

<br>

선형회귀 모델에 대한 통계적 분석이 필요할 때 statsmodels를 사용해 여러 지수들을 한 눈에 볼 수 있다.

```python
from statsmodels.formula.api import ols

data = pd.read_csv('Dataset_06.csv')

var_list = data.columns.drop(['id', 'date', 'zipcode', 'price'])
form1 = 'price~ ' + '+'.join(var_list)

lm = ols(form1, data2).fit()
dir(lm2)
lm.summary()
```

ols 클래스는 (식, 데이터)를 매개변수로 받는데 식을 표현하는데에 규칙이 있다.

``y~ + x1 + x2 + ... + xn``

식에서 y는 레이블, x는 독립 변수들을 의미한다. `~`는 `==`로 이해하면 된다.

summary 메소드를 통해 여러가지 통계 지수들을 확인할 수 있다.

![summary 이미지](https://github.com/Cheolyong-Kim/TIL/blob/master/%EA%B8%B0%ED%83%80/%ED%9A%8C%EA%B7%80%20%EB%B6%84%EC%84%9D%20image/i1.png?raw=true)