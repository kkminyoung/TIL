## 1. Feature Scaling
#### Normalization
1) Min-Max Normalization : 모든 feature 들의 스케일이 동일하지만, outlier를 잘 처리하지 못한다.
2) Standardiztion(표준화) : outlier를 잘 처리하지만, overfitting의 가능성

#### Feature Scaling의 효과
모든 feature의 영향력 고려, 수렴속도(계산비용)의 감소

## 2. Overfiiting VS Underfitting
#### Underfitting(high bias)
많은 공통특성 중 일부 특성만 반영하여, too bias 하게 train 되어 새로운 데이터도 막 예측해버리는 모델
- 해결방법 : feature를 더 많이 반영하여, variance를 높인다

#### Overfitting(high variance)
많은 공통특성 외에 지엽적인 특성까지 반영하여, high varianve 하게 train되어 새로운 데이터에 대해서는 예측하지 못하는 모델
- 해결 방법
1. Feature 수를 줄인다.
2. 데이터 크기 늘린다.
3. cross validation : 전체데이터셋을 k개의 subset으로 나누고 k 번의 평가를 실행
  장점) 오버피팅 방지 가능( 1. train에 들어가는 dataset이 계속 바뀜 2. validation set으로 계속 적합도 확인가능)
  단점) 시간이 오래걸린다.
4. early stopping, dropout 
  early stopping : 여러 layer로 이루어진 뉴런들을 epoch 수를 통해 반복학습하게 되면, 어느 시점에서 train set의 acuuracy 는 올라가나, validation set의 accuaracy는 멈추거나 낮아지는 지점이 오는데 accuracy가 더이상 올라가지 않을 때 stop하는 것
  dropout : train 과정에서 몇 개의 뉴런을 쉬도록하여 variance를 낮춘다. 몇번 쉬는 과정에서 overfitting을 막아줌
5. regularization 모델 복잡도 제한
n개의 parameter가 있을 때, 일부 parameter를 작은 값으로 만들어 식을 부드럽게 만들어준다. weight에 대한 제약조건을 추가하여 과최적화를 막는다.

## 3. Regularization
MSE와 penalty 항의 합을 최소로 만드는 값을 찾는 것
1. L1 Regularizion(Lasso) : 변수 간 상관관계 높으면 good / 크기가 큰 변수 먼저 줄이기 / 변수가 사라짐
2. L2 Regularization(Ridge) : 변수 간 상관관계 높으면 bad /  중요하지 않은 변수 먼저 없애기 / 계수는 줄어들지만 변수 선택이 어려움
3. Elastic net(Lasso + Ridge) : 변수 간 상관관계 반영 /변수도 줄이고, 분산도 줄이고 싶은 경우
