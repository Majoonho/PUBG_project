![header](https://capsule-render.vercel.app/api?type=waving&color=auto&height=200&text=Welcome!%20&fontSize=60&fontAlignY=40&desc=I'm%20joonho)


# PUBG_project

##### Task 목적 : 회귀모델을 활용해 우승자를 예상해야 함 <br/> EDA -> Preprocessing -> Feature Engineering -> 학습 -> Hyper-parameter Tuning -> Submission <br/>My part : 모든 과정 참여 <br/>Team : 5명

#### 개인 목적 : VIF 와 Feature Importance 에 대한 이해

## EDA
  * 결측치 처리 
  * Feature 별 분석
  * corr() 를 통한 sns.heatmap 으로 전체적인 상관관계 파악 <br/>--> 이때 target과의 상관관계를 살펴보아야함
    * walkDistance > boosts > weaponsAcquired > damageDealt > heals > kills > longestKill <br/>-->label(target)인 winPlacePerc와 상관관계가 높음(이들 feature들을 분석)
    * 상관관계 분석, VIF 분석, RF의 feature_importance, LGBM의 feature_importance, PCA등 <br/>다양한 feature 분석 툴들을 통해 공통의 feature를 추출하는 것을 고려해봄
  * VIF ( Variance Inflation Factor : 다중공선성 분석)
    * 다중공선성이란 ? 독립 변수 X는 종속 변수 Y(target) 하고만 상관 관계가 있어야 하는데,<br/> 독립 변수끼리 상관 관계를 보이는 것
    * ```from statsmodels.stats.outliers_influence import ariance_inflation_factor```
    * 보통 VIF 점수가 10점 이상이면, 상관관계가 있다고 해석할 수 있음
    <br/>
 
<img src="https://user-images.githubusercontent.com/103080228/201831212-92e6c7a1-986c-4ccc-a11e-7acf9fe5cf94.png"  width="700" height="350">
<br/>



모형이 데이터에 잘 맞는 정도를 보여주는 지표중 하나이다. R 제곱이라는 뜻으로 위의 데이터에는 0.603으로 표현되는 것을 알 수 있다. 쉽게 말하면 얼마나 선형적인가(a = B0 + B1 * weight)를 표현한 수치라고 할 수 있다. 범위는 0에서 1 사이의 값으로 0이면 모델의 설명력이 전혀 없는 상태이고, 1에 가까울수록 모델이 데이터를 잘 성명해 주는 상태라고 할 수 있다. 보통은 0.4 이상이면 괜찮은 모델이라 할 수 있다.

 

 

coef(coefficient, 계수)

데이터로부터 얻은 계수의 추정치이다. 출력 값에서 coef 부분을 보면 intercept가 -13.3957이고 b가 0.4749라는 것을 알 수 있다. 이것을 선형 회귀분석 모델 식에 넣어보면 'a = -13.3957 + 0.4749 * b'라는 식이 만들어진 거라고 생각하면 된다. 이는 b가 1 상승할 때마다 a가 0.4749 증가한다고 해석 할 수 있다.

 

 

P>|t| (유의 확률)

독립변수의 유의 확률로 보통은 독립변수가 95%의 신뢰도를 가져야 유의미하다 판단하고 유의 확률은 0.05보다 작은 값이 산출된다. 즉, 0.05보다 작으면 독립 변수가 종속 변수에 영향을 미치는 것이 유의미하다고 보면 된다.

p-value(유의 확률, significance probability)는 '귀무가설(Null hypothesis)이 맞는다고 가정할 때 얻은 결과보다 극단적인 결과(관측 결과)가 나타날 확률'로 정의됩니다. 일반적으로 p-value < 0.05 혹은 0.01을 기준으로 합니다. 계산된 p-value가 기준값보다 작은 경우 귀무가설을 기각하는 것으로 즉, 극단적으로 귀무가설이 일어날 확률이 매우 낮은 상태를 의미합니다.
 

