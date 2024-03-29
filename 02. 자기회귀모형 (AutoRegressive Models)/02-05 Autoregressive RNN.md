합성곱 신경망 (CNN)과 마찬가지로 순환 신경망 (RNN) 또한 Autoregressive Model에 적용해 이전 시간대의 데이터를 기반으로 현재 시간대의 확률 분포를 예측할 수 있습니다.

<img src="https://wikidocs.net/images/page/228899/rnn2.png" width="80%">

자연어 처리 모델을 학습하면서 흔히 발생하는 문제 중 하나는, 입력 데이터인 자연어 문장 $x_{1:t-1}$이 매우 길어질 수 있다는 점에서 발생합니다. 순환 신경망은 해당 문제점을 은닉층 $h_t$을 통해 해결합니다. 은닉층 $h_t$는 $t$ 시점까지 정보의 요약으로 간주할 수 있습니다. 

RNN 모델이 조건부 확률 $p(x_t|x_{1:t-1}; \alpha^t)$을 추정하는 과정은 다음과 같습니다. 먼저 데이터를 입력받기 전 은닉층을 $h_0$으로 초기화 (initialize) 한 다음, 새로운 입력 $x_{t+1}$이 주어지면 은닉층 $h_{t+1}$을 이전 시점의 은닉층 $h_t$와 새로운 입력 $x_{t+1}$을 가지고 업데이트합니다.  그 후 은닉층 $h_{t+1}$을 가지고 새로운 토큰 $o_{t+1}$을 예측함으로서, 기존 $p(x_t|x_{1:t-1}; \alpha^t)$으로부터 예측하는 경우보다 컴퓨팅 자원을 절약할 수 있습니다.

$$h_{t+1} = tanh(W_{hh}h_t + W_{xh}x_{t+1} )$$
$$o_{t+1} = W_{hy}h_{t+1}$$

RNN 모델은 이와 같이 자연어 처리 작업에 적용해 여러 태스크를 수행할 수 있지만, 시퀀스의 길이가 길어지면 발생하는 기울기 소실과 폭주 (vanishing/exploding gradients)문제가 존재합니다. 또한 RNN모델의 시간 복잡도는 입력 시퀀스 $x$의 길이 $n$에 비례합니다. 즉, RNN의 파라미터인 $W_{hh}, W_{xh}, W_{hy}$를 $n$번 업데이트 해야하므로, big-O-notation은 $O(N(XH + H^2 + HY))$으로 RNN의 느린 학습 속도에 크게 기여합니다.

RNN의 이러한 기울기 소실 문제점과 시간 복잡도 문제를 해결한 프레임워크가 바로 트랜스포머 (transformers) 아키텍쳐이고, 현 시점 대규모 언어 모델(Large Language Models)들은 트랜스포머 아키텍쳐를 기반으로 학습한 알고리즘들입니다. 해당 내용은 챕터 4-7에서 상세히 다루겠습니다.