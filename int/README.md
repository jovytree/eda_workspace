# int's EDA_Workspace

## 1. 서론: MBA 데이터 소개 및 분석 방향

- **데이터 출처**: [MBA Admission dataset, Class 2025 (Kaggle)](https://www.kaggle.com/datasets/taweilo/mba-admission-dataset)

- **MBA 데이터 설명**: 2025년 와튼스쿨 MBA 지원자 정보와 합격 여부를 기록한 Synthetic 데이터로, Wharton Class of 2025 통계를 기반으로 생성됨.

- **분석 목표**: MBA 데이터에 대해 탐색적 자료 분석(EDA)을 수행하여
   - 지원자 정보에 따른 **MBA 합/불 여부 예측** (분류)
   - 합/불 여부에 영향을 미치는 **주요 특성 탐색**

## 2. 데이터 소개

- **메타 데이터**
   - `application_id`: 각 지원자의 고유 식별자
   - `gender`: 지원자의 성별 (남성, 여성)
   - `international`: 국제 학생 여부 (TRUE/FALSE)
   - `gpa`: 지원자의 학점 (4.0 만점)
   - `major`: 학부 전공 (상경계열, 이공계열, 인문계열)
   - `race`: 인종적 배경 (백인, 흑인, 아시아인, 히스패닉, 기타 / 국제학생인 경우 null)
   - `gmat`: GMAT 점수 (800점 만점)
   - `work_exp`: 업무 경력 (연수)
   - `work_industry`: 이전 근무 경력의 산업 분야 (컨설팅, 금융, 기술 등)
   - `admission`: 합격 여부 (합격, 대기, Null: 거절)

- **활용**
   - **탐색적 자료 분석(EDA)**: 데이터 분포, 관계, 패턴 파악
   - **분류(Classification)**: 다른 특성들을 바탕으로 합격 여부 예측

## 3. 자료 분석 및 기초통계

### 3-1. 업무 경험 연차 별 지원자 수
   - 평균 업무 연차는 약 5년이며, 4~6년 차 지원자가 다수  
   - **차트**: 업무 경험 연차별 지원자 수 히스토그램 첨부

### 3-2. 전공 / 산업 별 지원자 수
   - 전공별 지원자 수 확인: Humanities(2481명), Business(1838명), 자연과학 및 공학(STEM)(1875명)  
   - **차트**: 전공별 지원자 분포 그래프 첨부


### 3-3. 인종 및 국제학생 여부에 따른 지원자 수
   - 백인 지원자 가장 많음, 국내 학생이 국제 학생보다 약 2.36배 많음  
   - **차트**: 인종 및 국제학생 분포 그래프 첨부

   ![](https://github.com/srogsrogi/eda_workspace/blob/master/int/image/descriptive_statistics.png?raw=true)
   ![](https://github.com/srogsrogi/eda_workspace/blob/master/int/image/Count_international.png?raw=true)

### 3-4. GPA, GMAT 점수 - 합격 여부
   - GPA와 GMAT 점수 분포를 통한 합격 여부 분석  
   - **차트**: GPA 및 GMAT별 합격 여부 Box Plot, Histogram 첨부
   ![](https://github.com/srogsrogi/eda_workspace/blob/master/int/image/gmat_gpa_boxplot.png?raw=true)
   ![](https://github.com/srogsrogi/eda_workspace/blob/master/int/image/Hist_GPA&GMAT.png?raw=true)

### 3-5. 연관성 분석
   - **히트맵**: 변수 간 상관 계수를 시각화하여, 합격 여부에 대한 주요 특성을 탐색. 상관성이 낮아 여러 특성의 조합이 중요한 것으로 해석 가능
   ![](https://github.com/srogsrogi/eda_workspace/blob/master/int/image/heatmap_numerical.png?raw=true)

## 4. 데이터 전처리

1. **불필요한 column 삭제**: `application_id`, `international`

   ![](https://github.com/srogsrogi/eda_workspace/blob/master/int/image/df_race_international.png?raw=true)

3. **결측치 처리**: `admission`의 NaN을 decline(거절)로 대체

4. **이상치 탐색**: `admission`의 Waitlist를 거절로 간주

![](https://github.com/srogsrogi/eda_workspace/blob/master/int/image/df_admission.png?raw=true)

4. **범주형 특성 인코딩**
   - **Label Encoding**: gender
   - **One-hot Encoding**: major, race, work_industry
   
5. **변수 구간화 (범주화)**: work_exp → 3개의 구간으로 분할 (1~3, 4~6, 7~9)


## 5. 훈련

1. **스케일링**
   - StandardScaler로 특성값을 표준화하여 모델 성능 최적화

2. **교차검증**
   - cross_val_score를 통해 모델 신뢰도를 높이고 과적합 방지

3. **특성공학**
   - PolynomialFeatures로 2차원 특성을 생성하여 다차원 데이터를 학습 가능

4. **훈련모델 선택**
   - DecisionTree를 사용하여 빠른 훈련과 높은 정확도 확보

5. **하이퍼파라미터 튜닝**
   - GridSearchCV를 통해 DecisionTree의 `max_depth`, `min_sample_split`, `min_sample_leaf`를 최적화

## 6. 결론

### 6-1. **분석 결과 요약**
   - **핵심 결과**
      - 테스트 데이터에 대해 약 85%의 정확성으로 분류 성공
      - 단일 특성보다는 여러 특성의 조합이 MBA 합격 여부에 중요한 영향을 미침
    ![](https://github.com/srogsrogi/eda_workspace/blob/master/int/image/results.png?raw=true)

   - **주요 성과**: GPA와 GMAT의 상관 계수는 0.58로 다소 낮아 합격 여부에 독립적인 특성으로 작용할 가능성 있음

### 6-2. **기존 분석 대비 새로운 해석 및 의미**
   - 중요 특성으로 예상되었던 GMAT과 GPA의 절대적 영향력이 크지 않음을 확인
   - **의미**: 지원자의 다양한 특성 조합이 합격 여부에 복합적인 영향을 미침을 시사

### 6-3. **분석에서의 제약사항**
   - Waitlist를 거절로 간주한 점이 분석 결과에 영향을 줄 가능성이 있음
   - 국제 학생 여부로 인종 변수가 null 처리된 점은 분석에 있어 제약이 될 수 있음

### 6-4. **추가 분석의 필요성 및 의미**
   - 다양한 특성의 비선형적 상호작용을 반영할 수 있는 머신러닝 모델 적용 필요성 제안
   - 보다 정교한 특성 조합을 활용한 추가 분석이 합격 예측 모델의 성능을 개선할 수 있을 것으로 기대됨

---