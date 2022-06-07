# 시퀀스-투-시퀀스(Sequence-to-Sequence, seq2seq)

<br>

### RNN을 이용한 인코더-디코더

<br>

인코더-디코더 구조는 한 RNN셀을 인코더, 또 다른 RNN셀을 디코더라는 모듈로 명명하고 두 개의 RNN셀을 연결해서 사용하는 구조이다.

인코더-디코더 구조는 주로 입력 문장과 출력 문장의 길이가 다를 경우에 사용하는데, 대표적으로 번역기에 사용된다.

<br>

<br>

#### 시퀀스-투-시퀀스

<br>

시퀀스-투-시퀀스는 입력된 시퀀스로부터 다른 도메인의 시퀀스를 출력하는 모델이다. 챗봇이나 기계 번역이 그 예이다.

<br>

![seq2seq 블랙박스 이미지](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/seq2seq%20image/i1.png?raw=true)

seq2seq는 번역기에서 대표적으로 사용되는 모델이다. 내부가 보이지 않는 블랙 박스에서 점점 자세히 이미지를 표현하면서 알아본다.

<br>

![seq2seq 인코더 디코더](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/seq2seq%20image/i2.png?raw=true)

seq2seq는 크게 인코더와 디코더 두 개의 모듈로 구성된다.

인코더는 입력 문장의 단어들을 순차적으로 입력받고 마지막에 모든 단어 정보를 압축하여 컨벡스트 벡터로 만든다.

디코더는 인코더에게 컨벡스트 벡터를 받아서 번역된 단어를 하나씩 순차적으로 출력한다.

<br>

![seq2seq LSTM셀 추가](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/seq2seq%20image/i3.png?raw=true)

인코더와 디코더 내부는 RNN 아키텍처로 이루어져 있다.

바닐라 RNN보다는 LSTM, GRU셀들이 자주 사용된다.

디코더는 기본적으로 RNNLM이다.

디코더는 초기 입력으로 문장의 시작을 의미하는 심볼 ``<sos>``가 들어간다. 이 심볼로 다음에 등장할 확률이 높은 단어를 예측한다. 예측한 단어로 다음 단어를 예측하고 이 과정을 반복해서 ``<eos>``가 등장하면 반복을 종료한다.

<br>

seq2seq는 훈련 과정과 테스트 과정의 작동 방식이 조금 다르다.

훈련 과정에서는 [교사 강요](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/%EA%B5%90%EC%82%AC%20%EA%B0%95%EC%9A%94(Teacher%20Forcing).md)로 정답을 알려주면서 훈련한다.

반면 테스트 과정에서는 컨벡스트 벡터와 ``<sos>``만을 입력으로 받고 다음 단어를 예측한다.

<br>

![seq2seq 워드 임베딩 층](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/seq2seq%20image/i4.png?raw=true)

seq2seq에서 사용되는 모든 단어들은 임베딩 벡터로 변환 후 입력으로 사용된다.

<br>

![seq2seq 디코더](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/seq2seq%20image/i5.png?raw=true)

디코더는 인코더의 마지막 RNN셀의 은닉 상태인 컨벡스트 벡터를 첫번째 은닉 상태의 값으로 사용한다. 첫번째 RNN셀은 이 첫번째 은닉 상태의 값과 ``<sos>``로부터 다음 단어를 예측한다. 예측한 단어는 다음 t+1 RNN의 입력값이 된다.

<br>

![seq2seq 다중 분류](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/seq2seq%20image/i6.png?raw=true)

디코더에서 각 시점의 RNN셀에서 출력 벡터가 나오면, 해당 벡터는 소프트맥스 함수를 통해 출력 시퀀스의 각 단어 별 확률값을 반환하고, 디코더는 출력 단어를 결정한다.

<br>

---

### Word-Level 번역기 만들기 예제

<br>

![j1](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/seq2seq%20image/j1.png?raw=true)

필요한 패키지와 사용할 데이터를 불러온다.

<br>

![j2](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/seq2seq%20image/j2.png?raw=true)

약 20만개의 데이터 중에서 예제 진행을 위해 3만개 정도만 사용한다.

<br>

![j3](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/seq2seq%20image/j3.png?raw=true)

해당 예제는 영어를 프랑스어로 번역하는 프로그램 코드이다.

번역 전에 영어와 프랑스어를 전처리하기 위해 전처리 함수를 정의한다.

<br>

![j4](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/seq2seq%20image/j4.png?raw=true)

전처리 전의 영어, 프랑스어 문장과 전처리 후의 문장을 확인해본다.

<br>

![j5](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/seq2seq%20image/j5.png?raw=true)

불러온 병렬 코퍼스를 전처리해준다. 병렬 코퍼스란 두 개 이상의 언어가 병렬적으로 구성된 코퍼스를 이야기한다.

디코더의 학습을 위해 target 데이터의 처음과 끝에 ``<sos>``, ``<eos>``를 추가해준다.

<br>

![fra 데이터](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/seq2seq%20image/fra_data.png?raw=true)

병렬 코퍼스는 이렇게 구성되어 있다.

<br>

![j6](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/seq2seq%20image/j6.png?raw=true)

전처리된 문장들을 5개만 출력해서 확인해본다.

디코더의 입력과 레이블은 ``<sos>``와 ``<eos>``를 제외하고는 동일한 문장인 것을 확인할 수 있다.

<br>

![j7](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/seq2seq%20image/j7.png?raw=true)

전처리한 코퍼스들을 토큰화하고 정수화한 다음, 패딩해준다.

<br>

![j8](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/seq2seq%20image/j8.png?raw=true)

영단어와 프랑스단어 집합의 크기를 확인해본다. 

<br>

![j9](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/seq2seq%20image/j9.png?raw=true)

테스트 데이터를 분리하기 전에 데이터를 섞어준다.

<br>

![j10](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/seq2seq%20image/j10.png?raw=true)

훈련 데이터의 10%를 테스트 데이터로 분리한다.

<br>

![j11](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/seq2seq%20image/j11.png?raw=true)

모델 생성을 위해 패키지를 불러온다.

미리 뉴런 수와 임베딩 차원을 정의해둔다.

<br>

![j12](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/seq2seq%20image/j12.png?raw=true)

인코더 모델을 생성한다.

인코더는 컨벡스트 벡터를 디코더에게 넘겨줘야 하기 때문에 상태값 리턴을 위해 return_state를 True로 설정한다. 넘겨줄 은닉층 상태를 encoder_states에 저장해둔다.

<br>

![j13](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/seq2seq%20image/j13.png?raw=true)

디코더 모델을 생성한다.

모든 시점의 단어를 예측하기 위해 return_sequences를 True로 설정한다.

인코더의 은닉 상태를 디코더의 초기 은닉 상태로 사용한다. 디코더도 은닉 상태, 셀 상태를 반환하긴 하지만 훈련단계에서는 사용하지 않는다.

소프트맥스 함수를 통해 다음 단어의 확률을 계산할 수 있도록한다(다중 클래스 분류).

다중 클래스 분류에서 손실 함수를 categorical_crossentropy를 사용하려면 레이블이 원-핫 인코딩되어 있어야 한다. 예제에서는 레이블이 원-핫 인코딩되어 있지 않는데, 정수 레이블에 대해 다중 클래스 분류 문제를 풀고싶다면 categorical_crossentropy 대신 sparse_categorical_crossentropy를 사용하면 된다.

<br>

![j14](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/seq2seq%20image/j14.png?raw=true)

모델을 학습시킨다.

