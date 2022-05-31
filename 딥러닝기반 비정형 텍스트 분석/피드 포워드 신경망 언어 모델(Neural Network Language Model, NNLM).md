# 피드 포워드 신경망 언어 모델(Neural Network Language Model, NNLM)

<br>

기존 N-gram 언어 모델은 충분한 데이터를 관측하지 못하면 언어를 정확히 모델링하지 못하는 희소 문제가 존재했다. 학습 데이터에 없는 단어 시퀀스는 예측하지 못하는 문제이다.

<br>

만약 언어 모델이 단어의 의미적 유사성을 학습할 수 있도록 설계한다면, 훈련 코퍼스에 없는 단어 시퀀스에 대한 예측이라도 유사한 단어가 사용된 단어 시퀀스를 참고하여 예측할 수 있을 것이다.

이런 아이디어를 반영한 언어 모델이 신경망 언어 모델 NNLM이다.

<br>

NNLM은 아래 그림과 같은 구조를 가진다.

![NNLM 구조](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/NNLM%20image/i1.png?raw=true)

<br>

예문 'what will the fat cat sit on'

우선 NNLM은 훈련 코퍼스의 단어들을 원-핫 인코딩한다.

그리고 n-gram 언어 모델처럼 다음 단어를 예측할 때, n개의 단어를 참고한다. 설명에서는 n을 4로 설정한다.

<br>

4개의 원-핫 벡터를 입력받은 NNLM은 다음 층인 투사층(projection layer)을 지나게 된다. 투사층이 은닉층과 다른 점은 가중치 행렬과의 곱셈은 이루어지지만 활성화 함수가 존재하지 않는다는 것이다.

![투사층](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/NNLM%20image/i2.png?raw=true)

원-핫 벡터와 가중치 w 행렬의 곱은 w행렬의 i번째 행을 그대로 읽어오는 것과 동일하다. 그래서 이 작업을 룩업 테이블이라고 한다.

룩업 테이블 후에 원-핫 벡터는 원-핫 벡터때 보다 차원이 작은 M차원의 벡터로 맵핑된다.

이 벡터들은 초기에는 랜덤한 값을 가지지만 학습 과정에서 값이 계속 변경되는데 이 단어 벡터를 임베딩 벡터라고 한다. 이 임베딩 벡터가 후에 Word2Vec, FastText, GloVe 등으로 발전되어 딥러닝 자연처 처리 모델에서는 필수적으로 사용되는 방법이 되었다.

<br>

![임베딩 벡터 연결](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/NNLM%20image/i3.png?raw=true)

각 단어가 임베딩 벡터로 변경되고, 투사층에서 모든 임베딩 벡터들의 값이 연결된다(concatenate).

연결은 이어붙이는 것을 의미한다.

<br>

![은닉층 통과](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/NNLM%20image/i4.png?raw=true)

투사층의 결과는 h의 크기를 지나는 은닉층을 지난다. 은닉층을 지나면서 은닉층의 입력은 가중치가 곱해진 후 편향이 더해져 활성화 함수의 입력이 된다.

이 때 가중치와 편향을 Wh와 bh라고 하고, 활성화 함수를 하이퍼볼릭탄젠트 함수라고 했을 때, 은닉층을 수식으로 표현하면 아래와 같다.

![수식](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/NNLM%20image/i5.png?raw=true)

<br>

![출력층](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/NNLM%20image/i6.png?raw=true)

은닉층의 출력은 v의 크기를 가지는 출력층으로 향한다. 이 과정에서 다시 가중치가 곱해지고 편향이 더해지면 입력 벡터와 같은 v차원의 벡터를 얻는다.

출력층에서는 활성화 함수로 소프트맥스 함수를 사용하는데, 벡터의 각 원소는 총 합이 1이 되는 확률을 나타내게 된다.

NNLM은 손실 함수로 크로스 엔트로피 함수를 사용한다(다중 클래스 분류 문제).

이후에 역전파가 이루어지면서 모든 가중치 행렬들이 학습되면서 임베딩 벡터값 또한 학습된다.

<br>

NNLM은 단어를 표현하기 위해 임베딩 벡터를 사용함으로써 단어의 유사도를 계산하고 이를 통해 희소 문제를 해결했다.

하지만 n-gram 언어 모델과 마찬가지로 다음 단어를 예측하기 위해 정해진 n개의 단어만 참고할 수 있다.

이 한계를 극복할 수 있는 언어 모델이 RNN을 사용한 RNN언어 모델이다.