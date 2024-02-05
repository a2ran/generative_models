이제 간단한 tossing coins 예제를 넘어 저희가 정의했던 Autoregressive 생성 모델에 Maximum Likelihood Learning 기법을 적용해 $p_{\theta}(x)$를 $p_{\theta}^{\star}(x)$으로 유도하는 목적 함수를 설정하겠습니다.

통상적인 autoregressive model은 다음과 같이 정의할 수 있고, $pa(x_i)$는 *parents i*으로, $<i$ 시점의 데이터입니다.

$$ P_{\theta}(x) = \prod\limits_{i=1}^n p_{neural}(x_i|pa(x_i); \theta_i) $$

해당 autoregressive model을 데이터 $D = [x^{(1)}, \cdot\cdot\cdot, x^{(m)}]$을 가지고 학습한다면, 다음과 같이 likelihood function을 정의할 수 있습니다.

$$L(\theta, D) = \sum\limits_{j=1}^m log P_{\theta}(x^{(j)}) = \sum\limits_{j=1}^m \sum\limits_{i=1}^n log p_{neural}(x_i^{(j)}|pa(x_i)^{(j)}; \theta_i) $$

저희의 목표는 해당 likelihood function을 maximize하는 방향으로 학습하는 것이므로, $arg \max\limits_{\theta} L(\theta, D)$ 최적화 문제를 풀이하면 되고, 해당 방식으로 파라미터 $\theta$를 업데이트하는 과정이 바로 저희에게 익숙한 경사 하강법 **(Gradient Descent)**입니다.

Maximum Likelihood Learning을 통해 파라미터 $\theta$를 최적화하는 과정을 Gradient Descent라고도 부르는 이유는, 파라미터 $\theta$를 업데이트 하기 위해 likelihood function $L(\theta, D)$의 미분값을 learning rate $\alpha_t$에 곱한 값을 더해주기 때문입니다.

1. Initialize $\theta^0$
2. Compute $\nabla_{\theta} L(\theta, D)$ --> back propagation!
3. Update $\theta^{t+1} = \theta^t + \alpha_t\nabla_{\theta}L(\theta, D)$