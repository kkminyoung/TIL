
### 스키마
-  데이터베이스의 구조와 제약조건에 관한 전반적인 명세를 기술한 것
-  외부스키마, 개념스키마, 내부스키마

### 고려사항
- 무결성 : 연산 후에도 제약조건을 만족해야함
- 일관성
- 회복
- 보안
- 효율성
- 확장


### 설계순서
- 개념->논리->물리

### 데이터모델에 표시할 요소
- 구조, 연산, 제약조건

### 데이터모델 구서요소
- 개체, 속성, 관계

### 식별자 분류
대표성
- 주식별자
- 보조식별자

스스로 생성
- 내부식별자
- 외부식별자

단일속성여부
- 단일식별자
- 복합식별자

대체여부
- 원조식별자
- 대리식별자

### 관계형 데이터베이스
- 2차원적인 표를 이용하여 데이터 상호관계 정의
- 다른 db로 변환 용이
- 튜플 : 릴레이셔늘 구성하는 각각의 행, 튜플수 - 차수
- 속성 : db를 구성하는 가장 작은 논리적 단위
- 도메인 : 하나의 애트리뷰트가 취할 수 있는 같은 타입의 원자값의 집함


### 키
후보키(유일성 최소성) 기본키 대체키(기본키 제외 남은 후보키) 슈퍼키(유일성o 최소성x) 외래키


### 무결성
데이터베이스에 저장된 데이터값과 그것이 표현하는 현실세계의 실제값이 일치하는 정확성
- 개체무결성 : 기본키- NULL, 중복값X
- 참조무결성 : 외래키값은 NULL이거나 참조 릴레이션의 기본키 값과 동일해야한다.
- 도메인무결성
- 사용자정의무결성
- NULL 무결성
- 고유무결서
- 키무결성
- 관계무결성



### 이상
테이블에서 일부 속성들의 종속으로 인해 데이터 중복이 발생하고 이로인해 테이블 조작시 문제가 발생하는 현상
- 삽입 이상
- 갱신 이상
- 삭제이상


### 함수적종속
결정자, 종속자


### 반정규화
정규화된 데이터모델을 의도적으로 통합 중복 관리하여 정규화 원칙을 위배 하는 행위

- 테이블 통합
- 테이블 분할 :수평분할, 수직분할
- 중복 테이블 추가 : 집계테이블 추가, 진행테이블 추가, 특정 부분만을 포함하는 테이블 추가
- 중복 속성 추가


### 테이블 종류
- 일반테이블
- Clustered index 테이블 : 기본키나 인덱스키의 순서에 따라 저장, 일반적인 인덱스를 사용하는 테이블에 비해 접근 경로가 단축됨
- 파티셔닝 테이블 : 대용량의 테이블을 작은 논리적 단위인 파티션으로 나눈 테이블
- 외부 테이블
- 임시 테이블 

### 트랜잭션
데이터베이스의 상태를 변환시키는 하나의 농리적 기능을 수행하기위한 작업의 단위 또는 한꺼번에 모두 수행되어야할 일련의 연산
- 원자성(commit, rollback), 일관성, (독립성, 격리성, 순차성), (영속성, 지속성)


### 인덱스
- 데이터 레코드를 빠르게 접근하기 위해 <키값, 포인터> 쌍으로 구성되는 데이터구조
- 삽입 삭제 잦으면 별로
- 트리기반 인덱스, 비트맵 인덱스, 함수기반 인덱스, 비트맵 조인 인덱스, 도메인 인덱스
- 클러스터드 인덱스(순서에따라 정렬), 넌클러스터드인덱스(키값만 정렬)

### 뷰
하나이상의 기본테이블로부터 유도, 허용된 자료만 제한적 보여줌
- CREATE, DROP
- 인덱스 가질 수 없음, 변경 불가능

### 클러스터
- 데이터 저장시 액세스 효율을 향상 시키기위해 동일한 성격의 데이터를 동일한 데이터 블록에 저장하는 물리적 저장 방법
 - 조회속도 향상, 입수삭 저하
 - 분포가 넓을수록 좋지

### 파티션
대용량 테이블이나 인덱스를 작은 논리적 단위인 파티션으로 나눔
- 데이터 처리는 테이블 단위, 데이터 저장은 파티션별
- 액세스 범위를 줄여줘 쿼리 성능 향상
- 디스크 성능 향상
- 조인 비용 즈가
- 용량이 작은 테이블에 파티셔닝을 수행하면 오히려 성능 저하

#### 파티션 종류
- 범위분할, 해시분할, 조합분할

### 분산 데이터베이스
- 논리적으로는 하나의 시스템에 속하지만 물리적으로는 네트워크를 통해 연결된 여러개의 사이트에 분산된 데이터베이스
- 위치 투명성 : 액세스 하려는 db의 실제 위치를 알 필요 없이 단지 데이터베이스의 논리적인 명칭만으로 액세스
- 중복 투명성 : 동일 데이터가 여러 곳에 중복되어있더라도 사용자는 마치 하나의 데이터만 존재하는 것처럼 사용하고 시스템은 자동으로 여러 자료에 대한 작업 수행
- 병행 투명성 
- 장애투명성


#### 분산 설계 방법
- 테이블 위치 분산
- 분할
- 할당

### 데이터베이스 이중화
- 동일 데이터베이스를 복제하여 관리하는 것
- Eager 기법, Lazy 기법

### 클러스터링
- 고가용성 클러스터링 : 하나의 서버에 장애가 발생하면 다른 노드가 반아처리
- 병렬처리 클러스터링 : 전체 처리율을 높이기 위해 하나의 작업을 여러개의 서버에서 분산하여 처리하는 방식

### RTO/RPO
- RTO : Recovery time objective : 목표 복구시간
- RPO : Recovery point objective : 목표 복수 시점


### 접근통제
데이터가 저장된 객체와, 사용하려는 주체사이 제한
- 접근 통제 정책,접근통제 매커니즘, 접근통제 보안모델
- 기술
   - DAC : 임의 접근 통제, 데이터소유자
   - MAC : 강제 접근 통제, 시스템
   - RBAC : 역할기반 접근통제, 중앙관리자


### 스토리지
- 단일 디스크로 처리할 수 없는 대용량 데이터를 저장하기 위해 서버와 저장장치를 연결하는 기술

#### DAS
- Direct Attached Storage
- 서버와 저장장치를 전용 케이블로 직접 연결하는 방식

### NAS
- 서버와 저장장치를 네트워크를 통해 연결하는 방식

### SAN
- 서버와 저장장치를 연결하는 전용 네트워크를 별도로 구성하는 방식
