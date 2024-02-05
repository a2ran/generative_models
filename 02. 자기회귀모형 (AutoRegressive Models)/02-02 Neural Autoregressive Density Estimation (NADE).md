이전 FVSBN에서는 로지스틱 회귀 모형을 사용해 각 시점의 파라미터 $\alpha^i$를 학습했다면, FVSBN에서 한 단계 발전한 Autoregressive model인 **Neural Autoregressive Density Estimation (NADE)** 모형은 로지스틱 회귀 대신 은닉층 (hidden layers) $h_i$을 탑재한 심층 신경망 구조를 가지고 학습합니다.

<img src="https://wikidocs.net/images/page/228891/nade1.png" width="60%">

위 이미지와 같이 NADE는 FVSBN과는 다르게 $x_i$와 $\bar{x_i}$ 사이에 은닉층 $h_i$가 추가된 것을 확인할 수 있고, 다음과 같이 심층 신경망 구조를 수정할 수 있습니다.

$$h_i = \sigma(W_{<i}x_{<i} + c_i )$$$$\bar{x_i} = p(x_i|x_1, \cdot\cdot\cdot x_{i-1}; W_i, c_i, \alpha_i, b_i) = \sigma(\alpha_ih_i + b_i)$$

** ($W_i, c_i, \alpha_i, b_i$ : 파라미터)

## Parameter Sharing

NADE는 FVSBN과 비교해 $x_i \rightarrow \bar{x_i}$의 과정에서 로지스틱 회귀 대신 은닉증 $h_i$을 경유하는 심층 신경망 구조를 사용한다는 차이만 존재하지만, 해당 구조를 차용함으로서 사용하는 파라미터 수를 최적화할 수 있습니다.

![](https://wikidocs.net/images/page/228891/nade2.png)

이는 데이터에서 은닉층으로 변환하는 $h_i = \sigma(W_{<i}x_{<i} + c_i )$을 진행하는 과정에서 각각의 $x_i$에서 동일한 $w_i$를 보낼 수 있기 때문입니다. 은닉층의 크기를 $h_i \in R^d$으로 가정하면, Autoregressive model의 특징대로 $h_i$는 $<i$ 이전 시점의 $x_{<i}$와 $w_{<i}$를 입력으로 받습니다.  이 때 $h_i$에 각각의 $x_i$에서 동일한 $w_i$를 보냄으로서 학습해야할 파라미터의 수를

$$ d + 2\times d + \cdot\cdot\cdot + n\times d \rightarrow O(n^2 d)$$
$$ d + d  + \cdot\cdot\cdot +  d \rightarrow O(nd)$$

와 같이 $O(n^2 d) \rightarrow O(nd)$로의 최적화가 가능합니다.