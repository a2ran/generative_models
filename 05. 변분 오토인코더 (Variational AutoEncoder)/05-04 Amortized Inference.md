이전 Stochastic Variational Inference을 통해 다음과 같이 변분 오토인코더의 학습 목표를 설정할 수 있었습니다.

$$\max\limits_{\theta}l(\theta;D) \ge \max\limits_{\theta, \phi^1, \cdot\cdot\cdot, \phi^M} \sum\limits_{x^i \in D} L(x^i; \theta, \phi^i) $$
$$(L(x^i; \theta, \phi^i)  = E_{q(z;\phi^i)}[\log p(z, x^i;\theta) - \log q(z; \phi^i)]) $$

저희는 Autoregressive 모델들에서 살펴보았듯이 아주 큰 데이터셋에 적용했을 때를 대비해 데이터에서 표본을 추출해 그 표본에서 추출하는 stochastic 방법을 사용했지만, $\phi$라는 파라미터가 새롭게 추가되었기 때문에 기존 stochastic 방법만으로는 온전한 최적화 방법이 불가합니다.

따라서, 변분 오토인코더 모델에서 새롭게 도입된 개념이 바로 **Amortized Inference**입니다. 

Amortization이란 쉽게 설명하자면 $M$번에 걸친 최적화 과정을 통해 구했던 $\phi^{i, \star}$를 하나의 심층신경망 $f_\lambda$를 학습시켜 한 번의 계산으로 $\phi^{i, \star}$을 구하는 방법입니다.

즉, $q(z; \phi^i)$를 최적화하는 $\phi^{i, \star}$를 구하기 위해 M 번의 Gradient Descent을 수행하는 Stochastic Variational Inference와는 달리, Amortized Inference를 사용하면 하나의 심층 신경망 구조를 학습시켜 $q(z; f_{\lambda}(x^i)))$ 단 한 번의 계산만 수행하면 됩니다. 따라서 최종 풀이과정을 나열하면 다음과 같습니다.

1. $\theta^{(0)}$, $\phi^{(0)}$ 초기화
	* (기존에는 $\theta, \phi^1, \cdot\cdot\cdot, \phi^M$)
2. 랜덤하게 $x^i$를 데이터셋 $D$로부터 랜덤하게 추출
3. 심층 신경망을 통해 $\nabla_{\phi} L(x^i;\theta, \phi)$ 계산
	* 기존에는 M번 gradient를 구한다음 파라미터 $\phi$가 수렴할 때까지 업데이트
4. 최적의 $\phi^{i, \star}$를 가지고 $\theta$에 대해 gradient descent 수행
5. 2번째 step으로 다시 돌아가 반복

정말 좋습니다~