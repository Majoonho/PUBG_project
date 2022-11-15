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
 
<img src="https://user-images.githubusercontent.com/103080228/201831212-92e6c7a1-986c-4ccc-a11e-7acf9fe5cf94.png"  width="450" height="300"> <img src="https://user-images.githubusercontent.com/103080228/201832677-1e32944f-8500-46cd-86d0-789ea2a4d0d5.png"  width="300" height="300">
<br/>


---- R-squared

##### 모형이 데이터에 잘 맞는 정도를 보여주는 지표.<br/><br/> R 제곱이라는 뜻으로 위의 데이터에는 0.832으로 표현되는 것을 알 수 있습니다. <br/>쉽게 말하면 얼마나 선형적인가(y=w*X+b)를 표현한 수치라고 할 수 있습니다. <br/>1에 가까울수록 모델이 데이터를 잘 성명해 주는 상태이며, 0.4 이상이면 괜찮은 모델이라 할 수 있습니다.


---- coef(coefficient, 계수)

##### 데이터로부터 얻은 계수의 추정치. <br/><br/> 쉽게 생각해서 w, 기울기라고 생각할 수 있습니다.<br/>

---- P>|t| (유의 확률)

##### 독립변수의 유의 확률로 보통은 독립변수가 95%의 신뢰도를 가져야 유의미하다 판단하고 유의 확률은 0.05보다 작은 값이 산출된다. <br/>즉, 0.05보다 작으면 독립 변수가 종속 변수에 영향을 미치는 것이 유의미하다고 보면 된다.<br/>p-value(유의 확률, significance probability)는 '귀무가설(Null hypothesis)이 맞는다고 가정할 때 <br/>얻은 결과보다 극단적인 결과(관측 결과)가 나타날 확률'로 정의됩니다. <br/>보통 p-value < 0.05 이면, 신뢰할 수 있습니다.
##### 참고 : https://han-py.tistory.com/343
<br/>

  * Feature Importance code
    ```
    show_plot = True
    model = RandomForestRegressor(max_features="sqrt", n_jobs=-1, random_state=0xC0FFEE)
    model.fit(trainX, y)
    important_features = find_feature_importance(trainX, model, show_plot)
    X = trainX[important_features]
    print(X.shape)```
    
   * Feature Importance graph
