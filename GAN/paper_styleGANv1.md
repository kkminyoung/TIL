# styleGAN 
A Style-Based Generator Architecture For Generative Adversarial Networks

논문 링크 : https://arxiv.org/abs/1812.04948

## 0. Abstract
- PCGAN의 Generator model의 구조와는 다른 구조로 Generator model을 구성
- 사진에 Style을 scale-specific control하게 적용
- scale-specific control ? 이미지 합성에서 특정한 scale로 자유롭게 조절을 해가면서 style을 적용
- 본 논문에서는 Interpolation quality와 disentanglement를 측정할 수 있는 새로운 2가지 방법에 대해 소개
- 새로운 고해상도 사람 얼굴 데이터셋 공개

## 1. Introduction
- G를 통한 이미지 합성 과정은 black-box이기 때문에 합성되는 이미지의 속성을 조절하기가 어렵다는 한계점이 존재하였다. 또한 latent space의 속성에 대한 이해가 부족하기 때문에 다른 G와 비교할 수 있는 질적 방법이 없었다.
- styleGAN은 PCGAN 구조에서 Generator Architecture 재구성하여 PGGAN에서 불가능 했던 이미지 합성 과정 조절이 가능하게 되었다.
- Learned constant input 즉, mapping network로 부터 학습된 입력 w로 부터 Generator가 Image를 생성하는데에 Intermediate latent vector w로 부터 얻은 각각의 style(w1,w2,...)들을 Conv Layer를 거치는 과정에서
조금씩 적용을 해 나간다. 이러한 과정으로 인해 Strength 또한 조절이 가능해진다.
- 풀어서 말하면 여러가지 Z값들을 mapping network에 input으로 넣어주어 w라는 여러개의 값이 나오게 된다.
- 이런식으로 style들을 conv layer를 거치는 과정에 조금씩 적용하면 strength 또한 조절이 가능하다.

- new automated metrics(disentanglement를 정량화하기위함)
1. perceptual path length
2. linear separability

- disentanglement : latent space가 linear한 구조를 가지게 되어서 하나의 latent vector Z를 움직였을 때, 정해진 어떠한 하나의 특성이 변경되게 만들고자 하는 것.
- 즉, latent vector Z의 specific한 값을 변경했을 때 생성되는 이미지 하나의 특성들만 영향을 주게 만들었다고 하면(머리카락 길이, 사람의 시선 등) 이 모델의 latent space는 disentanglement 하구나 할 수 있음

## 2. Style-based generator
***AdalN(Adaptive Instance Normalization) : ex) 사진을 특정 그림 style로 적용

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

### 4-1 Mapping Network(위에서 연장되는 개념)

- entanglement한 것을 피하기 위해서 training data의 probability density를 따르지 않도록 하기 위해 Nonlinear function인 Mapping network를 사용했고, 

  - 이렇게 비선형 f를 통과시키면 training data의 분포에 따라갈 필요가 없으면서 feature들 사이의 편중된 상관관계를 줄여줄 수 있음

  

- 이로 인해서 latent vector간의 correlation을 줄일수 있어서 latent space를 더 disentanglement하게 만들 수 있다.

  - z의 특정한 값을 바꿧을 때 생성되는 이미지의 하나의 특성만 영향을 주게 되면 disentanglement라고 함

- 여기서 사용된 latent space, FC layer, W의 차원 모두 512임

- 이렇게 Mapping network를 이용해서 z값을 w값으로 만들어 주어서 이것을 Synthesis network에 style을 입혀줘야한다.
  - w는 constant tensor가 이미지로 변환되는 과정에서 스타일을 입히는 역할을 수행함. 그렇기에 다양한 스타일의 이미지를 만들어낼 수 있음.
- 여기서 key point는 중간 중간에 Style을 입혀 주면서 학습하는게 큰 특징임



- w에 매핑하는 것의 장점: 고정된 분포를 따르지 않음. 학습된 F에 의해 정해지기에 warping(틀어짐)이 많이 일어나지 않음. 그렇기에 factors of variation은 더욱 linear하고, disentangled하다고 할 수 있음



### 4-2 Synthesis Network

- 처음에 나온 layer는 const(상수, 고정) 4x4x512.
- 기존에는 random input을 사용해서 G의 초기 이미지를 생성하는데 여기서는 image features가 w와 AdalN으로 조절이 되기에 초기 입력을 생략하고, 
- 이미 학습되어진 tensor Const 4x4x512(데이터 분포의 평균으로 이루어진 텐서)로 데체
- 이렇게 하면 좋은점이 network가 input vector에 의존하지 않고 mapping network를 거쳐 나온 w로만 사용을 하는게 학습시키기 더 쉽다.



- 4x4x512로 시작해서 1024x1024x3으로 끝나는 8개의 layer로 이루어져 잇음.

### 4-2 AdaIN(Adative Instance Normalization), Style Module

- 스타일을 입혀주는 과정
- upsample, conv layer 이후에 적용을 시켜서 style을 입힘.
- w는 1x512인데 Affine Transformation을 거처셔 2x512 이런식으로??. 그냥 그 style scale이랑 style bias를 만들어내서 IN을 해주는거야. 
- 즉, conv output에서 각 채널들을 먼저 정규화시키고 scale, bias를 적용

- 매 layer마다 AdaIN을 통해서 스타일을 normalize한 후에 새로운 스타일을 입히게 되므로 특정 layer에서 입혀진 style은 바로 다음 conv layer에만 영향을 끼침. 
- 그렇기에 각 layer의 style이 특정한 visual attribute만 담당하는 것이 용이해짐.

### 4-3 Noise

- AdaIN이 이미지의 큰 부분들을 바꾸는 방법이라면 세세한 부분을 바꾸기 위해서 (stochastic variation) noise를 더하는 방법 사용
- 큰 부분은 인종, 성별 등등, 작은 부분은 수염, 머리카락, 주름살의 위치 등.
- 방법은 AdaIN과 똑같고 랜덤한 가우시안 노이즈를 각 채널별로 집어넣었음.

