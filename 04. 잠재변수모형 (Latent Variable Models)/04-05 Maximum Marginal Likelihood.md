<img src="https://wikidocs.net/images/page/229768/deeplatent2.png" width="70%">

$p(x)$을 계산할 수 있고 나타낼 수 있는 autoregressive 모델은 $p(x)$을 정의한 후, maximum likelihood learning을 통해 $p(x)$가 최대가 되는 $x^{\star}$를 찾아 샘플링을 통해 데이터를 생성할 수 있었습니다.

하지만, 잠재 변수인 $z$을 포함한 latent variable model $\sum\limits_{z}p(x,z)$은 autoregressive 모델처럼 계산할 수 없기 (intractable) 때문에, 기존과는 다른 방법을 사용해야 합니다. 저희는 이를 계산하기 용이한 식으로 근사화 (marginalize)해 풀이한다는 의미로 이 방법을 **Maximum Marginal Likelihood **이라고도 명칭합니다. 앞서 정의한 가우시안 혼합 모델을 $p(x,z)$으로 정의하면 다음과 같습니다.

$$p(x) = \sum\limits_{z}p(x,z) =  \sum\limits_{z}p(z)p(x|z) = \sum\limits_{k=1}^K p(z=k)N(x;\mu_k, \sigma_k)$$

계산이 가능한 autoregressive 모델과는 달리 $\sum\limits_{z}p(x,z)$가 계산이 불가능한 이유는 바로 고려해야 할 잠재 변수 $z$의 특성 때문입니다.  생성 모델의 학습 목표인 maximum likelihood learning을 잠재 변수를 추가해 다음과 같이 수정할 수 있는데,

$$log \prod\limits_{x \in D} p(x;\theta) = \sum\limits_{x \in D} log p(x;\theta) = \sum\limits_{x \in D} log \sum\limits_{z} p(x, z;\theta)$$

$log \sum\limits_{z} p(x, z;\theta)$에서 모든 각각의 잠재 변수 $z$에 대해서 joint probability를 구한 다음 합산해야, $x$와 $z$의 marginal probability를 구할 수 있습니다. 잠재 변수의 개수가 적으면 문제가 없겠지만, 복잡한 데이터에서는 잠재 변수의 개수가 굉장히 많아지기 때문에, 실질적인 계산이 불가능에 가깝습니다.

이를 테면, 잠재 변수 $z$가 30개의 잠재 이진 잠재 변수 $[z_1, \cdot\cdot\cdot, z_{30}] \in [0,1]$로 이루어져 있다면, $log \sum\limits_{z} p(x, z;\theta)$을 구하기 위해서는 $2^{30}$개의 파라미터를 가진 모델을 구축해야 합니다. 또한, 파라미터를 업데이트하는 과정에서 gradient $\nabla_{\theta}$를 구하는 데에도 어려움이 생깁니다.

따라서 $\sum\limits_{z}p(x,z)$를 직접 구하는 것보다 해당 분포를 계산하기 상대적으로 더 쉬운 확률 분포로 근사(approximate)한 이후 쉬운 분포가 기존 분포에 trivial할 정도로 가깝다는 것을 증명하는 방식이 사용되고 있고, 이 방식이 생성 모델에서 아주 중요하게 다루어지고 있는 **변분 추론 (Variational Inference)** 방법입니다.