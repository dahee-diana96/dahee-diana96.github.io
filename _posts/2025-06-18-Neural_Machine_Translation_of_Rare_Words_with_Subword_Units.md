---
layout: single
title:  "Neural Machine Translation of Rare Words with Subword Units"
categories:
  - Deep Learning
tags:
  - LLM
  - 논문리뷰
classes: wide
---

```
발표날짜: 2016년 6월 10일
저자: (School of Informatics, University of Edinburgh) Rico Sennrich, Barry Haddow, Alexandra Birch
논문 링크: https://arxiv.org/abs/1804.10959
깃헙 링크: https://github.com/rsennrich/subword-nmt
```


# Abstract
> “[…] we introduce a simpler and more effective approach, making the NMT model capable of open-vocabulary translation by encoding rare and unknown words as sequences of subword units.”

= 희귀하거나 사전에 없는 단어들을 더 작은 **'서브워드' 단위로 인코딩**하여 **신경망 번역(NMT) 모델 스스로 어휘 제한이 없는 번역이 가능하도록** 만들기

# 기존 있었던 문제점
언어 모델이 보지 않았던 단어들에 대해 unknown 토큰 `<UNK>` 으로 처리되었다. (고유명사, 신조어, etc.)
&rarr; 이로 인해 **정보 손실이 발생**하곤 했다.

일반적으로 많은 언어들은 **<u>합성어</u>**가 정말 많이 존재하기 때문에 **일반 단어보다는 더 작은 단위로 쪼개질 필요**가 있었다.

**예시**
- Smartwatch → “smart” + “watch”
- Overfitting → “over” + “fitting”
- 인생네컷 → “인생” + “네컷”


# 이 논문의 두 가지 메인 기여도
- **희귀 단어를 포함한 모든 단어를 '서브워드(subword)' 단위로 표현함**으로써, 어휘 수에 제한받지 않는(openvocabulary)
신경망 기계 번역이 가능함을 보여주었다. → 대규모 어휘 사전을 구축하거나 별도의 보조 사전(back-off dictionary)을 사용하는 기존 방식보다 더 단순하고 효과적
- 데이터 압축 알고리즘인 '바이트 페어 인코딩(BPE)'을 단어를 의미 있는 조각으로 나누는 '어휘 분절' 작업에 적용했다. BPE를 사용하면 가변 길이의 문자 조합으로 이루어진 고정된 크기의 어휘 사전만으로도 사실상 무한한 단어를 표현할 수 있다.

![](/assets/images/image 1.png)

- `def get_status(vocab)` : 현재의 vocab에서 문자쌍(character pair)의 빈도를 계산한다.
- `def merge_vocab(pair, v_in)` : 해당 쌍을 찾아 병합한 문자열로 대체한다.
- `</w>` : 단어 끝을 의미하는 토큰

```
vocab = {
'l o w </w>': 5,
'l o w e r </w>': 2,
'n e w e s t </w>': 6,
'w i d e s t </w>': 3
}
```
어느 문단에서 위 단어가 오른쪽 횟수만큼 등장했다고 가정한다면, 각 단어에서 2개씩 인접한 문자를 찾아 전체 빈도를 센다.

만약

```
{ 
    ('e', 's'): 9, 
    ('s', 't'): 9, 
    ('n', 'e'): 6, 
    ('e', 'w'): 6, 
    ('l', 'o'): 7, 
    ('o', 'w'): 7, 
    ('w', '</w>'): 5, 
    ...
}
```
위와 같은 결과가 나왔다면, 'e'와 's'가 병합될 것이고, 점점 for loop를 더 돌 수록 ‘est’로 합쳐질 것이다.

# Experiment

![](/assets/images/image 2.png)

**[지표]**
- **BLEU**: 번역 품질 지표. 높을 수록 좋음.
- **chrF3**: 번역 품질 지표 (문자 기반). BLEU보다 짧은 단어에 민감하고 희귀 단어에 강한 문자 단위의 평가 지표.
- **Unigram F1**: 단어 단위 정밀도와 재현율의 조화 평균. 실제 번역된 단어와 정답 번역에서의 단어가 얼마나 겹치는지를 보는 지표.
    - all: 전체 단어 기준
    - rare: 학습 데이터에서 top 50,000에 포함되지 않은 희귀 단어
    - OOV(out of vocabulary): 학습 데이터에 없는 단어

⇒ BPE를 적용한 버전들이 보유한 단어 수가 적었음에도 불구하고 많은 지표들에 높은 점수를 기록했다.

![](/assets/images/image 3.png)

⇒ BPE가 보유하고 있는 단어의 종류(types)가 이전 방법들보다 대폭 줄였음에도 불구하고 unknown 발생 횟수가 상당히 줄어들었다.
⇒ 토큰 수가 좀 많은 게 함정.

# Analysis

![](/assets/images/image 4.png)

![](/assets/images/image 5.png)

- x축: 단어의 빈도 수 (왼쪽: 자주 등장하는 단어 / 오른쪽: 희귀하거나 OOV)
- y축: unigram F1 점수 (= 단어 단위 정밀도와 재현율의 조화 평균)

⇒ 단어가 희귀하거나 OOV임에도 불구하고 BPE-J90k 같은 경우에는 다른 방법들보다 제법 robust한 것으로 보인다

# 핵심
```
BPE 알고리즘을 언어 모델에 적용해 단어보다 작은 단위인 subword로 쪼개어 OOV(Out-of-Vocabulary) 이슈를 대폭 줄였다.
```