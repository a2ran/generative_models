<img src="https://wikidocs.net/images/page/229523/deeplatent1.png" width="70%">

앞서 학습했던 가우시안 혼합 모델에 심층 신경망 구조를 적용해 심층 신경망 모델이 가우시안 분포의 $\mu_k$와 $\Sigma_k$을 학습하도록 설정할 수 있습니다.

* 이 경우 주어진 정보인 prior는 $p(z) = N(0,1)$입니다. 이전 가우시안 혼합 모델에서는 데이터의 분포가 $= \text{Categorical}(1, \cdot\cdot\cdot, K)$ 중에 있다는 정보가 있었지만, 해당 심층 신경망 구조는 $N(0,1)$ 전체 분포를 대상으로 학습합니다.
* 생성 모델이 학습하는 목표는 $p(x|z)$는 $N (\mu_{\theta}(z), \Sigma_{\theta}(z))$이고, $\mu_{\theta}(z)$와 $\Sigma_{\theta}(z)$은 각기 다른 심층 신경망 구조입니다. 이를테면 두 개의 가우시안 분포를 지닌 데이터를 심층 신경망으로 분류하기 위해서,
	* $\mu_{\theta}(z) = \sigma(Az + c)$ = $\sigma(a_1z+c_1, \sigma(a_2z + c_2))=(\mu_1(z), \mu_2(z))$
	* $\Sigma_{\theta}(z) = diag(exp(\sigma(Bz+d)))=\begin{bmatrix} exp(\sigma(b_1z+d_1)) & 0 \\ 0 & exp(\sigma(b_2z+d_2)) \end{bmatrix}$
	* $\theta=(A,B,c,d)$

<img src="https://wikidocs.net/images/page/229523/deeplatent2.png" width="70%">

즉, 하나의 가우시안 분포로 나타내기 어려운 $p(x|z)$을 나타내기 위해 $p(x|z)$을 구성하는 각각의 가우시안 분포 $N (\mu_{\theta}(z), \Sigma_{\theta}(z))$를 학습하고, 해당 가우시안 분포들을 합침으로서 본래 심층신경망이 목표하는 $p(x|z)$을 구할 수 있습니다.