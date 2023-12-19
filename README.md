# RoBERTa를 활용한 가짜 뉴스 탐지(Detect fake news using RoBERTa)
## 1.요약 
본 프로젝트는 가짜뉴스 탐지를 위한 다양한 모델을 실험하고, 그 중에서 최적의 성능을 보인 Roberta 모델을 활용하였다. 연구의 핵심 목표는 뉴스 기사의 진위 여부를 판별하기 위한 효과적인 모델을 개발하고 이의 성능을 평가하는 것이었다. 이를 위해 RNN, LSTM, GRU 등 다양한 모델을 실험하고, Roberta 모델의 우수한 성능을 확인하여 최종적으로 이를 선택하게 되었다.

## 2.서론
가짜뉴스는 현대 사회에서 정보의 신뢰성을 훼손시키고, 사람들의 판단력을 혼란스럽게 만드는 중대한 문제 중 하나이다. 이러한 가짜뉴스의 증가는 신속한 정보 전달과 소셜 미디어의 보급으로 더욱 가속화되고 있다. 본 연구에서는 이러한 문제를 극복하기 위해 텍스트 분석을 기반으로 뉴스 기사의 진위 여부를 판별하는 딥러닝 모델을 연구한다. 

## 3.데이터 소개
본 연구에서 사용한 데이터는 세가지 유형으로 구분된다. kaggle의 Fake and real news dataset, PolitiFact.com의 LIAR Dataset, 다양한 소셜 미디어 게시물로 구성된 COVID19 Fake News Detection in English 이다. 세부내용은 아래와 같다.

1. title, text, subject, date를 포함한 약 2만행의 csv 파일

2. truthfulness, subject, context/venue, speaker, state, party, and prior history 으로 구성된 12,836 행의 tsv 파일

3. 다양한 소셜 미디어 플랫폼으로 구성된 8560 행의 csv 파일

위 데이터셋은 politics, world news, medical 등 다양한 카테고리로의 뉴스 기사들로 구성되어 있다.

## 4.방법
1.데이터 전처리
다음과 같이 데이터를 전처리 한다.

1. 텍스트에 자주 쓰이지만 중요하지 않은 A, The, I 등의 stop words를 제거한다.
2. 자연어 처리시 노이즈로 작용할 수 있는 대문자를 소문자로 변환한다.
3. 모델의 INPUT으로 한 번에 들어갈 수 있게 제목과 텍스트를 합친다.
4. 분류를 방해하는 너무 짧은 길이의 텍스트와 토큰 길이 제한을 유발하는 너무 긴 텍스트를 제거한다.
5. 전처리한 3개의 데이터셋을 하나의 dataframe으로 합쳐, ‘clean_message’라는 새로운 dataframe을 만든다. 불필요한 표현들이 없어진 것을 확인한다.
6. 전체 70810개의 텍스트 데이터를 학습:시험=7:3의 비율로 나누어 훈련시킨다. 

2. 모델 학습

텍스트 데이터의 sequential한 특성과 문맥을 고려하여 뉴스의 진위 여부를 판단하기 위해 여러가지 딥러닝 기반 모델을 학습하였다. RNN(Recurrent Neural Network), LSTM(Long Short-Term Memory), GRU(Gated Recurrent Unit), 그리고 RoBERTa (Robustly optimized BERT approach) 총 4가지의 모델이 사용되었다. 프로젝트 초기 단계에서는 RNN, LSTM, GRU 모델을 사용하였지만, 최종적으로 RoBERTa 모델을 통해 학습을 진행하였다.

또한 RoBERTa는 뉴스 콘텐츠의 복잡하고 다양한 문맥을 효과적으로 분석할 수 있는 능력을 가지고 있어, 더 높은 정확도와 신뢰성을 제공하였다.

RNN(Recurren Neural Network)
시퀀스 데이터에 효과적이다.
시간적 연속성을 가지는 데이터를 처리하는데  적합하다.
Vanishing Gradient 문제가 발생한다.
병렬 처리 능력이 제한적이다.
![image](https://github.com/dayunss/st/assets/111956429/4b67d622-a90f-4348-96ad-b121e4b84bdf)

LSTM (Long Short-Term Memory) 
RNN의 Gradient 소실 문제를 해결한다.
복잡한 시퀀스 데이터에 효과적이다.
복잡한 구조로 인해 계산 비용이 높다.
모델의 크기가 크기 때문에 학습시간이 오래 걸리고 많은 메모리를 필요로 한다.
![image](https://github.com/dayunss/st/assets/111956429/2e7e5204-aad2-4f7d-bb62-1f1e60465dac)

GRU (Gated Recurrent Unit)
LSTM보다 구조가 간단하여 계산 비용이 적게 든다.
LSTM에 비해 약간의 성능 저하가 있을 수 있다.
여전히 병렬 처리에는 제한이 있다. 
![image](https://github.com/dayunss/st/assets/111956429/43706482-ac75-4b6e-91a3-04d951b84564)

## ->
RoBERTa (Robustly Optimized BERT Pretraining Approach) 

RoBERTa 모델은 BERT의 변형으로, 더 큰 데이터셋과 긴 학습 과정을 통해 성능을 개선한 모델로, 트랜스포머 기반의 아키텍처를 사용하여 문맥에 더 민감하고 깊이 있는 언어 이해를 가능하게 한다. 이때 Dynamic Masking, Full sentence without NSP, Large mini-batches, Larger byte-level BPE 등의 기법이 사용되었다.
![image](https://github.com/dayunss/st/assets/111956429/bd9d12b3-8483-41ff-a7ca-20615ae3a710)
![image](https://github.com/dayunss/st/assets/111956429/3888814e-a973-4f10-a8ca-a9974d4eb8bc)



3. 평가 지표 선정
![image](https://github.com/dayunss/st/assets/111956429/3907f8c0-dd22-4ab4-be35-2eb6c0153b3f)
클래스의 불균형에 민감하지 않은 MCC(Matthews Correlation Coefficient)를 사용하여 평가되지만, 본 프로젝트에서 사용된 데이터셋은 뚜렷한 분류가 가능하여 ACC가 더 적합한 지표로 판단되었다.  

## 5.결론

![image](https://github.com/dayunss/st/assets/111956429/c86c5eab-7a88-42d2-92d0-90f6d37d8c2a)

![image](https://github.com/dayunss/st/assets/111956429/00b36fa7-4d9d-4080-a051-300401c1ffa1)

정확도 면에서 RoBERTa 모델은 0.9820으로 가장 높게 평가되었으며, 일반적으로 성능이 낮다고 여겨지는 RNN 모델도 0.9237로 상당히 높은 정확도를 보였다. 이러한 결과는 사용된 데이터셋 자체가 참과 거짓을 구분하는 데 있어 상대적으로 쉬운 구조를 가지고 있기 때문이다.

가짜 뉴스는 명확하게 참/거짓으로 나뉘지 않는다. 제목과 본문의 일치도가 낮은 경우, 과장된 내용이 있는 경우 등 가짜 뉴스의 범주는 굉장히 넓다. 이러한 복잡한 문제는 이진 분류보다 다중 분류를 이용한다면 더 개선된 결과를 도출할 것으로 기대된다.






