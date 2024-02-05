앞서 Reinforce Gradient Estimation 방법을 통해 $\phi$를 업데이트하는 과정을 최적화하는 방법을 학습했습니다. 하지만, 해당 방법은 큰 단점이 하나 있는데, 이상치의 영향을 매우 크게 받는다는 점입니다. Gradient를 $\log$으로 치환하면 0에 가까운 숫자는 $-\infty$로 발산하는데, 이 경우 출력의 편차가 매우 커지기 때문에 결과의 분산이 non-trivial할 정도로 커집니다.

따라서 실제 생성 모델 알고리즘은 또 다른 최적화 방법인 **Reparameterization Trick**을 주로 사용합니다. 만일 $z$가 연속인 경우 $\phi$에 대한 분포의 gradient를 구하고 싶을 때,

$$E_{q_{\phi}(z)} [r(z)] = \int q(z;\phi)r(z)dz$$

기존의 방식대로 $z \sim q_{\phi}(z)$을 수행한다면 가우시안 분포 $q_{\phi}(z) = N(\mu, \sigma^2I)$의 파라미터 $\mu$와 $\sigma$를 고려해야 합니다. 하지만, 해당 가우시안 분포를 $N(0,1)$으로 정규화를 시켜 $z = \mu + \sigma \epsilon = g(\epsilon;\phi)$로 나타낸다면, $\epsilon \sim N(0,1)$을 수행함으로서 파라미터 계산을 최적화할 수 있습니다. 이렇게 정규화 작업을 수행한 이후 최적화하는 방법을 Reparameterization Trick이라고 합니다.

이 경우 기존 $E_{q_{\phi}(z)} [r(z)]$ 식은 다음과 같이 나타낼 수 있으며,  gradient $\nabla_{\phi}L(x;\theta, \phi)$도 손쉽게 도출할 수 있습니다.

$$ E_{q_{\phi}(z)} [r(z)] = E_{\epsilon \sim N(0,1)} [r(g(\epsilon; \phi))] = \int p(\epsilon)r(\mu + \sigma \epsilon)d\epsilon$$
$$\nabla_{\phi} E_{q_{\phi}(z)} [r(z)] = \nabla_{\phi} E_{\epsilon}[r(g(\epsilon; \phi))] = E_{\epsilon}[\nabla_{\phi} r(g(\epsilon; \phi))]$$

즉, 계산하기 어려운 $z \sim q_{\phi}(z)$에 대한 식을 정규화해 미분이 가능한 $r$과 $g$, 그리고 추출하기 쉬운 $\phi$와 $\epsilon$으로 나타낸 이후 몬테 카를로 방법으로 근사해 gradient 작업을 수행할 수 있습니다.

$$ \nabla_{\phi}E_{\epsilon}[r(g(\epsilon; \phi))] = E_{\epsilon}[\nabla_{\phi}r(g(\epsilon; \phi))] ≈ \frac{1}{K} \sum\limits_{k}\nabla_{\phi}r(g(\epsilon; \phi))$$

