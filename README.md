# Covid-19 환자의 중증도 분석 모델의 불활실성 분석

## 연구 배경
>딥러닝은 머신 러닝에서 가장 많이 쓰이는 도구 중 하나이지만, 회귀 모델, 분류 모델 문제에서의 딥러닝은 모델의 불확실성을 잡지 못한다. 또한, 심층 신경망은 그 능력을 입증했지만 예측의 신뢰성을 추정하는 것은 여전히 어렵다. 불확실성(Uncertainty)은 딥러닝 분야에서는 지금까지 주목받지 못했으나 굉장히 중요한 주제이다.
>예를 들어, 자율 주행이나 의료 분야에서 불확실성을 파악하지 못하면 큰 사고가 일어날 수 있고, 실제사례로 구글 “포토”에서 아프리카 사람을 고릴라로 분류하는 이슈가 있었다. 이는 불확실성을 고려하지 않은 이미지 분류 모델이었기 때문이다. 또한, 본 연구에서 진행하는 Covid-19환자 중증도 분석과 같은 의료 모델은 인공지능 모델이 큰 영향을 미친다. 그렇기에 불확실성을 고려한 모델은 중요하다

## 연구 목표
>유용한 예측을 생성하는 심층 신경망의 능력은 이제 매우 분명해졌지만 이러한 예측의 신뢰성을 평가하는 것은 여전히 어려운 일이다. 보통의 딥러닝 모델은 틀리면 틀렸지 “모른다”고는 말하지 못하고 확신에 차서 내놓은 예측 값이 틀렸을 경우 문제가 생길 수 있다. 따라서, 
>* 첫번째 목표는 Ensemble과 MC-Dropout 방식을 이용하여 Covid-19 중증도 모델의 불확실성 분석을 하는 것이다.
>* 두번째 목표는 Label Distribution Learning을 사용하여 불확실성 분석을 하는 것이다.Label Distribution Learning은 예측하여 나온 결과값이 일정한 분포를 가지도록 한다. 이 데이터 상의 중증도 점수에 크게 의존하여 모델을 강하게 학습하면, 오히려 학습된 모델이 편향될 수 있는 문제가 발생할 가능성이 있다.
>* 세번째 목표는 Covid-19 환자의 중증도 분석 모델을 위의 세가지 방법을 이용하여 불확실성 추정 값을 비교 분석하여 가장 성능이 좋은 모델을 구현하는 것이다.

## 연구 방법 및 연구 결과

![model](https://user-images.githubusercontent.com/31675698/146669875-f3dfcd3d-3740-4e5e-81f5-69f977d7205d.png)


Brixia Dataset을 학습을 기반으로 COVID-19 환자의 흉부 X-ray 영상이 입력(input)값으로 주어졌을때, 흉부 전체의 이상을 감지하여 0-18 범위의 중증도 점수를 출력(Output)하는 Regression 모델을 사용한다.

### 1. Data Random Ensemble
딥러닝 모델에 같은 조건에서 학습 데이터만 랜덤하게 주어 5개의 모델을 학습시키고 각 모델의 MAE값(성능)을 구한다. 그리고 각 테스트 이미지에 대해 평균 예측값과 실제값의 오차를 구한 후 오차의 평균을 구해 Data Random Ensemble 방법을 적용한 모델의 성능을 측정한다.
![en1_mae](https://user-images.githubusercontent.com/31675698/146670125-0801d6ac-33cb-4418-a2bb-7e5513ea1b26.png)

![en1_sample1](https://user-images.githubusercontent.com/31675698/146670137-481c5e46-0bdb-49ce-8b29-19421b96e291.png)

![en1_sample2](https://user-images.githubusercontent.com/31675698/146670138-54b3da7d-5124-47c8-bd90-3ab8acb53dcf.png)

위의 두 그림은 각 테스트 샘플에 대한 결과를 정리한 표이다. 오차와 불확실성이 모두 높게 나오는 이미지들과 모두 낮게 나오는 이미지를 각각 정리한 표이다. 오차와 불확실성이 모두 낮은 이미지들이 대체로 선명함을 알 수 있다.

### 2. Mini-Batch Random Ensemble
![mini_mae](https://user-images.githubusercontent.com/31675698/146670143-b7f11097-fb4d-4e3b-8c5f-dc403f77aa28.png)


![mini](https://user-images.githubusercontent.com/31675698/146670157-e01b8c79-03ac-4f37-b432-c43f43aa58b1.png)

Mini Batch Random Ensemble은 각 모델의 Mini-Batch Size를 8, 16, 32, 64, 128로 다르게 하여 학습을 진행하고 테스트를 진행한다. 각 모델의 성능을 구하고, 위의 방법과 동일한 방법으로 Mini-Bach Random Ensemble 방법을 적용한 모델의 성능을 측정한다.
또한, 일부 테스트 이미지에 대한 결과를 정리한 표이다. 이 또한, 오차와 불확실성이 모두 낮은 샘플이 더 선명함을 알 수 있다.

### 3. MC-Dropout
![mc](https://user-images.githubusercontent.com/31675698/146670158-8f1218eb-026a-4fd9-a2ae-4ab2aa278f56.png)

앞의 두가지 방법과 동일하게 학습과 테스트를 진행하여 나온 결과이다. 앞의 방식보다 성능이 더 떨어짐을 알 수 있다. 테스트 이미지에 대한 결과는 위의 두 방식과 비슷한 양상이 나왔다.

## 연구 결론
![result](https://user-images.githubusercontent.com/31675698/146670159-75f04067-f0cf-49d7-96a8-d3900efe87b9.png)

본 연구에서는 데이터 랜덤 앙상블, 미니배치 랜덤 앙상블, Mc-dropout의 세가지 방법을 사용하여 불확실성을 측정하고 분석했다.
<br/>세가지 방법 중 성능이 가장 좋았던 방법은 첫번째 방법인 데이터 랜덤 앙상블 방법이다.
<br/>그리고 각 테스트 샘플에 대해서 
* 오차가 큰 경우 
* 이미지가 선명하지 않은 경우

불확실성이 크게 나타나는 경향이 있다.

## Reference

연구 코드는 [https://github.com/CAMP-eXplain-AI/CheXplain-IBA](https://github.com/CAMP-eXplain-AI/CheXplain-IBA)를 기반으로 작성하였습니다.


