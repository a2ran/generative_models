계산이 불가능한 $\sum\limits_{z}p(x,z)$을 계산하기 위해 구하기 쉬운 분포를 찾아 근사하는 변분 추론 방법을 수행하기 위해 임의의 가우시안 분포 $q$를 편의를 위해 추가하겠습니다.

$$log(\sum\limits_{z}p_{\theta}(x,z)) = log(\sum_{z}\frac{q(z)}{q(z)}p_{\theta}(x,z)) = log(E_{z \sim q(z)}[\frac{p_{\theta}(x,z)}{q(z)}])$$

이렇게 구하기 쉬운 임의의 가우시안 분포 $q$를 추가한 이유는 바로 marginal likelihood learning의 학습 목표 식을 몬테 카를로 방법으로 근사하기 위해서입니다. 

**몬테 카를로 방법**

$$E_{z \sim p}[f(z)] = \sum\limits_{z}f(z)p(z)$$

저희가 03-02강에서 학습한 몬테 카를로 방법의 정의에 따라, $f(z)$을 $log(\frac{p_{\theta}(x,z)}{q(z)})$으로, $p$를 $q$로 바꿔주면 몬테 카를로 방법을 통해 계산할 수 있습니다.

$$log(E_{z \sim q(z)}[\frac{p_{\theta}(x,z)}{q(z)}]) \rightarrow E_{z \sim q(z)}[log(\frac{p_{\theta}(x,z)}{q(z)})]$$

<img src="https://wikidocs.net/images/page/229774/elbo1.png" width="80%">

이 과정을 수행하기 위해 아주 유명한 추론 방식인 **Jensen's Inequality**를 사용하겠습니다. $log$ 함수는 $log(px + (1-p)y) \ge plog(x) + (1-p)log(y)$를 만족하는 오목함수(concave function)입니다. 즉 위의 식은 다음과 같은 성질을 만족합니다.

$$log(E_{z \sim q(z)}[\frac{p_{\theta}(x,z)}{q(z)}]) \ge E_{z \sim q(z)}[log(\frac{p_{\theta}(x,z)}{q(z)})]$$

$f(z)$을 $log(\frac{p_{\theta}(x,z)}{q(z)})$으로 설정하면, 다음과 같은 식으로 정리해 몬테 카를로 방법을 적용해 계산을 완료할 수 있습니다. 해당 방법을 종합한 과정이 바로 **Evidence Lower Bound (ELBO)**입니다.

$$log(E_{z \sim q(z)}[\frac{p_{\theta}(x,z)}{q(z)}]) =  log(E_{z \sim q(z)}[f(z)]) = log(\sum\limits_{z}q(z)f(z)) \ge \sum\limits_{z}q(z)logf(z)$$

즉, Evidence Lower Bound는 계산이 용이한 임의의 가우시안 분포 $q$를 설정해 계산이 불가능한 Evidence $logp(x;\theta)$에 계산이 가능한 lower bound를 설정해 해당 lower bound를 maximize하는 방식으로 $logp(x;\theta)$을 구하는 과정입니다. 이 lower bound를 maximize하는 과정이 ELBO이고, ELBO는 Variational Inference의 종류 중 하나입니다.

$$ \begin{align*}
\log p(x; \theta) &\geq \sum_z q(z) \log \frac{p_\theta(x, z)}{q(z)} \\
&= \sum_z q(z) \log p_\theta(x, z) - \sum_z q(z) \log q(z) \\
&= \sum_z q(z) \log p_\theta(x, z) + H(q)
\end{align*}
$$

$$ELBO(q) =  \sum_z q(z) \log p_\theta(x, z) + H(q)$$