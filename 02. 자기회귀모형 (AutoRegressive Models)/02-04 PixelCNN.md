앞서 저희가 다룬 autoregressive model들인 FVSBN, NADE, MADE는 간단한 로지스틱 회귀, 심층 신경망 구조, 그리고 오토인코더를 사용했기 때문에, 이미지의 픽셀값 간의 고차원의 비선형성 상관관계를 담기에는 한계점이 존재합니다.

따라서 비교적 간단한 구조에서 합성곱 신경망 (CNN), 순환 신경망 (RNN)등 복잡한 시퀀스 모델이 autoregressive model들에 적용되기 시작하였고, 그 중 유명한 알고리즘으로는 CNN 구조를 사용해 이미지를 생성하는 PixelCNN이 있습니다.

<img src="https://wikidocs.net/images/page/228898/pixcelcnn1.png" width="80%">

$$p(x) = \prod\limits_{i=1}^{n^2} p(x_i|x_1, \cdot\cdot\cdot, x_{i-1})$$
$$p(x_i|x_{<i}) = p(x_{i,R,G,B}|x_{<i}) = p(x_{i,R}|x_{<i})p(x_{i, G}|x_{<i},x_{i,R})p(x_{i,B}|x_{<i},x_{i,R},x_{i, G}) $$

PixelCNN은 흑백의 BinaryMNIST와는 달리 RGB 3개의 채널을 가진 컬러 이미지를 입력으로 받으므로, $p(x_i|x_{<i})$의 조건부확률을 $R \rightarrow G \rightarrow B$ 순서의 joint distribution으로 나타냅니다.

![](https://wikidocs.net/images/page/228898/pixcelcnn1.png)

PixelCNN의 특징 중 하나로는 Convolution 작업을 수행할 때 데이터의 시점 $i$ 이후 데이터의 영향을 제거하기 위해 특수한 mask filter를 사용한다는 점이 있습니다. 해당 masking한 $3 \times 3$ 크기의 필터를 사용해 빠르게 작업을 수행할 수 있지만, 모델이 $i$ 시점 이전의 일부 데이터를 활용하지 못하는 **Blind spot** 문제가 발생합니다.

## Gated PixelCNN

$$y = tanh(W_{k,f} \times x) \times \sigma(W_{k,g} \times x) $$

Gated PixelCNN은 PixelCNN의 blind spot 문제를 horizontal stack과 vertical stack을 조합해 사각지대를 없앰으로서 해결했습니다. 이는 horizontal stack이 시점 $i$ 까지의 가로축 데이터에 대해 학습을 완료한 후, vertical stack이 아무 masking 없이 convolution 작업을 진행하기 때문입니다. 이 때문에 PixelCNN의 인식 범위에 사각지대가 발생하는 현상을 방지하면서 직사각형 형태로 인식 범위를 늘릴 수 있게 되었습니다.

![](https://wikidocs.net/images/page/228898/pixelcnn4.png)
