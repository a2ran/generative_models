생성 모델(Generative Models)은 판별 모델(Discriminative Models)과 달리 입력 데이터 각각의 분포를 학습하여, 데이터에 내재된 복잡한 관계를 포착합니다. 생성 모델은 데이터 각각에 고유한 확률 분포 $P(X)$를 배정합니다.

<img src="https://wikidocs.net/images/page/228787/generative0.png" width="70%">


여기서 $P(X)$는 해당 데이터가 발생할 확률 분포를 의미합니다. 하지만, 저희가 구하고자 하는 이미지, 음악, 텍스트 등 고차원 데이터의 확률 분포 $P_{data}(X)$를 실제로 구하는 것은 아주 어려운 작업에 속합니다. 따라서, 생성 모델은 고차원 데이터의 확률 분포 $P_{data}(X)$에 한없이 가까운 근사 분포 $P_{model}(X)$을 학습해, 구하기 어려운 $P_{data}(X)$ 대신 구하기 상대적으로 쉬운  $P_{model}(X)$을 사용해 새로운 데이터 $X$을 생성하고자 합니다.

<img src="https://wikidocs.net/images/page/228787/generative3.png" width="40%">

이를 통해 생성 모델은 이미지, 음악, 텍스트 등 확률 분포로부터 표본을 추출해 다양한 고차원 콘텐츠를 생성할 수 있습니다.

<img src="https://wikidocs.net/images/page/228787/generative1.png" width="70%">

예를 들어, 학습 데이터셋 $X = [x_1, x_2, x_3, x_4, \cdots, x_n]$를 입력으로 받는 생성 모델은 각각의 데이터 $x_i$에 대한 고유한 확률 분포 $P(x_i)$를 학습합니다. 이 확률 분포는 해당 데이터가 발생할 확률을 나타내며, 제약이 없는 경우 각 분포가 발생할 가능성을 의미합니다. 예를 들어, 강아지 이미지에 대한 확률 분포 $P(x_2)$가 가장 높다고 할 수 있습니다. 이는 강아지 이미지가 생성될 가능성이 다른 이미지에 비해 높음을 의미합니다.

반면, 판별 모델은 주어진 입력이 특정 클래스에 속하는지 여부를 판단하는 데 집중합니다. 예를 들어, 동물 사진이 아닌 전혀 다른 유형의 이미지를 입력받았을 때, 판별 모델은 여전히 그 이미지를 고양이나 강아지로 분류하려 할 수 있습니다. 하지만 생성 모델은 각 이미지의 특성을 나타내는 확률 분포를 학습하기 때문에, 이러한 오류의 가능성을 줄일 수 있습니다.

<img src="https://wikidocs.net/images/page/228787/generative2.png" width="70%">

생성 모델은 또한 특정 임계값(Threshold)을 설정하여 원하지 않는 결과를 거부(Reject)하는 방식을 사용할 수 있습니다. 예를 들어, 동물의 사진을 생성하는 모델에서는 이러한 임계값을 사용하여 비현실적이거나 원치 않는 결과를 제거할 수 있습니다. 이를 통해 판별 모델과는 다르게 잘못된 결과의 생성을 줄일 수 있습니다.

## 보충 자료

[Understanding the Distinction: Generative Models vs. Discriminative Models](https://www.linkedin.com/pulse/understanding-distinction-generative-models-vs-shailendra-prajapati)<br>
[discriminative vs generative](https://ratsgo.github.io/generative%20model/2017/12/17/compare/)<br>
[Generative VS Discriminative Models](https://medium.com/@mlengineer/generative-and-discriminative-models-af5637a66a3)