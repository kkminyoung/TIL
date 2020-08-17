(참고용-메모)

## Pix2Pix

- Image to Image mapping network에서 Photo-realistic을 추구
- GAN의 Adversarial Traning 도입
- Training data가 쌍으로 존재해야함! (이후 cycleGAN 등 쌍이 필요 없는 모델이 나옴)

- Generator Networt에 U-Net을 사용
UNet - Encoder Decoder 구조에서, 정보의 손실이 발생하기때문에 Skip-connection을 이용해서 연결해준 구조
- Discriminator에 PatchGAN 사용
진짜인지 가짜인지 판별하는 과정에서 Image의 Overlap 되는 Patch 단위로 하는 것
- loss를 MSE로 하는데 이렇게 하면 문제가 loss에서의 합계(평균)이다. 
- 각 pixel별로 가장 완벽한 답을 찾으려고 하기보다는, 전체 pixel의 관점에서의 Loss를 줄이려고 하기 때문에, 픽셀 값이 정확한 값을 추정하기보다는 안전한 값으로 대충 얼버무리고 넘어간다는 것.

찾아내는 확률적 접근성이 짙은 방법이기에 원론적으로는 더 정확한 접근이라고 볼 수 있으나 image에 적용했을 경우 loss function이 생긴것처럼 Photo-realistic과는 거리가 멀다.
- 하지만 GAN은 D를 통해서 학습이 되는 것에 초점을 맞추었고 VAE에 비해서 확실한 선택을 하기에 Photo-realistic하게 매우 잘 나온다는 특징이 있다.
- 기존의 GAN은 noise distribution으로부터 Data Distribution을 뽑아내는 학습을 한다면 Cycle GAN은 한 image Domain으로부터 또 다른 Image Domain로부터의 Mapping function을 학습하게 되고 여기서부터 D를 통해 진짜인지 아닌지를 검사하는 것.

- 즉, 엄밀히 말하면 G는 생성(Generative)하는 것보다는 Transfer에 가까운 것.

- 마지막 term은 Reconstruction Loss로서, L1 norm.
 >> y와 x로부터 만들어진 G(x)의 차이를 Minimize

- GAN과의 차이는 D에 들어갈 때 Pair로 들어간다는 점이 특징.


### 요약

- Image to Image Mapping Network에서 Photo-realistic을 추구하고 싶음.
- 그래서 GAN의 Adversarial Training을 도입.
- U-Net과 PatchGAN을 통해서 성능 향상
- Training Data가 Pair로 존재해야 함.


## DiscoGAN

- unpaired dataset을 사용해야 한다는 점에서 cycleGAN과 완전히 동일
- Generator 학습시,  (GAN Loss + Feature Loss)
- ImprovedGAN에서의 방법을 어느정도 반영
- Encoder - Decoder 구조 (학습이 빠르다)
- 형태 변화가 자유롭지만 해상도는 낮음

- 각각 Domain에 해당하는 D가 존재하고 맨 위와 아래에 Cycle Loss에 해당하는 부분 존재.
- CycleGAN과 Data 구성에서부터 차이가 있음을 보여줌.  이거는 형태에 변화가 큰 dataset에 주로 실험을 함.

Generator

- Resnet도 아니고 U-net도 아닌 encoder-decoder구조를 사용해서 데이터의 핵심만 가져오는 형태.
- 이 구조의 특징은 정보의 손실이 발생하여 학습이 살짝 어려울 수 있고, 높은 해상도를 기대하긴 힘들 수 있어도 역설적으로 형태에 변환에 있어서는 자유롭다.

Discriminator

- DCGAN의 구조 그대로 가져옴. 해상도 높이는 것에 크게 관심은 없음.
- 모든 image는 64x64의 해상도
- Formulation은 동일한데, LSGAN을 사용하지 않고 Cycle Loss에서 L1대신 L2 Loss를 사용

