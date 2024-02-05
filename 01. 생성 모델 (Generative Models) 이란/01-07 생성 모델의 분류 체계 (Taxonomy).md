<img src="https://wikidocs.net/images/page/228789/taxonomy1.png" width="80%">

생성 모델은 세상에서 존재하는 모든 객체들이 어느 특정한 확률 분포 $P_{data}$를 따른다고 가정합니다. 

하지만 통상적인 고차원의 분포 $P_{data}$을 직접적으로 알아내는 것은 불가능합니다. ($P_{강아지} = ??$)

따라서 생성 모델은 실제 분포인 $P_{data}$에 한없이 가까운 $P_{model}$을 학습해 $P_{model}$에서 생성한 데이터 $X$가 실제 분포 $P_{data}$에서 추출한 데이터 $X$와 유사하게 만드는 방식으로 실제 분포를 근사하고자 합니다. 이것이 바로 생성 모델 (generative models) 알고리즘의 기본 원리입니다. 

$$ \therefore p_{data} ≈ p_{model}  $$

유명한 생성모델 알고리즘인 Generative Adversial Networks (GAN)의 창시자인 Ian GoodFellow는 위의 그림과 같은 생성 모델의 분류 체계도 (Taxonomy)를 사용해 $P_{data}$ 에 근사한 $P_{model}$ 분포를 추정하는 생성 모델 알고리즘들을 분류했습니다.

## Explicit & Tractable Density

Explicit & Tractable Density란, 생성 모델이 $P_{model}$을 직접적으로 (explicitly) 계산이 가능한 (Tractable) 분포로 추정해 $P_{model}$의 분포에 근사하게 하는 방식입니다. 이를 테면, 데이터 $X$가 정규분포 (Gaussian Distribution) $P(x;\theta)$를 따른다고 가정하면, 저희는 정규 분포의 성질을 이용해 확률 밀도 함수 (Probability Density Function)을 명시적으로 정의할 수 있을 뿐더러 최대 우도 추정 (Maximum Likelihood Estimation) 방법을 통해 $P_{model}(x;\theta)$의 모수를 계산할 수 있습니다. Explicit & Tractable Density의 대표적인 알고리즘으로는 AutoRegressive 모델 등이 있습니다.

하지만 이 방식은 데이터의 복잡성이 증가함에 따라 제한적일 수 있습니다. 예를 들어, PixelCNN은 이미지 데이터의 분포를 tractable한 형태로 정의했지만, 이미지와 같은 고차원의 데이터의 확률 분포는 계산 가능한 범위를 벗어난 심층적인 구조를 보유하고 있기 때문에 직접적인 계산이 매우 어렵습니다. 하지만 PixelCNN을 넘어 Autoregressive LLMs (AR-LLMs)까지 발전하는 과정에서 여러 혁신적인 알고리즘을 차용하여 기존 문제점을 해소할 수 있었고, 현재 자연어 처리 분야에서 주요 알고리즘으로 사용되고 있습니다.

## Explicit & Approximate Density

Explicit & Approximate Density 알고리즘은 고차원 데이터의 정확한 분포를 추정하지 못하는 Autoregressive Model의 한계점을 극복하기 위해 기존 계산을 목표로 한 분포를 더욱 복잡하게 만들어 계산이 가능하지 않은 (intractable)한 확률 분포로 $P_{model}$을 설정한 다음, 계산하지 못하는 해당 분포를 근사 (approximate)하여 사용하는 생성모델 알고리즘입니다. 

<img src="https://wikidocs.net/images/page/228789/taxonomy2.png" width="100%">

대표적인 알고리즘 중 하나인 Variational Autoencoder에서 posterior $P_{model} = P(Z|X)$을 구하려고 하면, 베이즈 정리에 따라 $P(Z|X) = \frac{P(X|Z)P(Z)}{P(X)}$을 계산하면 구할 수 있지만, 분모의 $P(X)$은 계산이 불가능하기 때문에 $P(Z|X)$은 계산이 불가능한 intractable한 확률 분포입니다.

저희는 계산이 불가능한 $P_{model} = P(Z|X)$을 구하기 위해 계산이 상대적으로 쉬운 가우시안 분포 $q(Z)$을 최적화해, 구할 수 없는 $P(Z|X)$의 분포에 가장 가까운 분포 $q^{\star}(z)$을 찾아서 근사하고자 합니다. 이러한 방법을 **Variational Inference**이라고 합니다.

## Implicit Density

마지막으로, Implicit Density 방법은 데이터 분포를 직접적으로 정의하지 않고, 해당 분포를 따르는 샘플들을 생성함으로써 간접적으로 분포를 표현합니다. 이 접근 방식의 대표적인 예시로는 GAN(Generative Adversarial Networks)이 있습니다.

## Summary

생성 모델 알고리즘의 핵심은 바로 posterior인 $P_{model}$, 즉 $P(Z|X)$를 계산할 수  없음에도 불구하고, 이 분포로부터 샘플을 추출하여 새로운 데이터 $Z$를 생성할 수 있다는 점입니다. 따라서, Autoregressive, VAE, GAN, Diffusion 등 여러 다른 생성 모델 알고리즘은 $P(Z|X)$을 심층 신경망 구조를 통해 추론하는 모델들이고, 이는 확률론적 최적화 방법(Stochastic Optimization Methods)의 진보에 의해 가능해졌습니다.


## 보충 자료

[cs231n_2017_lecture13](http://cs231n.stanford.edu/slides/2017/cs231n_2017_lecture13.pdf)<br>
[Generative Models](https://haehwan.github.io/posts/DS-GenerativeModel/)<br>
[NIPS 2016 Tutorial:Generative Adversarial Networks](https://arxiv.org/pdf/1701.00160.pdf)