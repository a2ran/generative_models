
<img src="https://wikidocs.net/images/page/229520/gmm1.png" width="60%">

대표적인 잠재변수모형으로는 가우시안 혼합 모델 (Gaussian Mixture Models)이 있습니다. 가우시안 혼합 모델은 레이블 되어있지 않은 데이터를 가우시안의 분포들로 클러스터링하는 알고리즘입니다. 예를 들어, 2차원의 데이터를 $N(\mu_k, \Sigma_k)$의 분포를 지니는 3개의 가우시안 분포로 나타낸다면,

* $p(z)$ **(prior)** $= \text{Categorical}(1, \cdot\cdot\cdot, K)$
* $p(x|z=k)$ **(Conditional)** $= N(\mu_k, \Sigma_k)$

<img src="https://wikidocs.net/images/page/229520/gmm2.png" width="100%">

으로 나타낼 수 있습니다. 가우시안 혼합 모델은 하나의 분포를 여러 가지의 가우시안 분포로 나타내게 함으로서 기존 하나의 가우시안 분포로는 수행할 수 없는 복잡한 작업을 수행하게 할 수 있습니다. 해당 작업을 통해 $x$의 특징 $z$에 따른 클러스터의 분포로부터 새로운 데이터를 생성해 $z$의 특징을 가진 데이터를 생성하는 Representation Learning이 가능합니다.
