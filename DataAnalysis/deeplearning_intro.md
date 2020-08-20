# DeepLearning Intro

#### 딥러닝이란?
- 여러 층(layer)을 가진 인공 신경망 기술
- 대표적인 모델 : CNN, RNN
- 머신러닝에 속하지만 최근 딥러닝 모델이 발전하며 구분되는 경향

#### 퍼셉트론
- Perceptron = perception(인지능력)+neuron(신경세포)
- 선형분류 모델
- And나 Or와 같은 논리회로 연산을 통해 분류
- 한계 : XOR 논리회로는 선형모델로 분류 불가능
- 한계극복 : 다층 퍼셉트론 - 퍼셉트론 여러 개를 이어 붙여 층을 쌓는다. 
(선형 분류만으로 풀지 못했던 문제를 비선형적으로 해결)

## 신경망
### 구조
- 입력층(Input)
- 은닉층(Hidden Layer)
- 출력층(Output)

### 학습방법
- Forward propagation : input에서 output 방향으로 결과 값을 내보내는 과정
- Back propagation : 결과 값을 통해 역으로 Input 방향으로 오차를 다시 보내며 가중치 업데이트
(epoch 을 늘릴 수록 가중치값을 업데이트 하며 loss값을 줄여나감)
(Chain Rule, example pdf 참고)
-

### 활성화 함수
- 노드에 들어오는 값들에 대해 다음 layer로 전달하기 전 비선형 함수 사용
- 사용 이유 : 선형 함수를 사용할 경우, 층을 깊게 하는 의미가 줄어든다
- 활성화 함수 종류 : sigmoid, tanh, Maxout, ReLU, Leaky ReLu, ELU..

#### sigmoid 함수
- 결과 값이 (0,1)사이로 출력
- 극단의 값은 대부분 0 또는 1
- 신경망 초기에 사용

#### sigmoid 함수 단점
- Gradient Vanishing 발생
- 함수 값 중심이 0이 아님 -> zig zag 현상
- Exp 사용으로 비용 많이 발생
- 지그 재그 현상 ? 함수값이 모두 양수 -> Loss가 가장 낮은 지점을 찾아 가는데 정확한 방향으로 
가지 못하고 지그재그로 수렴

#### tanh 함수
- sigmoid와 유사
- 결과 값이 (-1, 1) 사이로 출력
- 지그재그 현상이 덜하다.


#### tanh 함수 단점
- Gradient Vanishing 발생

#### ReLU 함수
- 현재 가장 많이 사용되는 함수
- 학습이 빠름
- 연산 비용이 크지않고 구현이 간단

#### ReLU 함수 단점
- 지그재그 현상 발생
- x<0 값들로 뉴런이 죽을 수도 있음

#### LeakyReLU
ReLU 음수부분 개선
#### ELU
ReLU 특성 공유, gradient vanishing 극복
#### Maxout
연결된 두 뉴런 값 중 큰 값 이용, 연산량이 많이 필요

### 출력 부분 함수
1. 회귀 : 출력 값을 그대로 반환하는 함수 = 항등함수
2. 분류(Binary) : sigmoid 함수
3. 분류(Multiple) : soft max 함수
#### softmax 함수
- 결과값이 (0,1)사이로 출력
- 총합은 항상 1
- 분류하고 싶은 클래스의 수 만큼 출력 구성

#### 가중치 초기값 설정
- 표준편차가 1일 경우, 값이 0과 1에 분포 -> Gradient Vanishing 문제
- 표준편차가 0.01일 경우, 값이 0.5에 치우쳐져 모두 같은 값 출력
- Xavier Initializer : 표준 정규분포를 입력 개수의 표준편차로 나누기, sigmoid 또는 tanh함수 사용시 주로 사용
- ReLU 함수 + Xavier Initialization -> 점차 0으로 수렴


### Loss function
- 신경망이 학습할 수 있도록 해주는 지표
- 모델의 출력 값과 사용자가 원하는 출력 값의 차이, 즉 오차를 의미
목적
- 손실함수 값을 가능한 작게 하는 매개변수 값을 찾는다.
- 정확도는 매개변수 변화에 둔감하고 변화가 있더라도 불연속적으로 변화하기 때문에 미분 불가능해 손실함수 사용
- ex) Mean Square Error, Cross Entropy Error

### 최적화(Optimizer)
- 손실함수의 값을 최저점 찾기
- 학습속도 빠르고 안정적인 방향 찾기
1. Gradient 수정 : Momentum, NAG
2. Learning Rate 수정 : Adagrad, RMSProp, AdaDelta
3. 1,2번 장점을 합침 : 

#### Gradient Descent
- 오차함수의 낮은 지점을 찾아가는 최적화 방법
- 낮은 쪽의 방향을 찾기 위해 오차함수를 현재 위치에서 미분







