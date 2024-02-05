비지도 학습(Unsupervised Learning)은 학습 데이터 $X$만을 사용하며, 정답 레이블 $Y$없이 학습을 진행합니다. 비지도 학습은 주어진 데이터 $X$ 내에 숨겨진 구조나 패턴을 발견하고 이해하는 데 목적을 두고 있습니다.

* 입력 데이터: $(X)$
* 목적: 데이터 $X$ 내의 숨겨진 구조나 패턴 학습

![](https://wikidocs.net/images/page/228783/unsupervised.png)

비지도 학습은 데이터의 내재된 특성을 탐색하고, 이를 통해 데이터를 더 잘 이해하거나 표현하는 데 사용됩니다. 예를 들어, 차원 축소는 비지도 학습의 한 형태로, 데이터의 용량을 줄이면서도 중요한 정보를 유지하고자 할 때 사용합니다. 이 과정에서 주성분 분석(PCA, Principal Component Analysis) 같은 알고리즘을 사용할 수 있습니다. PCA는 데이터의 분산을 최대화하는 방향으로 차원을 축소함으로써 데이터의 중요한 특성을 보존하려고 합니다.

비지도 학습의 예시

**클러스터링(Clustering)**: 데이터를 유사한 특성을 가진 여러 그룹으로 나누는 작업 .
> K-means 클러스터링: 데이터 포인트들을 K개의 클러스터로 그룹화

**차원 축소(Dimensionality Reduction)**: 데이터의 특성 수를 줄이면서도 중요한 정보를 유지하는 작업. 
> PCA, t-SNE

**피쳐 학습(Feature Learning)**: 데이터에서 유용한 특성을 자동으로 학습하는 작업.
> 오토인코더(Autoencoders)

**밀도 추정(Density Estimation)**: 데이터가 공간 상에서 어떻게 분포하는지를 추정하는 작업.
> 가우시안 혼합 모델(Gaussian Mixture Models)

비지도 학습은 레이블이 없거나 레이블을 얻기 어려운 상황에서 유용하게 사용할 수 있으며, 데이터의 숨겨진 특성을 발견하고 이해할 수 있습니다.

## 보충 자료

[비지도 학습이란 무엇입니까?](https://www.tibco.com/ko/reference-center/what-is-unsupervised-learning)<br>
[비지도학습 - 전통적인 기계학습과 딥러닝에서의 비지도학습](https://velog.io/@8068joshua/%EB%B9%84%EC%A7%80%EB%8F%84%ED%95%99%EC%8A%B5-%EC%A0%84%ED%86%B5%EC%A0%81%EC%9D%B8-%EA%B8%B0%EA%B3%84%ED%95%99%EC%8A%B5%EA%B3%BC-%EB%94%A5%EB%9F%AC%EB%8B%9D%EC%97%90%EC%84%9C%EC%9D%98-%EB%B9%84%EC%A7%80%EB%8F%84%ED%95%99%EC%8A%B5)