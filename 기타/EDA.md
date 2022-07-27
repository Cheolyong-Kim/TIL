# EDA(Easy Data Augmentation)
---
<br>
데이터 불균형 문제를 해결하기 위해 사용하는 방법은 여러가지가 있다.
<br>
<br>
대표적으로 언더샘플링, 오버샘플링이 떠오르는데, 
이는 데이터의 다양성을 줄이거나 중복 데이터가 많이 발생해 과적합이 발생하는 등의 자잘한 문제가 생긴다.
<br>
하지만 SMOTE와 같이 원래의 샘플링의 단점을 어느정도 없애주는 기술들이 존재한다. 
<br>
<br>
이미지 데이터의 경우에는 회전을 시키거나 배경의 색을 바꾸는 방법으로 쉽게 데이터를 늘릴 수 있다.
<br>
<br>
하지만 텍스트 데이터의 경우에는 위의 방법들을 사용하기에 쉽지않다.
<br>
문장에서는 한 단어만 바뀌어도 문장의 뜻이 달라질 수 있기 때문에 이미지처럼 쉽게 데이터를 증강시킬 수 없다.
<br>
SMOTE같은 기술을 사용하자니 텍스트데이터라 사용하기 힘들고
<br>
임베딩하여 SMOTE를 적용해도 텍스트의 왜곡이 생긴다.
<br>
<br>
이렇듯 불균형한 텍스트 데이터의 처리는 곤란한 점이 많은데
<br>
이 문서에서 소개할 EDA가 곤란한 점을 조금은 해소시켜준다.
<br>

---
### EDA 간단 소개
<br>
EDA는 Easy Data Augmentation의 줄임말이다.
<br>
Easy가 단어에 들어있는 만큼 쉽게 데이터를 증강시킬 수 있다.
<br>
EDA는 네가지 방법으로 텍스트 데이터를 늘린다.
<br>
<br>
1. SR(Synonym Replacement) : 특정 단어를 유의어로 교체<br>
2. RI(Random Insertion) : 임의의 단어를 삽입<br>
3. RS(Random Swap) : 문장 내 임의의 두 단어의 위치를 바꿈<br>
4. Rd(Random Deletion) : 임의의 단어 삭제<br>
<br>
위 방법을 통해 실제로 CNN과 RNN에서 성능을 향상시키는 것을 논문에서 보여주고 있다. 특히 데이터가 적은 경우에 큰 향상폭을 보인다고 한다.
<br>

---
### 들어가기 전
<br>
이전에 제안된 자연어 처리 분야의 데이터 증강 기법에는
<br>
<br>
1. 문장을 프랑스어로 번역하고 다시 영어로 번역하여 새로운 데이터를 얻음<br>
2. 데이터에 노이즈를 가볍게 주는 방식<br>
3. 유의어로 교체해주는 예측 언어 모델<br>
<br>
이렇게 대표적으로 세가지가 있었다.
<br>
유용하기는 한데, 성능 대비 구현 비용이 높아 잘 사용하지 않았다고 한다.
<br>

---
### EDA
<br>
EDA의 기법들에는 위에서 설명했듯이 네가지가 있다.
<br>
<br>
1. 유의어로 교체(SR) : 문장에서 랜덤으로 불용어가 아닌 n개의 단어를 선택하여 임의로 선택한 동의어들 중 하나로 바꾸는 기법<br>
2. 랜덤 삽입(RI) : 문장 내에서 불용어를 제외한 나머지 단어들 중에서, 랜덤으로 선택한 단어의 동의어를 임의로 정한다. 그리고 동의어를 문장 내 임의의 자리에 넣는 것을 n번 반복한다.<br>
3. 랜덤 교체(RS) : 무작위로 문장 내에서 두 단어를 선택하고 위치를 바꾼다. 이것을 n번 반복<br>
4. 랜덤 삭제(RD) : 확률 p를 통해 문장 내에 있는 각 단어들을 랜덤하게 삭제한다.<br>
<br>
긴 문장은 짧은 문장보다 단어 수가 많기 때문에 원래의 라벨을 유지하면서 노이즈에 상대적으로 영향을 덜 받는다.<br>
대신에 공식 n=al 과 함께 문장 길이 l 을 기준으로 SR, RI, RS에 대해 바뀐 단어의 수 n을 변화시킨다.
<br>
여기서 a는 단어의 백분율이 변경되었음을 나타내는 매개변수이다.
(RD에서의 a는 p로 표기함)
<br>
<br>

![i1](https://github.com/Cheolyong-Kim/TIL/blob/master/%EA%B8%B0%ED%83%80/EDA%20image/i1.jpg?raw=true)
<br>
위 이미지는 각 기법들에 대한 실제 예시이다.
<br>
각각의 원래의 문장에 대해 ``n_aug``개의 증강된 문장을 만든다.
<br>

---
### 어느 정도의 성능 향상을 보이는가?
<br>
<br>

![i2](https://github.com/Cheolyong-Kim/TIL/blob/master/%EA%B8%B0%ED%83%80/EDA%20image/i2.png?raw=true)
<br>
각 학습셋 크기에 대해 5개의 데이터셋에 걸쳐 EDA를 포함하지 않고 RNN과 CNN모델을 실행했다고 한다.
<br>
위 표에서 보이는 것 처럼 전체 데이터셋을 사용할 경우 0.8%의 성능향상이 보이고, 학습 데이터가 500개인 경우에서 3.0%의 성능향상을 보인다.
<br>
<br>

---
### 예제코드
<br>
<br>

```python
import random
import pickle
import re

wordnet = {}
with open("wordnet.pickle", "rb") as f:
	wordnet = pickle.load(f)


# 한글만 남기고 나머지는 삭제
def get_only_hangul(line):
	parseText= re.compile('/ ^[ㄱ-ㅎㅏ-ㅣ가-힣]*$/').sub('',line)

	return parseText



########################################################################
# Synonym replacement
# Replace n words in the sentence with synonyms from wordnet
########################################################################
def synonym_replacement(words, n):
	new_words = words.copy()
	random_word_list = list(set([word for word in words]))
	random.shuffle(random_word_list)
	num_replaced = 0
	for random_word in random_word_list:
		synonyms = get_synonyms(random_word)
		if len(synonyms) >= 1:
			synonym = random.choice(list(synonyms))
			new_words = [synonym if word == random_word else word for word in new_words]
			num_replaced += 1
		if num_replaced >= n:
			break

	if len(new_words) != 0:
		sentence = ' '.join(new_words)
		new_words = sentence.split(" ")

	else:
		new_words = ""

	return new_words


def get_synonyms(word):
	synomyms = []

	try:
		for syn in wordnet[word]:
			for s in syn:
				synomyms.append(s)
	except:
		pass

	return synomyms

########################################################################
# Random deletion
# Randomly delete words from the sentence with probability p
########################################################################
def random_deletion(words, p):
	if len(words) == 1:
		return words

	new_words = []
	for word in words:
		r = random.uniform(0, 1)
		if r > p:
			new_words.append(word)

	if len(new_words) == 0:
		rand_int = random.randint(0, len(words)-1)
		return [words[rand_int]]

	return new_words

########################################################################
# Random swap
# Randomly swap two words in the sentence n times
########################################################################
def random_swap(words, n):
	new_words = words.copy()
	for _ in range(n):
		new_words = swap_word(new_words)

	return new_words

def swap_word(new_words):
	random_idx_1 = random.randint(0, len(new_words)-1)
	random_idx_2 = random_idx_1
	counter = 0

	while random_idx_2 == random_idx_1:
		random_idx_2 = random.randint(0, len(new_words)-1)
		counter += 1
		if counter > 3:
			return new_words

	new_words[random_idx_1], new_words[random_idx_2] = new_words[random_idx_2], new_words[random_idx_1]
	return new_words

########################################################################
# Random insertion
# Randomly insert n words into the sentence
########################################################################
def random_insertion(words, n):
	new_words = words.copy()
	for _ in range(n):
		add_word(new_words)
	
	return new_words


def add_word(new_words):
	synonyms = []
	counter = 0
	while len(synonyms) < 1:
		if len(new_words) >= 1:
			random_word = new_words[random.randint(0, len(new_words)-1)]
			synonyms = get_synonyms(random_word)
			counter += 1
		else:
			random_word = ""

		if counter >= 10:
			return
		
	random_synonym = synonyms[0]
	random_idx = random.randint(0, len(new_words)-1)
	new_words.insert(random_idx, random_synonym)



def EDA(sentence, alpha_sr=0.1, alpha_ri=0.1, alpha_rs=0.1, p_rd=0.1, num_aug=9):
	sentence = get_only_hangul(sentence)
	words = sentence.split(' ')
	words = [word for word in words if word is not ""]
	num_words = len(words)

	augmented_sentences = []
	num_new_per_technique = int(num_aug/4) + 1

	n_sr = max(1, int(alpha_sr*num_words))
	n_ri = max(1, int(alpha_ri*num_words))
	n_rs = max(1, int(alpha_rs*num_words))

	# sr
	for _ in range(num_new_per_technique):
		a_words = synonym_replacement(words, n_sr)
		augmented_sentences.append(' '.join(a_words))

	# ri
	for _ in range(num_new_per_technique):
		a_words = random_insertion(words, n_ri)
		augmented_sentences.append(' '.join(a_words))

	# rs
	for _ in range(num_new_per_technique):
		a_words = random_swap(words, n_rs)
		augmented_sentences.append(" ".join(a_words))

	# rd
	for _ in range(num_new_per_technique):
		a_words = random_deletion(words, p_rd)
		augmented_sentences.append(" ".join(a_words))

	augmented_sentences = [get_only_hangul(sentence) for sentence in augmented_sentences]
	random.shuffle(augmented_sentences)

	if num_aug >= 1:
		augmented_sentences = augmented_sentences[:num_aug]
	else:
		keep_prob = num_aug / len(augmented_sentences)
		augmented_sentences = [s for s in augmented_sentences if random.uniform(0, 1) < keep_prob]

	augmented_sentences.append(sentence)

	return augmented_sentences
```
<br>
<br>

```python
wordnet = {}
with open("wordnet.pickle", "rb") as f:
	wordnet = pickle.load(f)
```
wordnet은 KAIST에서 만든 Korean WordNet(KWN)을 사용
<br>
<br>

```python
def get_only_hangul(line):
	parseText= re.compile('/ ^[ㄱ-ㅎㅏ-ㅣ가-힣]*$/').sub('',line)

	return parseText
```
한글만 남기는 함수
<br>
<br>

```python
def synonym_replacement(words, n):
	new_words = words.copy()
	random_word_list = list(set([word for word in words]))
	random.shuffle(random_word_list)
	num_replaced = 0
	for random_word in random_word_list:
		synonyms = get_synonyms(random_word)
		if len(synonyms) >= 1:
			synonym = random.choice(list(synonyms))
			new_words = [synonym if word == random_word else word for word in new_words]
			num_replaced += 1
		if num_replaced >= n:
			break

	if len(new_words) != 0:
		sentence = ' '.join(new_words)
		new_words = sentence.split(" ")

	else:
		new_words = ""

	return new_words


def get_synonyms(word):
	synomyms = []

	try:
		for syn in wordnet[word]:
			for s in syn:
				synomyms.append(s)
	except:
		pass

	return 
```
SR에 해당하는 함수, 입력된 단어와 유사한 단어를 WordNet에서 찾아 교체
<br>
<br>

```python
def random_deletion(words, p):
	if len(words) == 1:
		return words

	new_words = []
	for word in words:
		r = random.uniform(0, 1)
		if r > p:
			new_words.append(word)

	if len(new_words) == 0:
		rand_int = random.randint(0, len(words)-1)
		return [words[rand_int]]

	return 
```
RD에 해당하는 함수, 랜덤으로 단어를 삭제
<br>
<br>

```python
def random_swap(words, n):
	new_words = words.copy()
	for _ in range(n):
		new_words = swap_word(new_words)

	return new_words

def swap_word(new_words):
	random_idx_1 = random.randint(0, len(new_words)-1)
	random_idx_2 = random_idx_1
	counter = 0

	while random_idx_2 == random_idx_1:
		random_idx_2 = random.randint(0, len(new_words)-1)
		counter += 1
		if counter > 3:
			return new_words

	new_words[random_idx_1], new_words[random_idx_2] = new_words[random_idx_2], new_words[random_idx_1]
	return new_words
```
RS에 해당하는 함수, 단어의 위치를 교체
<br>
<br>

```python
def random_insertion(words, n):
	new_words = words.copy()
	for _ in range(n):
		add_word(new_words)
	
	return new_words


def add_word(new_words):
	synonyms = []
	counter = 0
	while len(synonyms) < 1:
		if len(new_words) >= 1:
			random_word = new_words[random.randint(0, len(new_words)-1)]
			synonyms = get_synonyms(random_word)
			counter += 1
		else:
			random_word = ""

		if counter >= 10:
			return
		
	random_synonym = synonyms[0]
	random_idx = random.randint(0, len(new_words)-1)
	new_words.insert(random_idx, random_synonym)
```
RI에 해당하는 함수, 임의의 단어를 삽입
<br>
<br>

```python
def EDA(sentence, alpha_sr=0.1, alpha_ri=0.1, alpha_rs=0.1, p_rd=0.1, num_aug=9):
	sentence = get_only_hangul(sentence)
	words = sentence.split(' ')
	words = [word for word in words if word is not ""]
	num_words = len(words)

	augmented_sentences = []
	num_new_per_technique = int(num_aug/4) + 1

	n_sr = max(1, int(alpha_sr*num_words))
	n_ri = max(1, int(alpha_ri*num_words))
	n_rs = max(1, int(alpha_rs*num_words))

	# sr
	for _ in range(num_new_per_technique):
		a_words = synonym_replacement(words, n_sr)
		augmented_sentences.append(' '.join(a_words))

	# ri
	for _ in range(num_new_per_technique):
		a_words = random_insertion(words, n_ri)
		augmented_sentences.append(' '.join(a_words))

	# rs
	for _ in range(num_new_per_technique):
		a_words = random_swap(words, n_rs)
		augmented_sentences.append(" ".join(a_words))

	# rd
	for _ in range(num_new_per_technique):
		a_words = random_deletion(words, p_rd)
		augmented_sentences.append(" ".join(a_words))

	augmented_sentences = [get_only_hangul(sentence) for sentence in augmented_sentences]
	random.shuffle(augmented_sentences)

	if num_aug >= 1:
		augmented_sentences = augmented_sentences[:num_aug]
	else:
		keep_prob = num_aug / len(augmented_sentences)
		augmented_sentences = [s for s in augmented_sentences if random.uniform(0, 1) < keep_prob]

	augmented_sentences.append(sentence)

	return augmented_sentences
```
EDA동작함수, 문장과 파라미터를 입력하면 EDA가 적용된 문장이 출력됨.
<br>
<br>

---
### 기타
<br>
<br>
이렇게 생성한 문장들이 기존 문장의 라벨과 다를 수 있지 않을까라는 질문이 생길 수 있다.
<br>
논문에서는 실험과 데이터시각화를 통해 EDA로 증강된 문장들이 대부분 원래 문장의 라벨값을 가지고 있다는 것을 보여준다.
<br>
<br>
위에서 EDA는 데이터가 적은 경우에 좋은 성능을 보여준다고 했다.
<br>
데이터 수에 따라 파라미터를 어떻게 조정해야 좋은 결과값이 나올까?
<br>
논문에서 직접 데이터 수에 따른 파라미터 값을 추천해주고 있다.
<br>
<br>

![i3](https://github.com/Cheolyong-Kim/TIL/blob/master/%EA%B8%B0%ED%83%80/EDA%20image/i3.png?raw=true)
<br>
<br>

---
### 참고 링크
<br>
<br>

[EDA논문](https://arxiv.org/pdf/1901.11196.pdf)
<br>
[EDA논문 정리](https://catsirup.github.io/ai/2020/04/21/nlp_data_argumentation.html)
<br>
[KorEDA github](https://github.com/catSirup/KorEDA/blob/master/eda.py)