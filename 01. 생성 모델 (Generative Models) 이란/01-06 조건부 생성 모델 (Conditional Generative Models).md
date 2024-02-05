통상적인 생성형 AI 모델은 저차원의 간단한 입력인 프롬프트 $Y$를 받아 고차원의 데이터 $X$을 생성하는 방식으로 운용되고 있습니다. 조건부 생성 모델(Conditional Generative Models)은 고차원 데이터의 복잡한 분포를 학습하여, 저차원 입력으로부터 고차원의 결과를 생성할 수 있습니다. 이 과정은 베이즈 정리를 통해 수학적으로 이해할 수 있습니다.

베이즈 정리:

$$P(X|Y) = \frac{P(Y|X)P(X)}{P(Y)} $$

이 때,

* $P(X|Y)$는 주어진 프롬프트 $Y$에 대한 데이터 $X$의 조건부 확률로, **조건부 생성 모델(Posterior)** 을 의미합니다. 즉, 특정 프롬프트가 주어졌을 때, 해당 프롬프트에 부합하는 데이터 $X$를 생성하는 확률을 나타냅니다.
* $P(Y|X)$는 데이터 $X$가 주어졌을 때 프롬프트 $Y$의 확률로, **가능도(Likelihood)** 를 의미합니다. 이는 주어진 데이터가 특정 프롬프트를 얼마나 잘 설명하는지를 나타냅니다.
* $P(X)$는 생성 모델이 학습한 데이터 $X$의 **사전 확률 분포(Prior)** 입니다. 이는 데이터 $X$가 일반적으로 얼마나 자주 발생하는지에 대한 정보를 제공합니다.
* $P(Y)$는 프롬프트 $Y$의 **사전 확률 분포(Prior)** 입니다. 이는 특정 프롬프트가 얼마나 일반적인지를 나타냅니다.

![](https://wikidocs.net/images/page/228788/conditionalgenerative1.png)

이러한 베이즈 정리를 기반으로, 생성형 AI 모델은 주어진 프롬프트 $Y$에 대해 가장 높은 가능도를 가지는 조건부 생성 모델, 즉 Posterior $P(X|Y)$를 찾아내고, 이를 바탕으로 고차원 데이터 $X$를 생성합니다. 예를 들어, 프롬프트가 "cat"이라면, 모델은 $P(X|\text{"cat"})$이 최대가 되는 고양이 이미지를 생성하고, "dog"라는 프롬프트에 대해서는 $P(X|\text{"dog"})$가 최대가 되는 강아지 이미지를 생성합니다.

이 과정에서의 핵심은 모델이 두 개의 확률 모델 $P(Y|X)$와 $P(X)$에 대한 충분한 학습을 통해 최적의 조건부 확률 분포 $P(X|Y)$를 도출하게 하는 것입니다. 이 과정은 고차원 데이터를 효과적으로 생성하기 위한 핵심 단계로, 모델의 성능은 이러한 학습 과정의 품질에 크게 의존합니다.

그러므로 현재 인공지능/AI 학계에서는 최적의 조건부 확률 분포 $P(X|Y)$를 설정하기 위해 다양한 알고리즘이 개발되어 왔습니다. 이러한 알고리즘들의 주요 목표는 주어진 입력 프롬프트 $Y$에 대해 가능한 정확하고 현실적인 고차원 데이터 $X$를 생성하는 것입니다. 이를 위해서, 생성 모델 알고리즘들은 복잡한 데이터 패턴을 인식하고, 이를 기반으로 실제와 유사한 출력을 생성하는 능력을 향상시키는 것을 중점으로 둡니다.

이 과정에서 중요한 것은 모델이 어떻게 사전 확률 분포 $P(X)$와 가능도 $P(Y|X)$를 학습하고, 이를 통해 얼마나 효과적으로 조건부 확률 분포 $P(X|Y)$를 추정하는가입니다. 이를 위해 Variational Autoencoders (VAE), Generative Adversarial Networks (GAN), AutoRegressive models, Normalizing Flows, Score Based models 등 여러 유명한 알고리즘들이 개발되었고, 각 알고리즘은 고차원 데이터를 모델링하고 생성하는 과정을 각자의 방식으로 최적화합니다.

이러한 기술적인 발전에 힘입어 생성 모델은 점점 더 정교하고 다양한 종류의 데이터를 생성할 수 있습니다. AI 커버 제작부터 시작해, 표현 학습, 가상 채팅봇, 심지어 복잡한 시뮬레이션 진행까지, 생성형 AI 모델들은 다양한 분야에서 혁신을 주도하고 있으며, 앞으로도 꾸준히 발전이 이루어질 예정입니다.  

## 보충 자료

[Understanding the Distinction: Generative Models vs. Discriminative Models](https://www.linkedin.com/pulse/understanding-distinction-generative-models-vs-shailendra-prajapati)<br>
[discriminative vs generative](https://ratsgo.github.io/generative%20model/2017/12/17/compare/)<br>
[Generative VS Discriminative Models](https://medium.com/@mlengineer/generative-and-discriminative-models-af5637a66a3)