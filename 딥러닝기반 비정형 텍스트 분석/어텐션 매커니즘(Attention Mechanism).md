# 어텐션 매커니즘(Attention Mechanism)

<br>

seq2seq 모델은 RNN 모델을 기반으로 만들어졌기 때문에 필연적으로 단기 기억 문제를 가지고 있다.

즉, 문장이 길어져버리면 번역 품질이 떨어지는 문제가 발생한다.

이런 문제를 보완하기 위해 등장한 기법이 어텐션이다.

<br>

어텐션은 디코더에서 출력 단어를 예측하는 매 시점마다 인코더에서의 전체 입력 문장을 참고하면서 번역의 질을 높인다.

쉽게 표현하여 임의의 시점에서 예측해야할 단어와 연관이 있는 단어 부분을 더 집중해서 본다고 할 수 있다.

<br>

어텐션을 함수로 표현하면 주로 다음과 같이 표현된다.

``Attention(Q, K, V) = Attention Value``

어텐션 함수는 주어진 쿼리에 대해 모든 키와의 유사도를 각각 구한다. 그리고 구한 유사도를 키와 맵핑되어 있는 각각의 값에 반영한다. 그 후 유사도가 반영된 값을 모두 더해 리턴한다. 이를 어텐션 값이라고 한다.

Q, K, V는 각각 Query, Keys, Values를 의미한다.

Q = t 시점의 디코더 셀에서의 은닉 상태

K = 모든 시점의 인코더 셀의 은닉 상태들

V = 모든 시점의 인코더 셀의 은닉 상태들

<br>

---

### 닷-프로덕트 어텐션(Dot-Product Attention)

<br>

어텐션에는 다양한 종류가 있는데 그 중에서도 가장 이해하기 쉬운 닷-프로덕트 어텐션을 알아본다.

<br>

![닷-프로덕트 전체 구조 이미지](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/Attention%20Mechanism%20image/i1.png?raw=true)

닷-프로덕트 어텐션의 전체 구조는 이렇게 구성되어 있다. 이해하기 쉽게 단계별로 자세히 알아본다.

<br>

![어텐션 스코어 구하기](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/Attention%20Mechanism%20image/i2.png?raw=true)

위 그림에서 h1, h2, h3, h4는 인코더의 은닉 상태를 의미한다. st는 시점 t에서의 디코더의 은닉 상태이다.

어텐션 스코어는 st를 전치하고 각 은닉 상태와 내적을 수행하여 얻어낸다. 

그러면 은닉 상태의 개수만큼 어텐션 스코어의 모음이 생긴다.

<br>

![어텐션 분포 구하기](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/Attention%20Mechanism%20image/i3.png?raw=true)

만들어 놓은 어텐션 스코어 모음에 소프트맥스 함수를 적용하여 확률 분포를 얻어낸다.

이를 어텐션 분포라고 하고, 각각의 값은 어텐션 가중치라고 한다.

<br>

![어텐션 값 구하기](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/Attention%20Mechanism%20image/i4.png?raw=true)

어텐션 값은 각 인코더의 은닉 상태와 어텐션 가중치값들을 곱하고, 최종적으로 모두 더하여 얻어낸다(가중합을 함).

이러한 어텐션 값은 컨벡스트 벡터라고도 불린다.

<br>

![연결](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/Attention%20Mechanism%20image/i5.png?raw=true)

구해놓은 어텐션 값과 t 시점의 디코더의 은닉상태를 연결한다(concatenate).

<br>

![최후 연산](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/Attention%20Mechanism%20image/i6.png?raw=true)

연결한 벡터를 가중치 행렬과 곱해 하이퍼볼릭 탄젠트함수를 지나게 하여 새로운 벡터를 얻는다.

그 후 새로 만든 벡터를 입력으로 사용하여 예측 벡터를 얻는다.

<br>

---

### 다양한 종류의 어텐션

<br>

어텐션 스코어를 구하는 어텐션 스코어 함수에 따라 여러 종류의 어텐션이 존재한다. 어텐션 마다 수식 종류가 다르고 과정은 닷-프로덕트 어텐션과 거의 같다.

<br>

![어텐션 종류](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/Attention%20Mechanism%20image/i7.png?raw=true)

이렇듯 여러가지의 어텐션이 존재한다.

<br>

---

### 양방향 lstm + 어텐션 예제

<br>

양방향 lstm에 어텐션 매커니즘을 추가하여 더 좋은 성능을 내는 분류기를 만들 수 있다.

<br>

![j1](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/Attention%20Mechanism%20image/j1.png?raw=true)

필요한 패키지를 불러오고 예제를 위해 imdb 리뷰 데이터를 가져온다.

<br>

![j2](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/Attention%20Mechanism%20image/j2.png?raw=true)

패딩을 위해 리뷰의 최대 길이와 평균 길이를 확인해본다.

최대 길이와 평균 길이의 차가 꽤 크기 때문에 평균 길이보다 조금 크게 패딩한다.

<br>

![j3](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/Attention%20Mechanism%20image/j3.png?raw=true)

해당 예제에서는 바다나우 어텐션을 사용한다. 위의 표에서 바다나우 어텐션 스코어 함수를 참고하여 함수를 작성한다.

<br>

![j4](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/Attention%20Mechanism%20image/j4.png?raw=true)

입력층과 임베딩층을 설계하고 양방향 LSTM을 설계한다.

어텐션 연산을 위해서 은닉 상태와 셀 상태를 저장해둔다.

양방향 LSTM이기 때문에 순방향, 역방향으로 각각 상태가 존재한다.

<br>

![j5](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/Attention%20Mechanism%20image/j5.png?raw=true)

각 상태의 크기를 확인

<br>

![j6](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/Attention%20Mechanism%20image/j6.png?raw=true)

양방향 LSTM의 은닉 상태와 셀 상태를 사용하기 위해 두 방향의 상태들을 연결해준다.

<br>

![j7](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/Attention%20Mechanism%20image/j7.png?raw=true)

어텐션 매커니즘 함수를 통해 컨텍스트 벡터를 얻는다. 입력으로는 은닉 상태를 사용한다.

<br>

![j8](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/Attention%20Mechanism%20image/j8.png?raw=true)

컨텍스트 벡터를 dense 층에 통과시키고, 이진 분류이기 때문에 그에 맞게 출력층을 설계해준다.

그 후 모델을 컴파일한다.

<br>

![j9](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/Attention%20Mechanism%20image/j9.png?raw=true)

모델을 훈련시키고 테스트 데이터에 대한 정확도를 출력한다.