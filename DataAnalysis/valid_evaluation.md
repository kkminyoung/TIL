# valid_evaluation

### Cross Validation
1) The holdout ->문제 : 한쪽 dataset에 특이한 데이터가 있을 수 있음
2) K-fold validation -> 문제 : 나눈 dataset들이 class 별로 나뉘어져 있을 수 있음
3) Stratified K-fold 
  -> Class 들이 비중에 맞게 할당!
  -> 시계열에서 많이 사용
           
- 사이킷런 kfold -> black box
- hold out -> white box 

### Confusion matrix
recall : 실제로 일어났는데, 그걸 모델이 일어났다고 예측할 확률
precision : 모델이 일어났다고 예측을 했는데 실제로 일어날 확률
F1-score : recall과 precision의 조화평균(trade-off관계)

Accuaracy : precision/recall 중요도 반영할 수 x, imbalanced 한 경우
