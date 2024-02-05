Maximum Likelihood Learning의 학습 방식을 설명하기 위해, 편향된 (biased) 동전을 던져 heads (H)와 tails (T)의 횟수를 추정하는 real-life example을 예시로 들겠습니다.

$$\max\limits_{p_{\theta}} \frac{1}{|D|}\sum\limits_{x \in D}log p_{\theta}(x)$$

정상적인 동전을 던지면 두 가지 결과 heads (H)와 tails (T) $x \in [H, T]$가 나오고, 각각의 결과가 나올 확률은 전부 $p(x = H) = 0.5$, $p(x = T) = 0.5$입니다. 하지만 만일 H가 T보다 나올 가능성이 더 높은 편향된 동전이 있다고 가정하고, 100번의 독립시행을 통해 앞면이 60번 나왔다면, 실제로 H가 나올 확률 $p_{\theta}(x = H)$은 어떻게 계산할 수 있을까요?

편향된 동전을 100번 던진 데이터셋을 $D = [H, H, T, H, T , \cdot\cdot\cdot, H]$이라고 하고, H가 나올 확률과 T가 나올 확률을 $p_{\theta}(x = H) = \theta$, $p_{\theta}(x=T) = 1 - \theta$으로 설정하면, 전체 데이터의 분포 $p_{\theta}(x)$는 다음과 같이 나타낼 수 있습니다.

$$p_{\theta}(x) = L(\theta) = \theta^{\text{#} heads} \times (1 - \theta)^{\text{#} tails}$$

$p_{\theta}(x)$은 log으로 묶여있는 log-likelihood function이므로 다음과 같이 계산할 수 있습니다.

$$logp_{\theta}(x) = logL(\theta) = log(\theta^{\text{#} heads} \times (1 - \theta)^{\text{#} tails})$$
$$= \text{#} heads \times log(\theta) + \text{#} tails \times log(1 - \theta) $$

이로써 $\sigma$ 안의 $logp_{\theta}(x)$의 계산을 완료했고, 저희의 목표는 바로 $log L(\theta^{\star})$를 최대로 만드는 $\theta^{\star} \in [0,1]$를 찾는 것이므로 $log L(\theta)$를 미분하여 극대값을 찾아 계산하면 $\theta^{\star}$을 구할 수 있습니다.

$$\theta^{\star} = \frac{\text{#} heads}{\text{#} heads + \text{#} tails}$$

즉, 편향된 동전을 100번 던진 데이터셋에서 H가 60번, T가 40번 나왔으므로 $\theta^{\star}$은 $\frac{60}{60 + 40} = 0.6$이 됩니다.