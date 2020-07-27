# Generative Adversarial Nets
논문 링크 : https://papers.nips.cc/paper/5423-generative-adversarial-nets.pdf

## 0. Abstract
- Adversarial Process을 통해, Generative Model을 evaluation 하는 새 Framework 제안
- G(Generator) : data의 분포를 train하는 모델
- D(Descriminator) : train data로 부터 나왔을 확률을 추정하는 감별 모델

- G의 train 과정은 D가 실수할 확률을 최대화 하는 것
- G는 훈련 데이터의 분포를 복구, D는 항상 1/2이 되는 고유한 솔루션이 존재
- 즉, G는 훈련데이터의 분포를 학습하여, 임의의 노이즈를 훈련 데이터와 같은 분포로 생성하고, D는 해당 인풋이 생성된 이미지인지, 훈련 데이터로 부터 나온 이미지인지에 대한 확률이 1/2이 되도록 한다.
- G와 D는 multilayer perceptrons로 정의되고, backpropagation으로 학습된다.

## 1. Introduction
- D는 sample이 G로 부터 생성되었는지, training data로 부터 왔는지 판별하는 것을 학습한다.
- 본 논문에서, G가 random noise를 multilayer perceptron을 통해 생성하고, D가 multilayer perceptron인 특수한 경우를 살펴본다. (Adversarial Nets)
- 두 model을 오직 backpropagation과 dropout 알고리즘만으로 학습할 수 있으며, G로 부터 나오는 sample은 forward propagation으로 생성된다.

## 2. Related Work
VAE, GAN

## 3.Adversarial Nets
- G(z,theta_g) : data에 대한 G의 분포를 학습하기 위해, 사전에 input noise를 정의하고, data space로의 mapping . (미분 가능)
- D(x, theta_d) : D(x)는 x가 data로 부터 온 확률을 나타낸다. training 예제와 G로 부터 오는 sample에 올바른 라벨을 지정할 확률을 최대화 하도록 D를 train. (single scaar) 
- 동시에 (1-D(G(z)))를 최소화 하도록 G를 train. 즉, D와 G는 2인 minmax

![1](https://user-images.githubusercontent.com/61506233/88512529-accf8200-d021-11ea-9ed5-9f4bc072a0a2.png)

- max D : discriminative model이 구분을 잘 한다면, D(x) = 1, D(G(x)) = 0. 즉, log(D(x)) 및 log(1-D(G(x)))는 무한대. 따라서 위 식을 maximize해야 D가 잘 학습된다.
- min G : generative model이 잘 생성한다면, discriminative model은 G(z)를 잘 구분하지 못하게 되며, D(G(z)) = 1이 된다. 따라서 log(1 - D(G(z))) = log(0) 은 -무한대. 따라서 위 식을 minimize해야 G가 잘 학습된다.

- D를 optimizing하는 것으로 training을 완료하면 overfitting이 될 수 있으므로, G optimizing 한 번에 D를 k step optimizing 한다.

## 4. Theoretical Results
- global optimum : p_g = p_data 일 때
- 실제 분포에서 G가 data를 생성했을 때, 이때 Discriminator가 제대로 분류 x. 1/2 확률로 분류하게 됨

![2](https://user-images.githubusercontent.com/61506233/88512533-ae00af00-d021-11ea-835c-0e33a8d8296c.png)

- 파랑색 : Descriminative distribution(0=가짜, 1=실제 data distribution으로 부터 온 data)
- 검정색 : 실제 data distribution
- 초록색 : Generator Distribution

- (a) -> (d) / (d) : global optimum

### proposition 1)
G가 고정되었을 때, optimal discriminator D는 아래와 같다.
![4](https://user-images.githubusercontent.com/61506233/88512539-afca7280-d021-11ea-88f7-96b0bc1624e0.jpg)


### proposition 2)
![5](https://user-images.githubusercontent.com/61506233/88512704-eef8c380-d021-11ea-90c7-3affd52e989f.jpg)

## 7. Conclustion

![3](https://user-images.githubusercontent.com/61506233/88512535-ae994580-d021-11ea-820d-f84fd084eed4.png)

