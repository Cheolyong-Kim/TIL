# 버트(Bidirectional Encoder Representations from Transformers, BERT)

<br>

### NLP에서의 사전 훈련(Pre-training)

<br>

#### 사전 훈련된 워드 임베딩

<br>

임베딩을 사용하는 방법으로는 크게 두 가지가 있다.

임베딩 층을 랜덤 초기화하여 처음부터 학습하는 방법.

방대한 데이터로 사전에 학습된 임베딩 벡터를 가져와 사용하는 방법.

만약 태스크에 사용하기 위한 데이터가 적다면, 사전 훈련된 임베딩을 사용하면 성능을 높일 수 있다.

하지만 이 두 방법 모두 하나의 단어가 하나의 벡터값으로 맵핑되기때문에, 문맥을 고려하지 못하여 다의어나 동음이의어를 구분하지 못하는 문제점이 있다.

이 한계를 사전 훈련된 언어 모델을 사용함으로서 극복할 수 있게 된다.

<br>

#### 사전 훈련된 언어 모델

<br>

![ELMo 이미지](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/BERT%20image/i1.png?raw=true)

ELMo는 순방향 언어 모델과 역방향 언어 모델을 각각 따로 학습시킨 후에, 사전 학습된 언어 모델로부터 임베딩 값을 얻는다. 이러한 임베딩은 문맥에 따라서 임베딩 벡터값이 달라지므로, 기존 워드 임베딩이 다의어나 동음이의어를 구분하지 못하는 문제를 해결할 수 있었다.

<br>

![GPT-1 이미지](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/BERT%20image/i2.png?raw=true)

언어 모델이 RNN 계열의 신경망에서 벗어나 인코더-디코더 구조에서 트랜스포머가 더 좋은 성능을 내기 시작하면서 LSTM이 아닌 트랜스포머로 사전 훈련된 언어 모델을 학습하는 시도가 등장했다.

Open AI는 트랜스포머 디코더로 총 12개의 층을 쌓은 후에 방대한 텍스트 데이터를 학습시킨 언어 모델 GPT-1을 만들었다.

<br>

---

### BERT(Bidirectional Encoder Representations from Transformers)

<br>

BERT는 2018년에 구글이 공개한 사전 훈련된 모델이다.

BERT는 공개됨과 동시에 수많은 NLP 태스크에서 최고 성능을 보여주면서 NLP 역사의 한 획을 그은 모델로 평가받고 있다.

<br>

BERT는 트랜스포머를 이용하여 구현되었으며, 위키피디아(25억개 단어)와 BooksCorpus(8억 단어)와 같은 레이블이 없는 텍스트 데이터로 사전 훈련된 언어 모델이다.

BERT가 높은 성능을 얻을 수 있었던 것은, 레이블이 없는 방대한 데이터로 사전 훈련된 모델을 가지고, 레이블이 있는 다른 작업에서 추가 훈련과 함께 하이퍼파라미터를 재조정하여 이 모델을 사용하면 성능이 높게 나오는 기존의 사례들을 참고하였기 때문이다. 다른 작업에 대해 파라미터 재조정을 위한 추가 훈련 과정을 파인 튜닝이라고 한다.

<br>

![BERT 구조 이미지](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/BERT%20image/i3.png?raw=true)

BERT의 기본 구조는 트랜스포머의 인코더를 쌓아올린 구조이다. Base 버전에서는 총 12개, Large 버전에서는 총 24개의 인코더가 쌓여있다.

<br>

<br>

#### BERT의 문맥을 반영한 임베딩

<br>

![BERT 입력 이미지](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/BERT%20image/i4.png?raw=true)

BERT의 입력은 임베딩 층을 지난 임베딩 벡터들이다.

<br>

![BERT 임베딩 이미지](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/BERT%20image/i5.png?raw=true)

BERT의 연산을 거친 후의 출력 임베딩은 문장의 모든 문맥을 참고하여 그 정보를 반영한 임베딩이 된다.

위 그림에서 ``[CLS]`` 벡터는 입력 임베딩 당시에는 단순히 임베딩 층을 지난 벡터였지만 BERT를 지나고 나서는 ``[CLS], I, love, you``라는 모든 단어 벡터들을 참고한 후에 문맥 정보를 가진 벡터가 된다.

이러한 연산은 BERT의 모든 층에서 이루어진다. 이 모든 층을 지난 후에 최종적으로 출력 임베딩을 얻게 되는 것이다.

<br>

<br>

#### BERT의 서브워드 토크나이저 : WordPiece

<br>

BERT는 단어보다 더 작은 단위로 쪼개는 서브워드 토크나이저를 사용한다. BERT는 WordPiece 토크나이저를 사용하고 글자로부터 서브워드들을 병합해가는 방식으로 최종 단어 집합을 만든다.

BERT에서 토큰화를 수행하는 방식은 다음과 같다.

```tex
준비물 : 이미 훈련 데이터로부터 만들어진 단어 집합

1. 토큰이 단어 집합에 존재한다.
-> 해당 토큰을 분리하지 않는다.

2. 토큰이 단어 집합에 존재하지 않는다
-> 해당 토큰을 서브워드로 분리한다.
-> 해당 토큰의 첫번째 서브워드를 제외한 나머지 서브워드들은 앞에 '##'를 붙인 것을 토큰으로 한다.
```

예를 들어 embeddings라는 단어가 입력으로 들어왔을 때, BERT의 단어 집합에 해당 단어가 존재하지 않았다고 하면 서브워드 토크나이저는 이 단어를 ``em, ##bed, ##ding, ##s``로 분리한다.

<br>

<br>

#### 포지션 임베딩

<br>

트랜스포머에서는 포지셔널 인코딩이라는 방법을 통해 단어의 위치 정보를 표현했다. BERT는 이와 유사하지만 학습을 통해서 위치 정보를 얻는 포지션 임베딩이라는 방법을 사용해서 단어의 위치 정보를 알아낸다.

![BERT 포지션 임베딩](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/BERT%20image/i6.png?raw=true)

워드 임베딩 층 위에 위치 정보를 위한 임베딩 층을 하나 더 사용한다.

문장의 길이가 4라면 4개의 포지션 임베딩 벡터를 학습시킨다. 그리고 BERT의 입력마다 다음과 같이 포지션 임베딩 벡터를 더해준다.

```tex
첫번째 단어의 임베딩 벡터 + 0번 포지션 임베딩 벡터
두번째 단어의 임베딩 벡터 + 1번 포지션 임베딩 벡터
세번째 단어의 임베딩 벡터 + 2번 포지션 임베딩 벡터
네번째 단어의 임베딩 벡터 + 3번 포지션 임베딩 벡터
```

<br>

<br>

#### BERT의 사전 훈련

<br>

BERT의 사전 훈련 방법은 크게 마스크드 언어 모델, 다음 문장 예측 두 가지로 나뉜다.

<br>

##### 마스크드 언어 모델

마스크드 언어 모델은 영어 시험에서 빈칸 채우기 문제라고 생각하면 이해가 쉽다.

BERT는 사전 훈련을 위해서 인공 신경망의 입력으로 들어가는 입력 텍스트의 15%를 랜덤으로 마스킹(빈 칸으로 만듦)한다. 그리고 인공신경망에게 이 빈칸들 예측하도록 한다.

더 정확하게는 선택된 15%의 단어들은 다시 다음과 같은 비율로 규칙이 적용된다.

```tex
80%의 단어들은 [MASK]로 변경함
10%의 단어들은 랜덤으로 단어가 변경됨
10%의 단어들은 동일하게 둠
```

이렇게 하는 이유는 [MASK]만 사용할 경우 [MASK] 토큰이 파인 튜닝 단계에서는 나타나지 않으므로 사전 학습 단계와 파인 튜닝 단계에서의 불일치가 발생하는 문제때문이다. 이 문제를 완화하기 위해 15%의 단어들의 모든 토큰을 [MASK]로 사용하지 않는 것이다. 이를 전체 단어 관점에서 그래프를 통해 정리하면 다음과 같다.

![MASK 비율 이미지](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/BERT%20image/i7.png?raw=true)

<br>

마스크드 언어 모델의 예시를 통해 더 자세하게 알아보자.

'My dog is cute. he likes playing'이라는 문장에 대해 마스크드 언어 모델을 학습하고자 한다.

전처리와 서브워드 토크나이저에 의해 이 문장은 ``'my', 'dog', 'is', 'cute', 'he', 'likes', 'play', '##ing'``으로 토큰화가 되어 BERT의 입력으로 사용된다.

언어 모델의 학습을 위해 다음과 같이 데이터가 변경되었다고 가정하자.

```tex
'dog' 토큰은 [MASK]로 변경됨
'he'는 랜덤 단어 'king'으로 변경됨
'play'는 변경되지는 않았지만 예측에 사용됨
```

![BERT 마스크드 언어 모델 예시 이미지](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/BERT%20image/i8.png?raw=true)

여기서 학습에는 오직 변경된 토큰의 출력층 벡터만이 사용된다.

BERT는 랜덤 단어 'king'으로 변경된 토큰에 대해서도 원래 단어가 무엇인지, 변경되지 않은 단어 'play'에 대해서도 원래 단어가 무엇인지 예측해야 한다. 'play'는 변경되지 않았지만 BERT 입장에서는 이것이 변경된 단어인지 아닌지 모르므로 마찬가지로 원래 단어를 예측해야 한다.

BERT는 마스크드 언어 모델 외에도 다음 문장 예측이라는 또 다른 태스크를 학습한다.

<br>

##### 다음 문장 예측

BERT는 두 개의 문장을 준 후에 이 문장이 이어지는 문장인지 아닌지를 맞추는 방식으로 훈련한다.

이를 위해 50:50 비율로 실제 이어지는 두 개의 문장과 랜덤으로 이어붙인 두 개의 문장을 주고 훈련시킨다.

![다음 문장 예측 이미지](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/BERT%20image/i9.png?raw=true)

BERT의 여러 개의 문장을 입력으로 넣을 때는 ``[SEP]``라는 특별 토큰을 사용해 문장을 구분한다. 첫번째 문장의 끝에 ``[SEP]``토큰을 넣고, 두번째 문장이 끝나면 역시 ``[SEP]``토큰을 붙여준다.

그리고 이 문장이 실제 이어지는 문장인지 아닌지를 ``[CLS]``토큰의 위치의 출력층에서 이진 분류 문제를 풀도록 한다.

위 그림에서 나타난 것 처럼 마스크드 언어 모델과 다음 문장 예측은 따로 학습하는 것이 아닌 loss를 합하여 학습이 동시에 이루어진다.

BERT가 언어 모델 외에도 다음 문장 예측이라는 태스크를 학습하는 이유는 BERT가 풀고자 하는 태스크 중에는 QA(Question Answering)나 NLI(Natural Language Inference)와 같이 두 문장의 관계를 이해한느 것이 중요한 태스크들이 있기 때문이다.

<br>

#### 세그먼트 임베딩

<br>

BERT는 문장 구분을 위해 세그먼트 임베딩이라는 또 다른 임베딩 층을 사용한다. 첫번째 문장에는 sentence 0 임베딩, 두번째 문장에는 sentence 1 임베딩을 더해주는 방식이며 임베딩 벡터는 두 개만 사용된다.

![세그먼트 임베딩 이미지](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/BERT%20image/i10.png?raw=true)

결론적으로 BERT는 총 3개의 임베딩 층(WordPiece Embedding, Position Embedding, Segment Embedding)이 사용된다.

여기서 두 개의 문장이 들어간다는 표현에서의 문장이 실제 우리가 알고 있는 문장의 단위는 아니다.

[SEP]와 세그먼트 임베딩으로 구분되는 BERT의 입력에서의 두 개의 문장은 실제로는 두 종류의 텍스트, 두 개의 문서일 수도 있다.

BERT가 두 개의 문장을 입력받을 필요가 없는 경우도 있다. 예를 들어 네이버 영화 리뷰 분류나 IMDB 리뷰 분류와 같은 감성 분류 태스크에서는 한 개의 문서에 대해서만 분류를 하는 것이므로, 이 경우에는 BERT의 전체 입력에 Sentence 0 임베딩만을 더해준다.

<br>

#### BERT를 파인 튜닝하기

<br>

이번에는 BERT에 우리가 풀고자 하는 태스크의 데이터를 추가로 학습 시켜서 테스트하는 단계인 파인 튜닝 단계에 대해 알아본다.

<br>

##### 하나의 텍스트에 대한 텍스트 분류 유형

![하나의 텍스트에 대한 텍스트 분류 유형 이미지](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/BERT%20image/i11.png?raw=true)

이 유형은 영화 리뷰 감성 분류와 같이 입력된 문서에 대해서 분류를 하는 유형으로 문서의 시작에 ``[CLS]``라는 토큰을 입력한다. 텍스트 분류 문제를 풀기 위해 ``[CLS]``토큰의 위치의 출력층에서 밀집층 또는 같은 이름으로 완전 연결층(fully-connected layer)이라고 불르는 층들을 추가하여 분류에 대한 예측을 하게 된다.

<br>

##### 하나의 텍스트에 대한 태깅 작업

![하나의 텍스트에 대한 태깅 작업 이미지](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/BERT%20image/i12.png?raw=true)

이 유형은 대표적으로 문장의 각 단어에 품사를 태깅하는 품사 태깅 작업과 개체를 태깅하는 개치명 인식 작업이 있다. 출력층에서는 입력 텍스트의 각 토큰의 위치에 밀집츨을 사용하여 분류에 대한 예측을 하게 된다.

<br>

##### 텍스트의 쌍에 대한 분류 또는 회귀 문제

![텍스트의 쌍에 대한 분류 도는 회귀 문제 이미지](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/BERT%20image/i13.png?raw=true)
BERT는 텍스트의 쌍을 입력으로 받는 태스크도 풀 수 있다. 텍스트의 쌍을 입력으로 받는 대표적인 태스크로 자연어 추론이 있다. 자연어 추론 문제란, 두 문장이 주어졌을 때, 하나의 문장이 다른 문장과 논리적으로 어떤 관계에 있는지를 분류하는 것이다. 유형으로는 모순 관계, 함의 관계, 중립 관계가 있다.

텍스트의 쌍을 입력받는 이러한 태스크의 경우에는 입력 텍스트가 1개가 아니므로, 텍스트 사이에 ``[SEP]``토큰을 집어넣고, Sentence 0 임베딩과 Sentence 1 임베딩이라는 두 종류의 세그먼트 임베딩을 모두 사용하여 문서를 구분한다.

<br>

##### 질의 응답

![질의 응답 이미지](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/BERT%20image/i14.png?raw=true)

텍스트의 쌍을 입력으로 받는 또 다른 태스크로 QA가 있다. BERT로 QA를 풀기 위해서 질문과 본문이라는  두 개의 텍스트의 쌍을 입력한다. 이 데이터셋을 푸는 방법은 질문과 본문을 입력받으면, 본문의 일부분을 추출해서 질문에 답변하는 것이다.

예를 들면 '강우가 떨어지도록 영향을 주는 것은?'이라는 질문이 주어지고

'기상학에서 강우는 대기 수증기가 응결되어 중력의 영향을 받고 떨어지는 것을 의미한다. 강우의 주요 형태는 이슬비, 비, 진눈깨비, 눈, 싸락눈 및 우박이 있다.'라는 본문이 주어졌다고 하자.

이 경우, 정답은 '중력'이 되어야 한다.

<br>

#### 어텐션 마스크

![어텐션 마스크 이미지](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/BERT%20image/i15.png?raw=true)

BERT를 실제로 실습하게 되면 어텐션 마스크라는 시퀀스 입력이 추가로 필요하다. 어텐션 마스크는 BERT가 어텐션 연산을 할 때, 불필요하게 패딩 토큰에 대해서 어텐션을 하지 않도록 실제 단어와 패딩 토큰을 구분할 수 있도록 알려주는 입력이다.

이 값은 0과 1 두 가지 값을 가지는데, 숫자 1은 실제 단어이므로 마스킹을 하지 않는다라는 의미이고 숫자 0은 해당 토큰은 패딩 토큰이므로 마스킹을 한다는 의미이다.

<br>

---

### 센텐스버트(Sentence BERT, SBERT)

<br>

#### BERT의 문장 임베딩

<br>

BERT로부터 문장 벡터를 얻는 방법 중 세 가지에 대해서 알아본다.

<br>

첫번째 방법은 ``[CLS]``토큰의 출력 벡터를 문장 벡터로 간주하는 것이다.

![CLS 문장 벡터](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/BERT%20image/i16.png?raw=true)

BERT로 텍스트 분류 문제를 풀 때, ``[CLS]``토큰의 출력 벡터를 출력층의 입력으로 사용했었다. 이는 ``[CLS]``토큰이 입력된 문장에 대한 총체적 표현이라고 간주하고 있기 때문에 가능했었다.

다시 말해 ``[CLS]``토큰 자체를 입력 문장의 벡터로 간주할 수 있다.

<br>

![BERT 모든 출력 벡터 평균](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/BERT%20image/i17.png?raw=true)

두번째 방법은 ``[CLS]``토큰만을 사용하는 것이 아니라 BERT의 모든 출력 벡터들을 평균내는 것이다.

위 그림에서는 출력 벡터들의 평균을 'pooling'이라고 표현해다. 이를 평균 풀링을 하였다고 표현하기도 한다. 풀링은 평균 풀링만 있는 것이 아니라 맥스 풀링 또한 존재한다.

세번째 방법은 BERT의 각 단어의 출력 벡터들에 대해서 평균 풀링 대신 맥스 풀링을 진행한 벡터를 얻는 것이다.

이 때 평균 풀링을 하느냐와 맥스 풀링을 하느냐에 따라서 해당 문장 벡터가 가지는 의미가 조금 다른데, 평균 풀링으로 얻은 문장 벡터의 경우는 모든 단어의 의미를 반영하는 쪽에 가깝다면, 맥스 풀링으로 얻은 문장 벡터의 경우에는 중요한 단어의 의미를 반영하는 쪽에 가깝다.

<br>

#### SBERT

<br>

SBERT는 기본적으로 BERT의 문장 임베딩의 성능을 우수하게 개선시킨 모델이다. SBERT는 BERT의 문장 임베딩을 응용하여 BERT를 파인 튜닝한다.

<br>

##### 문장 쌍 분류 태스크로 파인 튜닝

SBERT를 학습하는 첫 번째 방법은 문장 쌍 분류 태스크. 대표적으로는 NLI(Natural Language Inferencing) 문제를 푸는 것이다. NLI는 두 개의 문장이 주어지면 수반(entailment) 관계인지, 모순(contracdiction) 관계인지, 중립(neutral) 관계인지를 맞추는 문제이다.

아래는 NLI 데이터의 예시이다.

![NLI 데이터](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/BERT%20image/i18.png?raw=true)

<br>

SBERT는 NLI 데이터를 학습하기 위해 다음과 같은 구조를 가진다.

![NLI 구조](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/BERT%20image/i19.png?raw=true)

문장 A와 문장 B 각각을 BERT의 입력으로 넣고, 평균 풀링 또는 맥스 풀링을 통해 문장 임베딩 벡터를 얻는다. 이를 각각 u와 v라고 한다. 그리고나서 u벡터와 v벡터의 차이 벡터를 구한다(``|u-v|``). 그리고 이 세가지 벡터를 연결한다. 

![NLI 구조 2](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/BERT%20image/i20.png?raw=true)

그리고 연결한 벡터를 출력층으로 보내 다중 클래스 분류 문제를 풀도록 한다.

그 뒤에는 실제값에 해당하는 레이블로부터 오차를 줄이는 방식으로 학습시킨다.

<br>

##### 문장 쌍 회귀 태스크로 파인 튜닝

<br>

SBERT를 학습하는 두번째 방법은 문장 쌍으로 회귀 문제를 푸는 것으로 대표적으로 STS(Semantic Textual Similarity) 문제를 푸는 경우이다. 두 개의 문장으로부터 의미적 유사성을 구하는 문제를 말한다.

아래는 STS 데이터의 예시이다. 여기서 레이블은 두 문장의 유사도로 범위값은 0~5이다.

![STS 데이터](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/BERT%20image/i21.png?raw=true)

<br>

SBERT는 STS 데이터를 학습하기 위해 다음과 같은 구조를 가진다.

![STS 학습 구조](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/BERT%20image/i22.png?raw=true)

문장 A와 문장 B를 각각 BERT의 입력으로 넣고, 평균 풀링 또는 맥스 풀링을 통해서 각각에 대한 문장 임베딩 벡터를 얻는다. 이를 각각 u와 v라고 했을 때 이 두 벡터의 코사인 유사도를 구한다. 그 후 해당 유사도와 레이블 유사도와의 평균제곱오차를 최소화하는 방식으로 학습한다.

<br>

---

# BERT의 문장 임베딩(SBERT)을 이용한 한국어 챗봇

<br>

SBERT를 이용해 문장 임베딩을 얻을 수 있는 패키지인 sentence_transformers를 사용하여 쉽게 한국어 챗봇을 구현할 수 있다.

``pip install sentence_transformers``명령어로 패키지를 설치해준다.

<br>

![j1](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/BERT%20image/j1.png?raw=true)

필요한 패키지들을 불러온다.

<br>

![j2](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/BERT%20image/j2.png?raw=true)

입력한 링크에서 챗봇 데이터를 가져온다.

<br>

![j3](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/BERT%20image/j3.png?raw=true)

문장 임베딩을 얻기 위해 사전 훈련된 BERT를 로드한다.

로드한 모델은 한국어를 포함해서 학습된 다국어 모델이다.

모델에 대한 리스트는 [이 링크](https://huggingface.co/models?library=sentence-transformers)에서 확인 가능하다.

<br>

![j4](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/BERT%20image/j4.png?raw=true)

가져온 데이터에서 모든 질문열에 대해 문장 임베딩 값을 구하고 새로운 embedding열에 저장한다.

<br>

![j5](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/BERT%20image/j5.png?raw=true)

두 벡터의 코사인 유사도를 구하는 함수를 정의한다.

<br>

![j6](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/BERT%20image/j6.png?raw=true)

코사인 유사도 함수를 사용해 임의의 질문이 입력으로 들어오면 해당 질문의 문장 임베딩 값과 챗봇 데이터의 모든 질문 샘플들에 대한 문장 임베딩 값들을 비교하여 유사도가 가장 높은 질문 샘플을 찾아 짝이 되는 답변 샘플을 리턴하는 함수를 만든다.

<br>

![j7](https://github.com/Cheolyong-Kim/TIL/blob/master/%EB%94%A5%EB%9F%AC%EB%8B%9D%EA%B8%B0%EB%B0%98%20%EB%B9%84%EC%A0%95%ED%98%95%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EB%B6%84%EC%84%9D/BERT%20image/j7.png?raw=true)

챗봇을 테스트해본 결과이다.

<br>

---

### 참고

<br>

이 문서에서는 BERT에 관해서 간단하게 정리하고 자세한 내용은 살펴보지 않았다.

자세한 설명과 BERT를 통한 예제 등을 [이 링크](https://wikidocs.net/109251)
