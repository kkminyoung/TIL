## 부스팅(Boosting)

- 여러 개의 약한 학습기(weak learner)를 순차적으로 학습, 예측
- 잘못 예측한 데이터에 가중치(weight) 부여
- 계속해서 분류기에게 가중치를 끌어올려주면서 학습 진행
- Weak learner의 결합으로 최종 예측


### Adaboost
- Adaptive + boosting
- 간단한 weak classifier들이 상호보완하도록 단계적으로 학습
- 이들을 조합하여 최종 strong classifier의 성능 증폭
- 이전 분류기가 오분류한 샘플의 가중치를 바꾸어가며 잘못 분류되는 데이터에 더 집중하여 학습, 분류가능
- 약한 분류기로 *Stump사용(*Stump : 바로 leaf node 만을 가지는 tree, 한번의 test만 가능)
- stump들을 순차적으로 사용
- 처음에는 gini 계수가 가장 낮은 feature의 stump로 start
- 알고리즘
![adaboost](https://user-images.githubusercontent.com/61506233/90122580-74a59e80-dd98-11ea-9236-e580669baa9d.png)




### GBM (Gradient Boosting Machine)
- residual 을 줄이는 방향으로 weak learner들을 결합
- weak learner가 tree
- 과정
1. 초기에는 평균값으로 모든 예측값을 예측
2. 실제값과 예측값의 차이(잔차) 구하기
3. 잔차를 예측하는 Tree를 만든다.
4. 기존 예측값+(잔차*learning rate)로 새롭게 예측값 업데이트
5. 위 step과정을 반복

![gbm](https://user-images.githubusercontent.com/61506233/90123731-44f79600-dd9a-11ea-96a9-dfe3be2f03e9.png){: width="50%" height="50%"}


- 문제점 : 트리모델 base->분기를 효율적으로 해야함 -> 너무 큰 데이터.. / 과적합




### Xgboost
- GBM에 Regularization을 추가한 모델 -> 과적합 방지
- 다양한 loss function 제공
- 별령처리를 통한 빠른 연산(노드단위)
- 알고리즘
![xgb](https://user-images.githubusercontent.com/61506233/90123749-4aed7700-dd9a-11ea-981e-59818e6b01d7.JPG)

xgb 논문 : https://arxiv.org/abs/1603.02754




### Lightboost
- XGBoost 대비 성능향상 및 자원 소모 최소화
- Level wise(균형 트리) 대신 leaf wise 사용
- XGBoost와 달리, leaf node 가 분할되면서 깊이가 깊어지므로 하이퍼파라미터 튜닝 필수

![lgbm](https://user-images.githubusercontent.com/61506233/90123743-488b1d00-dd9a-11ea-9038-a4075e1b0ace.png)

논문링크 : https://papers.nips.cc/paper/6907-lightgbm-a-highly-efficient-gradient-boosting-decision-tree.pdf

### 
