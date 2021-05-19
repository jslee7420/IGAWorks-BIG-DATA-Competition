# IGAWorks-BIG-DATA-Competition
IGAWorks에서 주관하는 빅데이터 공모전으로 광고 클릭률 예측(CTR prediction)을 주제로 진행하였습니다.
진행과정은 다음과 같습니다.

## 1. 변수 파악
데이터는 모두 암호화되어 제공되었습니다. 각 속성의 의미를 알기 위해 주최즉에 질문하고 검색하여 각 속성들의 정확한 의미를 파악하고자 했습니다.
![변수 분석](https://user-images.githubusercontent.com/46511190/118798338-7dec0a80-b8d8-11eb-9b0a-f7a8c9f3f4f2.jpg)


## 2. 탐색적 자료분석
데이터 타입체크, unique값 확인, null 값 체크, 종속변수 분포 체크를 수행하여 다음과 같은 결론을 얻었습니다.

1.대부분의 변수가 범주형 데이터이다.(age feature포함) ->encoding 필요
2.install_pack, cate_code는 데이터를 가공해서 새로운 정보를 만들어내야한다.(가공 방안을 생각해내지 못해 해
당열 제외 결정)
3.predicted_house_price에 존재하는 null 값을 채워주어야한다.
4.event_datetime은 unique값이 너무 크기 때문에 hour 정보만 추출하여야 한다.
5.unique값이 1인 device_os, device_country는 click에 영향을 미치지 않는다. 제외가능.
6.bid_id의 경우 unique값이 너무 크기 때문에 영향을 미치치 않을 것으로 보인다.

## 3. 데이터 전처리
### 3.1.predicted house price의 nan값 채우기
predicted_house_price의 결측값을 채우기 위해 같은 어디언스에 있는 정보인 age,gender,marry와의 상관관계를
파악하여 해당 정보를 바탕으로 결측값을 채우려고 했지만 age, gender, marry와 predicted_house_price 사이의 유의미한 상관관계가 거의 없어
predicted_house_priced의 평균 값으로 nan값 대체하였습니다.

### 3.2. 이외의 전처리
- 'event_datetime'에서 시간정보만 추출하였습니다.
- 성별, 결혼여부 숫자값으로 변환했습니다.

## 4.Feature Engineering
### 4.1.Dimension Reduction
다음과 같은 속성들을 삭제하였습니다.
1.asset_index: 처음부터 잘못 들어간 특성
2.install_pack, cate_code: 데이터 가공방안 찾지 못함
3.bid_id: unique 값이 너무 큼
4.device_os, device_country:unique 값이 하나로 클릭여부에 영향 없음

### 4.2.Target Encoding
범주형 데이터들에 대해 target encoding을 진행하였습니다.

### 4.3.outlier 제거
numerical variable인 event_datetime, predicted_house_price 에 대해 이상치 제거 작업을 수행

### 4.4.Scaling

## 5.학습
### 5.1.모델선정
보통 가장 좋은 성능을 내는 앙상블 모델 5개 와 선형 분류 모델까지 총 6종의 모델을 선정하여 학습을 진행하였습니다.
- Gradient
- Boosting
- AdaBoost
- XgBoost
- LightGBM
- Random Forest
- Logistic Regression
feature importance를 구하여 결과에 영향을 많이 미치는 feature들만 추려내서 feature selection을 진행하고자 했습니다.

## 6.결과
리더보드 스코어와 validation score를 종합적으로 비교했을때 RandomForestClassifier가 가장 좋은 성능을 내는
것으로 나와 최종적으로 RandomForestClassifier를 모델로 결정하였습니다.
![결과](https://user-images.githubusercontent.com/46511190/118797976-16ce5600-b8d8-11eb-8deb-d45c45e88042.png)



## 7.아쉬운 점
1. RandomForestClassifier를 제외한 나머지 머신러닝 모델의 경우 하이퍼 파라미터의 값이 성능에 영향을 많
이 미칩니다. 하지만 시간적, 자원적 제한으로 하이퍼 파라미터 튜닝과정을 거치지 못하였습니다. 각 모델마다 Grid
Search 를 통한 하이퍼 파라미터 튜닝과정을 거쳤다면 RandomForest가 아닌 다른 모델을 선정했을 가능성
이 있습니다.
2. Feature selection 과정에서 threshold 값 설정에 대한 논리적인 결정 과정이 없었습니다.
3. Outlier 제거, Scaling, feature seleting 과정을 동시에 진행하였는데 이 세과정을 거치지 않았을때의 모델의
정확도(리더보드 기준)가 오히려 더 좋았습니다.세 과정을 하나하나 거치며 어느부분에서 문제가 있었는지에 대
한 조사가 필요합니다.
4. neural network, knn, Naive Bayse 등등 더 많은 모델에 대한 시도가 부족했습니다.
5. audience profile에 포함된 instal_pack, Cate_code feature를 가공하여 새로운 의미있는 feature를 생성해 내
지 못했습니다.
6. Target encoding외에 다른 방법으로 categorical data를 encoding 하는 방법을 시도해볼 필요가 있습니다.
