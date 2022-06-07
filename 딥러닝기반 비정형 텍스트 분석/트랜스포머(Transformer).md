# 트랜스포머

<br>

트랜스포머는 기존의 seq2seq의 구조인 인코더-디코더를 따르면서도, RNN을 사용하지 않고 어텐션만으로 구현한 모델이다. RNN을 사용하지 않았음에도 번역 성능에서 더 우수한 성능을 보여준다.

<br>

기존의 seq2seq의 문제점들을 보완하기 위해 어텐션이 사용되었다. 트랜스포머는 여기에서 어텐션을 RNN의 보정을 위한 용도로서 사용하는 것이 아니라 아예 어텐션만으로 인코더 디코더를 만들자는 생각에서 등장하게 되었다.

<br>

---

### 트랜스포머의 주요 하이퍼파라미터

<br>

아래에서 정의하는 수치는 트랜스포머를 제안한 논문에서 사용한 수치로 사용자가 모델 설계시 임의로 변경할 수 있는 값들이다.

<br>

- dmodel = 512

  트랜스포머의 인코더와 디코더에서의 정해진 입력과 출력의 크기를 의미.

  <br>

- num_layers = 6

  트랜스포머에서 하나의 인코더와 디코더를 층으로 생각했을 때, 트랜스포머 모델에서 인코더, 디코더가 총 몇 층으로 구성되어 있는지를 의미

  <br>

- num_heads = 8

  트랜스포머에서는 어텐션을 사용할 때, 여러 개로 분할해서 병렬로 어텐션을 수행하고 결과값을 다시 하나로 합치는 방식을 선택했다. 이 때 병렬의 개수를 의미

  <br>

- dff = 2048

  트랜스포머 내부에는 피드 포워드 신경망이 존재하는데 이 신경망의 은닉층 크기를 의미한다.

  피드 포워드 신경망의 입력층과 출력층의 크기는 dmodel이다.

<br>

---

### 트랜스포머

<br>

![트랜스포머 모델 간단](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/Transformer%20image/i1.png?raw=true)

트랜스포머는 RNN을 사용하지 않지만 기존의 seq2seq처럼 인코더-디코더 구조를 유지하고 있다.

이전 seq2seq 구조에서는 인코더와 디코더에서 각각 하나의 RNN이 t개의 시점을 가지는 구조였다면 트랜스포머는 인코더와 디코더라는 단위가 N개로 구성되는 구조이다.

<br>

![트랜스포머 모델 자세히](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/Transformer%20image/i2.png?raw=true)

위 그림은 인코더와 디코더가 6개씩 존재하는 트랜스포머의 구조를 보여준다.

<br>

![트랜스포머 모델 동작](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/Transformer%20image/i3.png?raw=true)

위 그림은 인코더로부터 정보를 받아 디코더가 출력 격롸를 만들어내는 트랜스포머 구조를 보여준다.

디코더는 기존의 seq2seq 구조처럼 시작 심볼 ``<sos>``를 입력받아 종료 심볼 ``<eos>``가 나올 때까지 연산을 진행한다.

<br>

---

### 포지셔널 인코딩

<br>

RNN이 자연어 처리에서 유용했던 이유는 단어의 위치에 따라 단어를 순차적으로 입력받아서 처리하는 특성으로 각 단어의 위치 정보를 가질 수 있는 것에 강점이 있었기 때문이다.

하지만 트랜스포머는 단어 입력을 순차적으로 받는 방식이 아니기 때문에 단어의 위치 정보를 다른 방식으로 알려줄 필요가 있다.

트랜스포머는 단어의 위치 정보를 얻기 위해 각 단어의 임베딩 벡터에 위치 정보들을 더하여 모델의 입력으로 사용하는데, 이를 포지셔널 인코딩이라고 한다.

<br>

![포지셔널 인코딩 이미지](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/Transformer%20image/i4.png?raw=true)

위 그림은 입력으로 사용되는 임베딩 벡터들이 트랜스포머의 입력으로 사용되기 전에 포지셔널 인코딩의 값이 더해지는 것을 보여준다.

<br>

![포지셔널 인코딩 자세히](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/Transformer%20image/i5.png?raw=true)

위 그림은 임베딩 벡터가 인코더의 입력으로 사용되기 전에 포지셔널 인코딩값이 더해지는 과정을 시각화한 것이다.

<br>

---

### 어텐션

<br>

트랜스포머에서는 세 가지의 어텐션을 사용한다.

<br>

![트랜스포머 어텐션 이미지](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/Transformer%20image/i6.png?raw=true)

Encoder Self-Attention은 인코더에서 이루어지고, Masked Decoder Self-Attention와 Encoder-Decoder Attention은 디코더에서 이루어진다.

셀프 어텐션은 Query, Key, Value가 동일한 경우를 말한다. 여기서 Query, Key 등이 같다는 것은 벡터의 값이 같다는 것이 아니라 벡터의 출처가 같다는 의미이다.

각각 정리하면

```tex
인코더의 셀프 어텐션 : Query = Key = Value
디코더의 마스크드 셀프 어텐션 : Query = Key = Value
디코더의 인코더-디코더 어텐션 :: Query : 디코더 벡터 / Key = value : 인코더 벡터
```

<br>

![트랜스포머 어텐션 위치](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/Transformer%20image/i7.png?raw=true)

위 그림은 트랜스포머의 아키텍처에서 세 가지 어텐션이 각각 어디에서 이루어지는 지 보여준다.

멀티 헤드는 트랜스포머가 어텐션을 병렬적으로 수행하는 방법을 의미한다.

<br>

---

### 참고

<br>

이 문서에서는 트랜스포머에 대해 아주 간단하게만 알아봤는데 [이 링크](https://wikidocs.net/31379)에서 트랜스포머에 대한 자세한 설명과 트랜스포머를 활용한 챗봇 구현에 대해 공부할 수 있다.



