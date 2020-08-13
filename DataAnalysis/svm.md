## Support Vector Machine

#### 목표
- Two classification 문제
- 두 class를 나누는 hyperplane 많음
- 경계면 정하는 기준 : 샘플 사이의 거리를 가장 넓게!!
> Margin이 최소가 되는 hyperplane을 찾는 것
> (Margin = 각 클래스에서 가장 가까운 관측치 사이의 거리 2/||W|| ))

### 수식이해
- Decision Rule : y_i(w * u + b ) -1 >=0
- 목적식 : min(1/2 * ||w||^2)
- 제약식 : y_i(w*u + b) >=1
- 수식 : pdf 참고
- SVM 의 해 : w, b  w = sum(a_i*y_i*x_i)
> a_i가 0이 아니라는 것은 x_i가 경계를 정하는 샘플 이라는 뜻 -> Support Vector

### 선형분류가 어려운 데이터의 경우
#### C-SVM
- Margin안에 관측치의 존재 허용(Soft-Margin)
- 엡실론만큼 패널티 부과
- C의 크기로 마진 폭 조절

#### Kernel SVM
- 샘플(input space) 들을 먼저 선형 분류가 가능한 고차원 공간(feature space)로 매핑하는 것
- 매핑 후 SVM 모델 적용(커널트릭 적용)
- 커널의 종류 : linear, polynomial, sigmoid, gaussian 등















