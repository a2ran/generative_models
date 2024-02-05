![](https://wikidocs.net/images/page/228875/fvsbn1.png)

binarized MNIST 데이터셋 $D$을 가지고 생성 모델을 학습해 새로운 이미지를 생성하는 작업을 예시로 들자면, 각각의 이미지는 $n = 28 \times 28 = 784$개의 픽셀을 가지고 있고, 각각의 픽셀은 [검정색 (0), 하얀색 (1)]의 binary한 값을 가질 수 있습니다.

**Objective** : 학습 데이터셋 $D$로부터 $x \in [0, 1]^{784}$인 확률 분포 $p(x) = p(x_1, \cdot\cdot\cdot, x_{784})$를 학습해 $p(x)$로부터 $x$을 추출했을 때, $(x \sim p(x))$ $0$에서 $9$까지의 숫자 이미지 중 하나를 생성한다.

이 작업을 수행하기 위해 로지스틱 회귀 (logistic regression)을 사용한 autoregressive model을 구축할 수 있습니다.

## AutoRegressive Model from Logistic Regression

먼저 이미지의 픽셀을 순차적인 배열로 나타내기 위해서, 이미지의 좌측 상단 픽셀 $(X_1)$ 부터 우측 하단 픽셀 $(X_{n=784})$까지 순차적으로 정렬하겠습니다.

$p(X) = p(x_1, \cdot\cdot\cdot, x_{784})$를 Chain Rule을 사용해 다음과 같이 나타낼 수 있습니다.

$$p(x_1, \cdot\cdot\cdot, x_{784}) = p(x_1)p(x_2|x_1)p(x_3|x_1, x_2) \cdot\cdot\cdot p(x_n | x_1, \cdot\cdot\cdot x_{n-1}) $$

앞서 개요에서 설명드렸다 싶이 많은 컴퓨팅 파워를 요구하는 조건부 확률을 매개변수화 (parameterize)해 해당 조건부확률을 매개변수를 받아 학습하는 방식으로 학습할 수 있고, 조건부확률을 학습하기 위해 가장 메이저한 알고리즘 중 하나인 로지스틱 회귀 (logistic regression)을 사용하고자 합니다.

$$p(x_1, \cdot\cdot\cdot, x_{784}) = p_{CPT}(x_1;\alpha^1)p_{logit}(x_2|x_1;\alpha^2)p_{logit}(x_3|x_1, x_2;\alpha^3) \cdot\cdot\cdot p_{logit}(x_n | x_1, \cdot\cdot\cdot x_{n-1};\alpha^n) $$
<br><br>

* $p_{CPT}(X_1 = 1;\alpha^1) = \alpha^1$, $p_{CPT}(X_1 = 0;\alpha^1) = 1 - \alpha^1$

첫번째 픽셀의 확률 분포의 경우 파라미터 $\alpha^1$에 대해 조건부 확률 테이블 (CPT)에서 값을 받아 배정합니다. 학습 대상인 파라미터는 $\alpha^1$ 1개입니다.

* $p_{logit}(X_2=1|x_1;\alpha^2) = \sigma(\alpha_0^2  + \alpha_1^2x_1)$

두번째 픽셀의 조건부확률은 $x_2$ 이전의 $x_1$을 매개변수화해 파라미터 $\alpha_0^2, \alpha_1^2$에 대한 로지스틱 모델 $\sigma$를 학습합니다. 학습 대상인 파라미터는 $\alpha_0^2, \alpha_1^2$ 2개입니다.

* $p_{logit}(X_3=1|x_1, x_2;\alpha^3) = \sigma(\alpha_0^3 + \alpha_1^3x_1 + \alpha_2^3x_2)$

세번째 픽셀의 조건부확률은 $x_3$ 이전의 $x_1, x_2$을 매개변수화해 파라미터 $\alpha_0^3, \alpha_1^3, \alpha_2^3$에 대한 로지스틱 모델 $\sigma$를 학습합니다. 학습 대상인 파라미터는 $\alpha_0^3, \alpha_1^3, \alpha_2^3$ 3개입니다.

<br><br>

종합하자면, 저희는 MNIST 데이터 $X$의 확률 분포 $p(x_1 , \cdot\cdot\cdot, x_{784})$를 구하기 위해 $x_1$ 시점의 첫번째 픽셀부터 $x_{784}$ 시점의 마지막 픽셀까지의 순차적인 chain rule을 통해 풀이하고자 합니다. 

Chain Rule을 풀이하는 중 엄청난 컴퓨팅 파워를 요구하는 조건부확률 $p(x_i|x_1, \cdot\cdot\cdot, x_{i-1})$ 을 계산하기 위해 $x_i$ 시점 이전까지의 시점 $x_1, \cdot\cdot\cdot, x_{i-1}$을 매개변수화하여, 조건부확률을 매개변수 $\alpha^i$에 대한 로지스틱 회귀 알고리즘을 통해 학습할 수 있습니다. 

이를 통해 확률 분포가 가진 심층적인 구조는 유지하면서, 파라미터 수를 $1 + 2 + 3 + \cdot\cdot\cdot + n ≈ \frac{n^2}{2}$ 까지 줄일 수 있습니다. Big-O Notation 상으로도 기존 $2^n \rightarrow n^2$으로 최적화한 것을 확인 가능합니다. 여기까지의 과정이 바로 **FVSBN (Fully VIsible Sigmoid Belief Network)** 입니다.

## FVSBN

![](https://wikidocs.net/images/page/228875/fvsbn2.png)

FVSBN은 $i$ 시점 이전 $<i$ 시점의 파라미터를 가지고 학습한 심층 신경망 구조인 **AutoRegressive** 모델이며, 위의 이미지에서도 $\hat{x}_2$는 $x_1$,  $\hat{x}_4$는 $x_1, x_2, x_3$ 파라미터를 가지고 학습한 로지스틱 회귀 모형에서 왔음을 앞선 과정을 통해 확인할 수 있습니다.

결과적으로 FVSBN 학습의 목적인 $p(x_1 , \cdot\cdot\cdot, x_{784})$을 구하려면 조건부확률에 대해 학습시킨 모델을 가져와 적용해주면 됩니다.

<img src="https://wikidocs.net/images/page/228875/fvsbn3.png" width="80%">

$$p(X_1 = 0, X_2 = 1, X_3 = 1, X_4 = 0) = (1 - \hat{x}_1) \times \hat{x}_2 \times \hat{x}_3 \times (1 - \hat{x}_4)$$ $$=(1 - \hat{x}_1) \times \hat{x}_2(X_1 = 0) \times \hat{x}_3(X_1 = 0, X_2 = 1) \times (1 - \hat{x}_4(X_1 = 0, X_2 = 1, X_3 = 1)$$

<br>

이러한 방식을 통해 $p(X_1 = 0, X_2 = 1, X_3 = 1, X_4 = 0)$을 구했다면, 이제 표본 추출을 통해 새로운 이미지 $X$을 생성할 수 있습니다.

1. Sample $\bar{x_1} \sim p(x_1)$
2. Sample $\bar{x_2} \sim p(x_2 | x_1 = \bar{x_1})$
3. Sample $\bar{x_{784}} \sim p(x_{784} | x_1 = \bar{x_1}, \ldots, x_{783} = \bar{x_{783}})$

$X = [\bar{x_1}, \bar{x_2}, \ldots, \bar{x_{784}}]$을 다시 $28 \times 28$ 크기의 벡터로 만들어 이미지로 나타낼 수 있습니다.