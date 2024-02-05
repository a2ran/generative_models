지금까지 학습한 챕터 3을 되짚어보면 저희는 정의한 생성 모델 $p_{\theta}$을 가지고 실제 분포 $p_{data}$에 가장 근사한 최적의 $p_{\theta}^{\star}$으로 만들기 위해 

학습 목표인 목적 함수$L(\theta, D)$를 $L(\theta, D) = \sum\limits_{j=1}^m \sum\limits_{i=1}^n log p_{neural}(x_i^{(j)}|pa(x_i)^{(j)}; \theta_i) $ 으로 설정했고, 

해당 likelihood function을 최적화하는 maximum likelihood learning 과정인 $arg \max\limits_{\theta} L(\theta, D)$ 최적화 문제는 $L(\theta, D)$의 gradient을 descent하는 방식으로 풀이했습니다.

1. Initialize $\theta^0$
2. Compute $\nabla_{\theta} L(\theta, D)$ --> back propagation!
3. Update $\theta^{t+1} = \theta^t + \alpha_t\nabla_{\theta}L(\theta, D)$

$$\nabla_{\theta}L(\theta, D) = \sum\limits_{j=1}^m \sum\limits_{i=1}^n \nabla_{\theta} log p_{neural}(x_i^{(j)}|pa(x_i)^{(j)}; \theta_i)  $$

**확률론적 경사 하강법 (Stochastic Gradient Descent)** 은 저희가 이전까지 학습한 원리를 집약한 빅데이터와 대규모 언어 모델 학습에 있어 필수적인 역할을 수행하는 알고리즘입니다. 최근 생성 모델 아키텍쳐는 수백만개를 넘어 수천억개의 데이터 $D = [x_1, \cdot\cdot\cdot, x_100,000,0...]$에서 학습하기 때문에, 모든 데이터를 가지고 파라미터 $\theta$를 업데이트 하는 기존 방법으로는 학습이 불가능에 가깝습니다. 

이를 해결하기 위해 데이터셋 $D$에서 monte carlo method으로 일부 표본을 추출한 다음에, 해당 표본을 가지고 경사하강법을 진행하는 방법이 바로 저희가 생성 모델을 학습할 때 주로 사용하는 Stochastic Gradient Descent 알고리즘입니다.

$$\nabla_{\theta}L(\theta, D) = m\sum\limits_{j=1}^m \frac{1}{m} \sum\limits_{i=1}^n \nabla_{\theta} log p_{neural}(x_i^{(j)}|pa(x_i)^{(j)}; \theta_i)  $$
$$= mE_{x^{(j)} \sim D}[\sum\limits_{i=1}^n \nabla_{\theta} log p_{neural}(x_i^{(j)}|pa(x_i)^{(j)}; \theta_i)] $$

since $E_{D}[log p_\theta (x)] = \frac{1}{|D|}\sum\limits_{x \in D}log p_{\theta}(x)$ (monte carlo method)

즉 데이터셋 $D$ 전체에 대해 학습을 진행하는 것이 아니라, 한 번의 iteration 당 학습의 대상이 되는 $x^{(j)} \sim D; \nabla_{\theta}L(\theta, D)$ 샘플들을 가지고 일부에 대해서 파라미터 $\theta$를 업데이트합니다.