# DCGAN (Deep Convolutional Generative Adversarial Networks)
- 논문 링크 :  https://arxiv.org/abs/1511.06434
- Unsupervised Representation Learning with Deep Convolutional Generative Adversarial Networks - Alec Radford el ec, 2016


## 0. Abstract
본 논문에서는 CNN을 비지도학습에 적용하는 DCGAN을 소개한다.

## 1. Introduction
- 기존의 GAN :  종종 불완전한 결과물을 가져온다. 학습과정을 알 수 없어 제한적이었다. (black box)
- DCGAN :  
> 1) 안정적으로 학습이 되는 convolution을 GAN에 결합한 구조를 제안한다.
> 2) GAN 학습과정에서 필터들을 시각화하였다.
> 3) 다른 비지도학습 알고리즘과의 비교를 통해 성능을 평가한다.
> 4) latent vector에서 산술 특성을 가지고 있다는 것을 보여줬고 생성된 이미지의 특성을 조작할 수있다.

## 2. Related Work
- Representation Learning From Unabled Data
- Generating Natural Images
- Visualizing The Internals Of CNNs

## 3. Approach And Model Architecture

1. Max-pooling layer를 없애고 stride convolution이나 fractional-strided convolution을 사용하여 feature-map의 크기 조절
2. Batch normalization 적용(생성된 이미지들이 하나의 동일한 이미지로 만들어지는 collapsing 문제를 예방하는데에 효과적.)
3. Convolution feature의 마지막 layer에서 Fully connected layer 제거(global averaging pooling)
4. G의 activation으로 Tanh를 사용하고, 나머지 layer는 ReLU를 사용
5. D의 activation으로 모든 layer에 LeackyReLU를 사용


## 4. Details Of Adversarial Traning
![DCGAN2](https://user-images.githubusercontent.com/61506233/88564265-f8a81880-d06d-11ea-8092-47b1a70a305f.PNG)
- LSUN(Large-scale Scene Understanding) : 침실 사진 dataset
overfitting이나 memorizing을 통해 좋은 결과를 내는 것이 아님을 보였다.
- Faces 
- ImageNet-1K


## 5. Empirical Validation Of DCGANs Capabilities

- Classifying CIFAR-10 Using GANS as a Feature Extractor

- Classifying SVHN Using GANs as a Feature Extractor

## 6. Investigating And Visualizing The Internals Of The Networks
- Walking in the Latent space
- Visualizing the Discriminator Features
- Manupulating the Generator Representating
: 그 결과, 그 위치에 다른 오브젝트를 채워넣었다.

## 7. Conclusion and Feature Work
