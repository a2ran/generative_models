판별 모델(Discriminative Models)은 주어진 입력 데이터 $X$에 대해 특정 레이블 $Y$를 예측하는 알고리즘입니다. 이러한 모델은 입력 데이터와 데이터의 레이블 간의 관계를 학습하여, 새로운 입력에 대해 레이블을 예측하는 결정 경계(Decision Boundary)를 학습합니다. 

예를 들어 CNN(합성곱 신경망)을 사용한 판별 모델을 구축해 특정 동물의 이미지를 분류하는 경우, 주어진 데이터 $X$와 레이블 $Y$ 간의 관계를 학습하여 새로운 데이터에 대한 정답 레이블을 예측할 수 있습니다. 그러나 판별 모델은 데이터의 내재된 특성이나 구조를 심층적으로 이해하거나 새로운 데이터를 생성하는 데는 제한적입니다.

판별 모델의 이러한 한계는 모델이 조건부 확률 $P(Y|X)$, 즉 데이터 $X$가 주어졌을 때 레이블 $Y$의 발생 확률을 직접적으로 계산하는 방식에서 기인합니다. 예를 들어, 고양이와 강아지를 구분하는 이진 분류 모델에서 고양이 이미지에 대해 $P(\text{cat}|\text{이미지 } X) = 0.8$, $P(\text{dog}|\text{이미지 } X) = 0.2$으로 계산되면, 모델은 'cat' 레이블을 출력합니다.

![](https://wikidocs.net/images/page/228786/discriminative1.png)

>고양이의 이미지가 input X로 주어진 경우,<br>
고양이와 강아지의 이미지를 판별하는 이진 분류 모델의 결과는<br>
$P(cat|\text{이미지 } X) = 0.8$, $P(dog|\text{이미지 } X) = 0.2$ 이 되기 때문에<br>
가장 확률이 큰 cat이라는 label을 반환합니다.

하지만, 문제는 판별 모델이 오차 이미지나 모델이 학습하지 않은 유형의 데이터를 처리할 때 발생합니다. 예를 들어, 고양이나 강아지가 아닌 원숭이의 이미지가 입력될 경우, 판별 모델은 이 이미지에 대해 $P(\text{cat}|\text{이미지 }X)$와 $P(\text{dog}|\text{이미지 }X)$를 계산하려 하며, 이는 잘못된 레이블을 생성할 수 있습니다.

![](https://wikidocs.net/images/page/228786/discriminative2.png)

>원숭이의 이미지가 input $X$로 주어진 경우, 고양이와 강아지의 레이블 $Y$을 판별하는 이진 분류 모델은 "해당 없음"이라는 레이블을 출력해야 하지만, 분류 모델은 이미지 $X$를 받아 $P(Y|X)$을 직접적으로 계산하는 방식으로 학습이 되었기 때문에 해당 이미지에 대한 확률값을 계산하게 되고, 고양이라는 잘못 판단한 레이블을 출력합니다.

![](https://wikidocs.net/images/page/228786/discriminative3.png)

또한, 판별 모델은 동물의 이미지가 아닌 전혀 다른 유형의 이미지가 입력될 때도 해당 이미지를 고양이나 강아지로 분류하려고 합니다. 이는 판별 모델이 고차원 입력 데이터에서 저차원의 특징을 추출하여 레이블을 부여하는 예측 문제에는 적합하지만, 데이터의 실제 구조나 분포를 이해하는 데는 한계가 있음을 보여줍니다. 

생성 모델은 데이터 자체의 분포를 학습하고, 이를 바탕으로 새로운 데이터를 생성할 수 있는 능력을 가집니다. 따라서, 생성 모델은 레이블 $Y$로부터 이미지 $X$을 생성하는 작업과 같이 데이터의 내재된 구조를 이해하는 데 더 적합합니다.

## 보충 자료

[Understanding the Distinction: Generative Models vs. Discriminative Models](https://www.linkedin.com/pulse/understanding-distinction-generative-models-vs-shailendra-prajapati)<br>
[discriminative vs generative](https://ratsgo.github.io/generative%20model/2017/12/17/compare/)<br>
[Generative VS Discriminative Models](https://medium.com/@mlengineer/generative-and-discriminative-models-af5637a66a3)