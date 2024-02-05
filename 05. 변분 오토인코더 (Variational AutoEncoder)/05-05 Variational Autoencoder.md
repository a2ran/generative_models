앞서 저희가 4강부터 학습해온 과정이 바로 변분 오토인코더 (Variational Autoencoder) 생성 모델입니다. 즉 변분 오토인코더는 $p(x|z)$와 $q(z|x)$을 추론하는 생성 모델이고,각각의 분포는 파라미터 $\theta$와 $\phi$를 최적화함으로서 구할 수 있습니다.

<img src="https://wikidocs.net/images/page/229887/vae1.png" width="70%">

변분 오토인코더의 최종 학습 목표는 다음과 같으며, 가능도 $L(x; \theta, \phi)$를 전개하면 다음과 같습니다.

$$\max\limits_{\theta}l(\theta;D) \ge \max\limits_{\theta, \phi} \sum\limits_{x^i \in D} L(x^i; \theta, \phi) $$

$$L(x; \theta, \phi)  = E_{q(z|x;\phi)}[\log p(z, x;\theta) - \log q_{\phi}(z|x)] $$
$$= E_{q(z|x;\phi)}[\log p(z, x;\theta) - \log p(z) + \log p(z) - \log q_{\phi}(z|x)]$$
$$=  E_{q(z|x;\phi)}[\log p(z, x;\theta)] - D_{KL}(q_{\phi}(z|x)||p(z))$$

1. $\theta^{(0)}$, $\phi^{(0)}$ 초기화
2. 랜덤하게 $x^i$를 데이터셋 $D$로부터 랜덤하게 추출
3. $x^i$을 $q(z|x;\phi)$을 통해 $\hat{z}$으로 매핑 **(인코더!!)**
	* $D_{KL}(q_{\phi}(z|x)||p(z))$에 따라 매핑한 $\hat{z}$는 기존 $p(z)$와 유사합니다.
4. 다시 $p(x|\hat{z}; \theta)$을 통해 $\hat{x}$으로 복원 **(디코더!!)**
	* $E_{q(z|x;\phi)}[\log p(z, x;\theta)]$에 따라 복원한 $\hat{x}$은 기존 $x^i$와 유사합니다.

즉 여태까지 저희가 다루어온 $\phi$ 파라미터를 최적화하는 과정은 변분 오토인코더의 인코더 부분을 구축하는 과정이었고, $\theta$ 파라미터를 최적화하는 과정은 변분 오토인코더의 디코더 부분을 구축하는 과정이었습니다.

<img src="https://wikidocs.net/images/page/229887/vae2.png" width="70%">

변분 인코더에서 latent space $z$가 중요하다고 한 이유도, $D_{KL}(q_{\phi}(z|x)||p(z))$을 통해 기존 $x$을 $q_{\phi}(z|x)$으로 매핑해 $p(z)$에 trivial 정도로 같게 설정한다면, $z$의 내용을 $p(z)$으로부터 추출해 생성할 수 있기 때문입니다.

위 변분 인코더의 예시에서 이미지 $x$을 태그 $z$으로 $\phi$와 $\theta$를 최적화하는 과정으로 매핑하고 복원하는 과정을 학습시켜 $x^i$를 성공적으로 $\hat{z} \sim q_{\phi}(z|x^i)$을 통해 $p(z)$을 구할 수 있게 되고, 해당 $p(z)$로부터 $\hat{z} \sim p(z)$을 통해 태그를 생성할 수 있습니다!