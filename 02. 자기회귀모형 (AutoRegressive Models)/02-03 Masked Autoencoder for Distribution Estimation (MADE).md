<img src="https://wikidocs.net/images/page/228894/made2.png" width="60%">

얼핏 보면, AutoRegressive model인 FVSBN과 NADE는 저희에게 이미 익숙한 오토인코더 (autoencoder)와 비슷한 양상을 보이고 있습니다.

위의 오토인코더는 두 개의 인코드 은닉층 $W^1, W^2$과 한 개의 decoder 은닉층 $V$을 보유하고 있으며,

인코더 $e(\cdot)$: $e(x) = \sigma(W^2(W^1x + b^1) + b^2)$<br>
디코더 $d(e(x)) ≈ x$: $d(h) = \sigma(Vh + c)$

오토인코더의 인코더와 디코더는 심층 신경망 구조를 사용한 NADE와 상당히 유사한 구조를 보이고 있습니다. 하지만 오토인코더는 Autoregressive 모델들과는 다르게 생성 모델이 아닙니다. 그 이유는 즉슨 오토인코더는 인코더로 압축한 $e(x)$가 입력 데이터 $x$의 유의미한 feature를 담아내게 학습하는 비지도 학습 알고리즘이고, 예측 시점 이후의 데이터 $x_{>i}$을 가지고 학습을 진행하기 때문입니다.

## Autoregressive Models are Masked Autoencoders

하지만, 만일 기존 오토인코더에서 예측 시점 이후의 데이터 $x_{>i}$의 영향을 제거할 수 있다면, 이 구조를 $x_{<i} \rightarrow \bar{x}_i$의 예측에 사용할 수 있을까요? 정답은 "그렇다"이고, 그러한 작업을 수행하는 autoregressive model이 바로 Masked Autoencoder for Distribution Estimation (MADE)입니다.

<img src="https://wikidocs.net/images/page/228894/made1.png" width="100%">

MADE는 의도적으로 오토인코더의 일부 weight을 $0$으로 설정하는 마스킹 (masking) 작업을 통해 노드에서 노드 간 연결을 제거했고, 이를 통해 학습 시점 이후 데이터의 영향을 없앨 수 있었습니다.

이러한 작업은 은닉층 각각의 노드에 $1$ 부터 $n-1$까지 랜덤한 순서를 배부함으로 시작합니다. 이후 은닉층에서 은닉층, 그리고 은닉층에서 출력층으로 가기 까지 노드 ($i$)로부터 순서가 같거나 높은 노드 ($\ge i$)의 연결을 유지하고 나머지 연결은 마스킹하는 방식으로 autoregressiveness을 유지합니다.

예를 들어, $x_1^1$의 노드 $1$의 숫자가 배정되었기 때문에 $h_1^1, h_1^2, h_1^3, h_1^4, h_1^5$ 첫 번째 은닉층 다섯 개의 노드에 전부 연결할 수 있습니다. 하지만, 첫 번째 노드에서 두 번쨰 노드로 연결하는 경우 $h_1$의 숫자가 $h_2$의 숫자보다 적어야 하므로 높은 숫자의 연결에 대한 마스킹이 이루어지고, 이것이 출력층까지 도달할 때까지 이어지면서, 자연스럽게 시점 이후 데이터의 영향에 대한 제거작업을 완료할 수 있습니다.