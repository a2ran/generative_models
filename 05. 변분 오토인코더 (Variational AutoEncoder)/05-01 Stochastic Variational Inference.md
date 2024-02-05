기존 ELBO식을 maximize하기 위해, 변분 오토인코더는 $q(z)$를 optimize하고, 이는 $q(z)$에 변분 파라미터 $\phi$를 추가로 도입하여 $q(z;\phi)$로 바꾸는 과정으로 나타낼 수 있습니다. 기존 $\log p(x; \theta)$식을 다음과 같이 표현하겠습니다.

$$\log p(x; \theta) \ge \sum\limits_z q(z; \phi)\log p(z, x;\theta) + H(q(z; \phi)) = L(x; \theta, \phi)$$

학습 데이터 $D = [x^1, \cdot\cdot\cdot, x^M]$에 대해 학습을 진행하는 경우, 지난 3강 (최대우도학습)에서 그랬듯이 각각의 데이터 $x^i$에 대해 최적의 $\phi^i$를 적용한 이후 더해 최적의 가능도 $l(\theta; D)$를 구할 수 있습니다.

> $\theta$와 $\phi$가 헷갈리는 경우:<br>
> $\theta$는 전체 생성모델의 파라미터를 나타냅니다. (VAE의 디코더 파라미터)<br>
> $\phi$는 $q(z)$를 $p(z|x;\theta)$에 맞추기 위해 중간에 추가로 최적화하는 파라미터입니다. (VAE의 인코더 파라미터)

$$l(\theta; D) = \sum\limits_{x^i \in D} log p(x^i;\theta) \ge \sum\limits_{x^i \in D} L(x^i; \theta, \phi^i)$$

학습 목표인 가능도 $l(\theta; D)$를 다음과 나타낼 수있고, 이를 $\theta$에 대해 최대화하는 최종 학습목표 식에 적용하면 다음과 같은 식으로 나타낼 수 있습니다.

$$\max\limits_{\theta}l(\theta;D) \ge \max\limits_{\theta, \phi^1, \cdot\cdot\cdot, \phi^M} \sum\limits_{x^i \in D} L(x^i; \theta, \phi^i) $$
$$(L(x^i; \theta, \phi^i)  = E_{q(z;\phi^i)}[\log p(z, x^i;\theta) - \log q(z; \phi^i)]) $$

이 학습목표 식을 Monte Carlo 법칙을 적용한 Stochastic Variational Inference 방식으로 풀이하는 과정을 나열하면 다음과 같습니다.

1. $\theta, \phi^1, \cdot\cdot\cdot, \phi^M$와 같은 파라미터 초기화
2. $x^i$를 데이터셋 $D$로부터 랜덤하게 추출
3. 해당 $x^i$에 대해 $L(x^i; \theta, \phi^i)$을 $\phi^i$으로 최적화
	1. $\phi^i$ = $\phi^i$ + $\eta \nabla_{\phi^i}L(x^i;\theta, \phi^i)$식으로 gradient update
	2. $\phi^{i, \star} ≈ arg \max\limits_{\phi}L(x^i; \theta, \phi)$로 수렴할 때까지  gradient update을 반복
4. 최적의 $\phi^{i, \star}$를 가지고 $\theta$에 대해 gradient descent 수행
	1. $\nabla_{\theta}L(x^i; \theta, \phi^{i, \star})$ gradient 계산
	2. $\theta^{t+1} = \theta^t + \alpha^t \nabla_{\theta}L(x^i; \theta, \phi^{i, \star})$으로 생성 모델 gradient update
5. 2번째 step으로 다시 돌아가 반복