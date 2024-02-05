<img src="https://wikidocs.net/images/page/228932/mll1.png" width="60%">

앞선 autoregressive 챕터를 통해 저희는 추론할 수 있고 계산이 가능한 파라미터 $\theta$를 가진 생성 모델 $p_{\theta}$를 정의했습니다. 하지만, 아직 실제 분포 $p_{data}$에 가장 근사한 분포 $p_{\theta}^{\star}$으로 $p_{\theta}$의 boundary 내에서 최적화를 하는 과정이 남아있고, 이 학습 목표 (learning objective) $p_{\theta}^{\star}$으로 모델의 파라미터 $\theta$를 업데이트하는 방식이 바로 모델읋 학습 (learning)하는 최적화 (optimize) 과정입니다.

따라서 저희는 $p_{\theta}$가 $p_{\theta}^{\star}$으로 생성 모델을 성공적으로 학습시키기 위해 $p_{data}$와 $p_{\theta}^{\star}$간의 거리 $d(p_{data}, p_{\theta})$를 최소화시키는 학습 목표를 설정하고자 합니다.

해당 최소화 작업을 수행하는 알고리즘이 바로 **Kullback-Leibler Divergence (KL-Divergence)** 입니다.

KL-Divergence:

$$D(p||q) = \sum\limits_{x} p(x) log \frac{p(x)}{q(x)} $$

* $D(p||q) \ge 0$
* $D(p||q) = 0$ if $p = q$
* $D(p||q) \not= D(q||p)$

KL-Divergence를 통해 두 분포 $p$와 $q$간의 유사도를 측정할 수 있으며, 이 컨셉을 $p_{data}$와 $p_{\theta}$에도 적용할 수 있습니다.

$$ D(p_{data}|p_{\theta}) = E_{x \sim p_{data}}[log (\frac{p_{data}(x)}{p_{\theta}(x)})] = \sum_{x} p_{data}(x)log\frac{p_{data}(x)}{p_{\theta}(x)} $$

해당 KL-Divergence 식을 다음과 같이 정리할 수 있습니다. 첫 번째 식 $E_{x \sim p_{data}}[log p_{data}(x)]$은 $p_{\theta}$에 영향을 받지 않기 때문에 최적화 공식 계산에서 제거할 수 있기 때문에 $- E_{x \sim p_{data}}[log p_{\theta}(x)]$만 가지고 계산할 수 있습니다.

$$ D(p_{data}|p_{\theta}) = E_{x \sim p_{data}}[log (\frac{p_{data}(x)}{p_{\theta}(x)})]$$
$$= \cancel{E_{x \sim p_{data}}[log p_{data}(x)]} - E_{x \sim p_{data}}[log p_{\theta}(x)] $$

즉, KL-Divergence를 **최소화**하는 것은 Expected log-likelihood을 **최대화** 하는 작업과 동일하다는 것을 확인할 수 있습니다. 저희는 $arg \max\limits_{p_{\theta}} E_{x \sim p_{data}}[log p_\theta (x)]$ 을 최대화하는 방향으로 생성 모델 데이터를 모델에 학습시켜, 가장 이상적인 $p_{\theta}^{\star}$을 구할 수 있습니다.

$$arg \min\limits_{p_{\theta}} D(P_{data}||p_{\theta}) = arg \min\limits_{p_{\theta}} - E_{x \sim p_{data}}[log p_{\theta}(x)]$$
$$\therefore =  arg \max\limits_{p_{\theta}} E_{x \sim p_{data}}[log p_\theta (x)]$$