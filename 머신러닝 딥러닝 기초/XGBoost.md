# XGBoost

<br>

XGBoost는 Extreme Gradient Boosting의 약자이다. Gradient Boost를 병렬 학습이 지원되도록 구현한 라이브러리이다. 회귀, 분류문제 모두 지원하며 성능도 출중하고, 자원을 사용하는데 효율이 좋아 자주 사용하는 알고리즘이다.

<br>

[링크 1](https://bcho.tistory.com/1354)

[링크 2](https://wooono.tistory.com/97)

에서 자세한 설명을 확인할 수 있다.

<br>

XGBoost를 사용하려면 우선 라이브러리를 인스톨 해줘야한다.

``pip install xgboost``를 입력하여 인스톨한다.

모델 사용방법은 다른 모델들과 크게 다르지 않다.

```python
import xgboost as xgb

model = xgb.XGBRegressor()
model.fit(x, y)

y_pred = model.predict(x_test)
```

XGBoost는 하이퍼 파라미터가 많아서 모델 튜닝이 어렵지만 위에서 말한 것처럼 성능이 좋기 때문에 다른 회귀모델들보다 자주 이용된다.

하이퍼 파라미터는 [API 문서](https://xgboost.readthedocs.io/en/stable/parameter.html)에서 확인할 수 있다.

하이퍼 파리미터 튜닝에 관한 정보는 [링크](https://zzinnam.tistory.com/entry/XGBoost-%ED%8C%8C%EB%9D%BC%EB%AF%B8%ED%84%B0-%EC%A1%B0%EC%A0%95%ED%8A%9C%EB%8B%9D) 이 곳에서 확인할 수 있다.