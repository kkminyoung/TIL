# cycleGAN 
논문링크 : https://arxiv.org/abs/1703.10593

"Unpaired Image-to-Image Translation Using Cycle-Consistent Adversarial Networks"

## 0. Abstract
- Image-to-image translation 란? image 쌍의 train set을 이용해 input image와 output image를 mapping 하는 것이다.
- 본 논문에서는 쌍을 이루는 예시 없이 X라는 도메인으로부터 얻은 이미지를 target 도메인 Y로 바꾸는 방법을 제안한다. 목표는 Adversarial loss를 이용해, G(x)로부터의 분포와 Y로 부터의 분포가 불가능하도록 학습시키는 것이다. 이러한 mapping은 제약이 적기때문에, 역매핑을 함께 진행했고 F(G(x))가 X와 유사해지도록 강제하는 cycle consistency loss를 도입한다.
- collection stype transfer, object transfiguration, season transfer, photo enhancement와 같은 task에서 퀄리티가 좋았다.

## 1. Introduction
본 논문에서는 쌍을 이루지않는 데이터셋을 이용해 학습하는 방법에는 몇몇의 관계가 존재한다고 가정했다. 
y_hat과 y를 구분하도록, 즉 adversary하게 train된 model을 이용해 이미지 y와 구분되지 않는 아웃풋 y_hat=G(x) 를 산출하는 G: X->Y를 학습시킨다.
하지만 이런 식의 translation이 개개의 input x와 y가 의미있는 방식으로 짝지어지는 것을 보장하지 않는다.
실제로 단독 Advesarial objective는 학습시키기 어렵다.

위와 같은 보통의 과정은 mode-collapse를 일으킨다.(mode-collapse란 ? 어떤 input 이미지는 모두 같은 output 이미지로 mapping하면서 최적화에 실패하는 것)
목적함수에 구조를 추가하는 것이 필요하다. 따라서 translationdl 'cycle consistent'(주기적 일관성)이어야 한다는 속성을 이용한다. 
이 구조를 G와 F를 동시에 학습하고 F(G(x))~x, F(G(x))~x이게 하는 cycle consistency loss를 추가함으로써 적용한다. 
Cycle consistency loss와 adversarial losses를 X와 Y에 적용함으로써 전체 목적함수가 완성된다.

## 2. Related Work
- GAN
- Image-to-Image Translation
- Unpaired Image-to-Image Translation
- Cycle Consistency
- Neural Style Transfer

## 3. Formulation
목표는 주어진 도메인 X와 Y를 mapping하는 함수를 학습하는 것이다.

- Adversarial Loss

<img width="206" alt="cGAN1" src="https://user-images.githubusercontent.com/61506233/89123399-03e5c300-d50a-11ea-8f57-1e70c8b0beae.PNG">

- Cycle Consistency Loss

<img width="167" alt="cGAN2" src="https://user-images.githubusercontent.com/61506233/89123402-0516f000-d50a-11ea-874a-2fb5b2e8cd2d.PNG">

mode collapse 문제가 생길 수 있기에, 위의 Adversarial Loss 만으로는 학습을 보장하기 어렵다. 따라서 가능한 mapping 함수의 공간을 줄이기 위해 cycle-consistent 해야하기 때문에 사용

- Full Objective

<img width="162" alt="cGAN3" src="https://user-images.githubusercontent.com/61506233/89123403-05af8680-d50a-11ea-92a7-b2353ee7c411.PNG">
<img width="165" alt="cGAN4" src="https://user-images.githubusercontent.com/61506233/89123404-05af8680-d50a-11ea-8f6d-ef0ae350671e.PNG">



## 4. Implementation 5. Results
<img width="434" alt="cGAN5" src="https://user-images.githubusercontent.com/61506233/89123405-06481d00-d50a-11ea-885d-4e7a7f559503.PNG">
-> 코드 참고

## 6. Limitations and Discussion
대부분의 경우 성능이 좋았다. (특히 색깔과 질감 변화와 같은 translation 작업) 하지만 실패하는 경우도 있었는데 분석 결과 다음과 같은 예시들이 있었다. 1. 개->고양이 와 같은 경우, 입력에 최소한의 변화를 주는 것으로 변질된다. 2. 이미지 셋 분포의 특징에서 차이가 난다. (말->얼룩말 : 말을 타는 사람의 이미지는 많지만, 얼룩말을 타는 사람의 이미지는 없기때문.) 3. 사진의 출력 -> 라벨링 작업에서 나무와 건물에 대한 라벨을 허용한다. 이런 모호성을 해결하기 위해 semantic supervision이 필요하다. 
