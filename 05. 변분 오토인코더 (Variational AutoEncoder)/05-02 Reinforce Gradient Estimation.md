지난 챕터를 통해 계산이 불가능한 $\log p(x;\theta)$를 $\theta$, 그리고 $\phi$에 대한 두 가지의 gradient update 문제를 해결하는 작업으로 나누어 근사하는 stochastic variational inference 방법에 대해 학습했습니다.

하지만, 3장에서 다루었듯이 $\theta$에 대해서는 비교적 쉽게 gradient descent 작업을 수행할 수 있지만, $q(z)$의 파라미터인 $\phi$는 $\theta$를 업데이트 할 때마다 내부적으로 최적화 과정을 거쳐야 하기 때문에 상대적으로 계산이 복잡합니다.

> For loop을 예시로 든다면, $\theta$에 대한 loop 안에 $\phi$가 들어있음!

```
for _ in range(N):
	theta optimization
	for i in range(M):
		phi optimization
```
Reinforce Gradient Estimation은 계산이 어려운 $q(z;\phi) = q_{\phi}(z)$를 몬테 카를로 방법을 통해 근사하는 방식으로 해결하는 방법입니다. 

즉, 기존 $\phi^1$부터 $\phi^M$까지 업데이트해 각각에 대해 $\nabla_{\phi^i}L(x^i;\theta, \phi^i)$를 구하는 대신, 몬테 카를로 방법을 통해 $\nabla_{\phi}L(x;\theta, \phi)$을 근사하여 계산하는 방법으로 계산량을 효과적으로 줄일 수 있습니다.

ELBO 식인 $L(x; \theta, \phi)  = E_{q(z;\phi)}[\log p(z, x;\theta) - \log q(z; \phi)]$을 간단하게 $f(z)$으로 나타내면, 다음과 같은 관계식을 손쉽게 도출할 수 있습니다.

$$E_{q_{\phi}(z)}[f(z)] = \sum\limits_{z}q_{\phi}(z)f(z)$$
$$ \frac{\partial}{\partial \phi^i} E_{q_{\phi}(z)}[f(z)] = \sum\limits_{z}\frac{\partial q_{\phi}(z)}{\partial \phi^i}f(z) = \sum\limits_{z} q_{\phi}(z) \frac{1}{q_{\phi}(z)}  \frac{\partial q_{\phi}(z)}{\partial \phi^i}f(z) $$
$$ \sum\limits_{z} q_{\phi}(z) \frac{\partial \log q_{\phi}(z)}{\partial \phi^i}f(z) = E_{q_{\phi}(z)}[\frac{\partial \log q_{\phi}(z)}{\partial \phi^i}f(z)]$$

즉, 전체 gradient $\nabla_{\phi}L(x;\theta, \phi)$를 몬테 카를로 방법으로 근사할 수 있게 하는 방법이 Reinforce Gradient Estimation 방법입니다.

$$ \nabla_{\phi}E_{q_{\phi}(z)}[f(z)] = E_{q_{\phi}(z)}[f(z)\nabla_{\phi}\log q_{\phi}(z)] ≈ \frac{1}{K} \sum\limits_{k}f(z^k)\nabla_{\phi}\log q_{\phi}(z^k)$$


