앞서 KL-Divergence를 사용해 구한 학습 목표 $arg \max\limits_{p_{\theta}} E_{x \sim p_{data}}[log p_\theta (x)]$ 을 학습 데이터셋에 적용하기 위해, 저희는 고전 표본추출 알고리즘인 **몬테-카를로 예측 (Monte Carlo Estimation)** 을 이용할 수 있습니다.

$$E_{x \sim p}[g(x)] = \sum\limits_{x}g(x)p(x)$$

Monte Carlo Estimation은 $p$에 대한 expectation $E_{x \sim p}$가 주어지고, $g(x)$라는 확률 분포가 있을 때, 학습 데이터 $x^1, \cdot\cdot\cdot, x^T$를 확률 분포 $g(x)$에 적용한 $g(x^t)$의 평균을 구함으로서 expectation을 예측할 수 있다는 알고리즘입니다. 

$$\hat{g}(x^1, \cdot\cdot\cdot, x^T) = \frac{1}{T}\sum\limits_{t=1}^T g(x^t)$$

Monte Carlo Estimation의 세 가지 요소

* Unbiasedness : $E_p[\hat{g}] = E_p[g(x)]$
* Convergence : $\hat{g} = \frac{1}{T}\sum\limits_{t=1}^T g(x^t) \rightarrow E_p[g(x)]$ for $T \rightarrow \infty$
* Variance : $V_p[\hat{g}] = V_p[\frac{1}{T}\sum\limits_{t=1}^T g(x^t)] = \frac{V_p[g(x)]}{T}$

$p_{data}$에 대한 expectation $E_{x \sim p_{data}}$ 가 주어지고, 확률 분포 $g(x) = log p_\theta (x)$ 인 학습 목표 $E_{x \sim p_{data}}[log p_\theta (x)]$에 저희의 학습 데이터 $D = [x^1, \cdot\cdot\cdot, x^T]$을 적용할 수 있습니다.

$$E_{x \sim p_{data}}[log p_\theta (x)] = E_{D}[log p_\theta (x)] = \frac{1}{|D|}\sum\limits_{x \in D}log p_{\theta}(x)$$

즉, 저희의 데이터셋 $D$를 KL-Divergence으로 정의한 학습 목표에 적용해 해당 학습 목표를 최대화하는 최적화 문제를 풀이함으로서 $p_{\theta}^{\star}$를 구할 수 있게 되고, 해당 방법을 최대 우도 학습 (Maximum Likelihood Learning)으로 정의할 수 있습니다.

$$\max\limits_{p_{\theta}} \frac{1}{|D|}\sum\limits_{x \in D}log p_{\theta}(x)$$