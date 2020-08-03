(참고용-메모)

### Pix2Pix

- Image to Image mapping network에서 Photo-realistic을 추구
- GAN의 Adversarial Traning 도입
- Training data가 쌍으로 존재해야함! (이후 cycleGAN 등 쌍이 필요 없는 모델이 나옴)

- Generator Networt에 U-Net을 사용
UNet - Encoder Decoder 구조에서, 정보의 손실이 발생하기때문에 Skip-connection을 이용해서 연결해준 구조
- Discriminator에 PatchGAN 사용
진짜인지 가짜인지 판별하는 과정에서 Image의 Overlap 되는 Patch 단위로 하는 것
- GAN Loss + L1 


### DiscoGAN

- unpaired dataset을 사용해야 한다는 점에서 cycleGAN과 완전히 동일
- Generator 학습시,  (GAN Loss + Feature Loss)
- ImprovedGAN에서의 방법을 어느정도 반영
- Encoder - Decoder 구조 (학습이 빠르다)
- 형태 변화가 자유롭지만 해상도는 낮음
