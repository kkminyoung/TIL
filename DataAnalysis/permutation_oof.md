## Permutation
t-test 등의 일반적인 통계 검정을 수행할 만큼 샘플의 수가 크지 않은 경우에 사용할 수 있는 검정 방법. 이 경우 주어진 샘플을 무작위로 추출하여 인공적으로 샘플 숫자를 늘림으로써 전체 모수를 통계 검정이 가능한 크기만큼 키운 다음, 원래 주어진 샘플의 통계 값(ex. 평균, 분산 등)이 전체 모수와 비교하여 얼마나 유의하게 차이 나는지를 검정하는 방법이다.

#### 과정)
1. 생성된 모든 feature에 대해 train
2. 각 column을 one by one 으로 permutate 하고 hold-out validation의 변화를 살펴봄
3. 
1) 만약 훨씬 더 나빠지고 있다면, 이 column은 target에 대해 유용한 정보를 포함하고 있는 것임 
2) 변화가 없거나 좋아지지 않는다면 이 feature는 useless(bad_features)
4. column을 normal state로 return & 다음 column으로 이동

#### get_score_importance(score_func, X, y, n_iter,columns_to_suffle,random_state)
  parameters
> - score_func: your function with model inference and scoring.# MAE - 위에서 정의한 score 함수
> - X: features
> - y: target
> - n_iter=5: how many times columns will be permuted.
> - columns_to_shuffle=None: subset of columns to shuffle. If None, then all columns will be checked.
> - random_state=None

 return
> - base_score: score on original features
> - score_decreases: list of length n_iter with feature importance arrays

## oof (out of fold)
K-Fold cross validation에서 매 fold마다 90%의 데이터로 훈련을 하고, 나머지 10%의 데이터에 대해 예측. 이 10%로 error matric(ex RMSE) 계산. 이 과정을 통해 결국 남는 것은, 10차례의 각 Fold마다 RMSE 값 하나와 각 10% 데이터셋에 대한 예측치. 이 결과물을 2가지 방법으로 처리할 수 있다.

1. 10개의 RMSE 값의 평균과 표준편자를 확인한다. K-Fold는 랜덤으로 데이터를 나누므로, 각 Fold에서 나온 에러값(RMSE)들은 서로 비슷해야 한다. 만약, 값들이 서로 비슷하지 않다면, 이 때의 모델(피쳐와 하이퍼패러미터들)로는 테스트셋에서 안정적인 예측치를 얻기 어려울 것이다.
2. 10세트의 예측을 합쳐서 하나의 예측을 만든다. 예를들어, train data가 1000개였다면, 100개의 예측치로 이루어진 10개의 test set을 얻는다. (10x100 = 1000 이므로) 이 10 set를 하나의 벡터로 만들어서, 1000개의 예측치를 갖는 하나의 전체 예측세트를 얻는다. 이 전체 test set을 out-of-folds 예측이라고 한다. 이 전체 test 데이터에 대한 예측을 이용해 RMSE 를 계산할 수 있습니다. 즉, rmse = compute_rmse(oof_predictions, y_train) . 최종적으로 예측모델을 평가하는 깔끔한 방법 중의 하나
