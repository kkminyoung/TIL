# BEGAN
논문 링크 : https://arxiv.org/abs/1703.10717

Boundary Equilibrium Generative Adversarial Networks, David Berthelot et al. 2017

#### Contribution
- 단순하지만 강력한 구조
- 빠르고 안정적인 학습 / 수렴 가능
- 이미지의 다양성 & 퀄리티 사이의 trade-off를 조정하는 것이 가능
- 수렴의 approximate measure

#### 특징
- EBGAN, WGAN의 아이디어와 다른 기술을 이용해 좋은 결과를 만들어냈다.
- EBGAN과 마찬가지로 auto-encoder를 사용
- 일반적인 GAN은 data 분포를 맞추기위해 학습하는 반면 BEGAN은 auto-encoder loss distribution
을 맞추려 한다.


#### Wassertein distance 사용
- pixel-wise loss(error)들이 ~iid(indepented identically distributed) 라고 할 때, 
CLT에 의해 image-wise loss는 근사적으로 정규분포를 따른다.

![began1](https://user-images.githubusercontent.com/61506233/90364121-dcacfb00-e09e-11ea-8444-c5893104cffc.JPG)

#### Equilibrium
- G와 D사이에서 균형을 맞추는 것이 중요하다.(parameter중요)
- BEGAN에서는 이 부분도 equilibrium measure technique라는 개념을 도입하여 좀더 학습이 잘
된 경우에도 모델이 안정적으로 학습되게 해준다.

![equi 1](https://user-images.githubusercontent.com/61506233/90364117-db7bce00-e09e-11ea-8d4c-f84fa8d671b4.JPG)
![equi2](https://user-images.githubusercontent.com/61506233/90364118-dc146480-e09e-11ea-94bc-48a4c7ac56b9.JPG)

#### Convergence measure
- 최대한 real image에 가깝게 복원하도록
![convergence](https://user-images.githubusercontent.com/61506233/90364114-da4aa100-e09e-11ea-993a-4f63162066ad.JPG)

#### Training procedure
- 병렬로 동시 학습


#### Model architecture

![began2](https://user-images.githubusercontent.com/61506233/90364123-dd459180-e09e-11ea-91d1-ea652bb891af.JPG)



#### Result
- convergence measure and image quality
- equilibrium for unbalanced networks



![objective](https://user-images.githubusercontent.com/61506233/90364116-dae33780-e09e-11ea-9af3-517df5e3ee46.JPG)

![ob](https://user-images.githubusercontent.com/61506233/90364120-dcacfb00-e09e-11ea-89ba-02dd05a409cd.JPG)
