# styleGAN (쓰는중)
A Style-Based Generator Architecture For Generative Adversarial Networks

논문 링크 : https://arxiv.org/abs/1812.04948

## 0. Abstract
- PCGAN의 Generator model의 구조와는 다른 구조로 Generator model을 구성
- 사진에 Style을 scale-specific control하게 적용
> scale-specific control ? 이미지 합성에서 특정한 scale로 자유롭게 조절을 해가면서 style을 적용
- 본 논문에서는 Interpolation quality와 disentanglement를 측정할 수 있는 새로운 2가지 방법에 대해 소개
- 새로운 고해상도 사람 얼굴 데이터셋 공개

## 1. Introduction
- styleGAN은 PCGAN 구조에서 Generator Architecture 재구성하였다. 이로인해 PGGAN에서 불가능 했던 Image synthesis process control이 가능하게 되었다. 
- Learned constant input 즉, mapping network로 부터 학습된 입력 w로 부터 Generator가 Image를 생성하는데에 Intermediate latent vector w로 부터 얻은 각각의 style(w1,w2,...)들을 Conv Layer를 거치는 과정에서
조금씩 적용을 해 나간다. 이러한 과정으로 인해 Strength 또한 조절이 가능해진다.

- new automated metrics
1. perceptual path length
2. linear separability

## 2. Style-based generator
AdalN(Adaptive Instance Normalization) : ex) 사진을 특정 그림 style로 적용
### 2-1. PGGAN 
- latent vector Z가 normalization을 거쳐 모델에 바로 input value로 들어감 
- -> latent space는 반드시 training data의 probability density를 따라야함
- -> latent space가 entanglement하게 만들어진다.
### 2-2. StyleGAN
- lantent space에서 나온 latent vector Z를 model에 입력으로 바로 넣는게 아니라, 
mapping network를 거쳐 나온 w를 뽑아내게 된다.
- latent vector z -> Normalization -> mapping network(fc 8개로 생성) -> w를 뽑아냄
### Mapping Network
styleGAN은 PGGAN에서 latent space가 entanglement한 문제를 해결하기 위해,
훈련데이터의 확률분포를 따르지 않도록 하기 위해 비선형 함수인 Mapping network를 사용했다.
이로 인해 latent vector간의 상관관계를 줄이게 되어 latent space를 더 disentanglement하게 만들 수 있게 된다.

아래와 같이 mapping network를 거쳐 intermediate vector w로 먼저 변환 후 이미지를 생성한다.

### Style Module
생성된 w는 synthesis network가 이미지를 생성하는 과정에서 style를 입히는 데에 사용된다.


특징
- 특정 layer에서 입혀진 style은 바로 다음 convolutional layer에만 영향을 준다.
따라서 각 layer의 style이 특정한 visual attribute만 담당하는 것이 용이해진다.
- Style을 조정한다는 것은 이미지의 전체적인 정보를 통째로 조정한다는 것을 의미한다. 이로 인해 항상 spatially-consistent한 이미지를 얻게 되고, 기존의 generator보다 훨씬 안정적으로 자연스러운 이미지를 얻게된다.


## 3. Properties of the style-based generator
### 3-1. style Mixing
corelation 문제를 해결하기 위해 style mixing 기법을 사용함. 
input vector로 부터 w1, w2를 만들어냄. 이후 synthesis nework의 초반에는 w1으로 학습하다가 특정 layer
이후 부터는 w2를 적용하여 학습. 이때 style이 교체되는 layer를 매번 랜덤하게 결정함으로써 연속된 두 layer간 
style의 상관관계가 생기는 현상을 방지할 수 있다.
### 3-2. Stochastic Variation
stochastic하다고 볼 수 있는 요소들을 표현하기 위하여, synthesis network의 각 layer마다
random noise를 추가했다. 이로 인해 사실적인 이미지를 생성할 수 있으며 input latent vector는 이미지의
중요한 정보를 표현하는 데에만 집중할 수 있게 되고 이를 조절하는 것도 용이해진다.

## 4. Disentanglement studies
### 4-1 Perceptual path length

### 4-2. Linear separability
