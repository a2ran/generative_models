<img src="https://wikidocs.net/images/page/228900/wavenet1.png" width="80%">

WaveNet은 오디오 데이터를 생성하는 Autoregressive 모델입니다. WaveNet은 $\hat{x_{i+1}}$를 예측하기 위해 여러 은닉층 $h$와 이전 시점의 데이터 $x_{<i}$를 기반으로 한 조건부 확률 $p(x_{i+1}|x_{<i}; \alpha)$을 계산한다는 점에서 저희가 전에 살펴본 MADE (Masked Autoencoder)와 비슷한 양상을 보이지만, 해당 구조보다 노드와 노드간 연결이 확실히 산발적(sparse)임을 확인할 수 있습니다.

그 이유는 바로 WaveNet이 학습하는 오디오 데이터가 엄청나게 큰 고차원의 데이터이기 때문입니다. Ex) 44.1 kHz의 오디오 데이터는 1초에 44,100개의 데이터를 추출합니다. 따라서, 기존 Masked Autoencoder를 사용하기에는 컴퓨팅 용량에 대한 압박이 이미지, 텍스트와 같은 다른 고차원 데이터를 생성할 때 보다 크게 다가옵니다.

이를 해결하기 위해 WaveNet은 **Casual Convolution** 방법과 **Dilation**방법을 도입했습니다.

## Casual Convolution

<img src="https://wikidocs.net/images/page/228900/wavenet2.png" width="60%">

WaveNet은 $x_{<i}$ 데이터를 매개변수화 (parameterize)하기 위해 PixelCNN과 같이 합성곱 신경망 (CNN)을 사용합니다. 따라서, 2D 이미지에서 데이터 $i$ 이후 데이터의 영향을 제거하기 위해 masking 필터를 적용했던 PixelCNN과 마찬가지로, WaveNet은 1D인 시퀀스 데이터의 인식 범위 (receptive field)에 masking 필터를 적용해 기존 symmetric한 형태를 보인 인식 범위를 절반으로 나누어 $\le i$ 까지의 데이터를 가지고 학습하도록 수정합니다.

## Dilated Convolution

<img src="https://wikidocs.net/images/page/228900/wavenet3.png" width="80%">

WaveNet은 또한 모델의 용량을 최적화하기 위해 기존 $3 \times 3$ Convolution Filter를 확장해(dilate) 필터가 기존의 파라미터 개수를 유지하면서도 보다 산발적으로 이전 시점의 데이터를 학습하도록 설정하는 Dilated Convolution 방법을 사용합니다.

<img src="https://wikidocs.net/images/page/228900/wavenet4.png" width="80%">

앞서 Casual Convolution 방식을 통해 CNN 구조를 적용한 Autoregressive 모델을 학습시킨다면, 해당 모델은 $>{i+1}$ 시점 데이터의 영향을 받지 않으면서 $i+1$ 시점 데이터를 예측하는 Autoregressive 모델의 역할을 수행할 수 있지만, 노드와 노드간 학습해야할 파라미터가 매우 집약적이기 때문에, 노드와 노드간 파라미터 $w_i$를 동일하게 유지하는 파라미터 공유 (parameter sharing) 방법을 사용해도 총 $O(2(k+1)n) ≈ O(kn)$개의 파라미터를 학습해야 합니다. 여기서 $k$는 은닉층의 숫자이고, $n$은 데이터의 개수입니다.

<img src="https://wikidocs.net/images/page/228900/wavenet5.png" width="80%">

하지만, 산발적으로 데이터의 특징을 추출하는 Dilated Casual Convolution 방식을 사용한 WaveNet 모델을 학습시킨다면, 모델은 은닉층을 지날 때 마다 노드와 노드간 연결 간격을 2 배로 확장하기 때문에, 파라미터 공유 방법을 사용해 총 파라미터 수를 $O(2n(1 - (\frac{1}{2}))^k) ≈ O(n)$개로 압축할 수 있습니다.

이처럼 WaveNet은 이후 시간대 데이터의 영향을 제거하는 Casual Convolution 방법과 매 레이어마다 확장된 필터를 사용하는 Dilated Casual Convolution 방법을 사용함으로서 성공적으로 고차원의 오디오 데이터의 분포 $p(x)$을 추론해 생성할 수 있습니다.