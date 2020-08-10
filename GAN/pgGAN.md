# pgGAN(Progressive Growing Of GANs for improved qaulity, stability, and variation)

### Problem
- 학습 분포와 생성 분포간에 substantial overlap이 없다면, gradient가 랜덤한 방향을 가리킬 수 있다.
특히 고화질 이미지를 생성하는 경우, 이런 문제가 더 심화되어 학습이 불안정해진다.
- 다양한 variation을 만들지 못한다. 

### Solution
- 이런 문제를 해결하기 위해 학습과정에서 저화질 이미지에 대한 generator와 discriminator로부터 시작하여 layer를 키워나가면 
안정성과 고화질 모두를 얻을 수 있을 것으로 기대하며 시작한다.
- mode collapse를 방지하고 variation을 키우는 방법을 사용

### Progressive growing of GANs
- Generator와 discriminator의 layer를 대칭적으로 하나씩 쌓아가며 학습
- 이는 large-scale structure를 먼저 파악한 뒤에 finer scale detail로 점점 옮겨가는 것
- 모든 scale을 동시에 학습하는 것이 아님
- Layer를 늘리는 시점에 갑작스러운 충격을 막기위해 Highway network의 구조 사용

### Increasing variation using minibatch standard deviation
- GAN은 training data의 variation에 대한 subset만을 학습하는 경향이 있다. -> Mode collapse
- 이를 해결 하기위해  minibatch discriminator 라는 기법을 통해 
각 이미지와 minibatch에 대한 통계정보를 discriminator에 같이 제공함으로써 variation에 대한 학습효과를 증진시키려는 시도를 했다. 
이 연구에서는 learnable parameter나 새로운 hyperparameter없이 비슷한 접근을 시도해본다.

1. 전체 minibatch에 대해, 각 feature의 각 spatial location의 표준편차를 구한다. (Input: N x Cx H x W, Output: C x H x W)
2. 앞서 계산된 값으로 각 spatial location에서 모든 feature에 대한 평균을 구한다. (Input: C x H x W, Output: 1 x H x W)
3. 이를 discriminator의 feature map 중 어딘가에 넣어준다. 
4. 마지막에 넣는 것이 가장 효과가 좋았다. 


### Normalization in generator and discriminator
- Equalized learning rate
- Pixelwise feature vector normalization in generator

### Multi-scale statistical similarity for assessing GAN results
전제 : 잘 학습된 generator가 생성한 이미지는 모든 스케일에서 training set과 유사한
local image structure를 가짐
GAN 성능 평가 방법 : Training set과 generated samples에서 Laplacian pyramide를 통해 local image patch 후 sliced Wasserstein distance(SWD)


결과
- MS-SSIM보다 SSD가 색, variation을 훨씬 잘 잡아냄. 
- 학습이 중간에 중단된 모델과 학습을 온전히 끝낸 모델간의 결과물을 비교했을때 MS-SSIM는 큰 차이 X 
- SWD는 training set과 유사한 생성 이미지의 분포에 대해 잘 찾아냄
- progressive growing은 better optimum에 수렴시키고 training time을 줄이는 효과
- non-progressive variant에 비해 빠른 속도
- 고화질 결과를 보이기 위해 AppendixC의 방법을 사용해 CELEBA의 high-quality 버전을 만들어냄

