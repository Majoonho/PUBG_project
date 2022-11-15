![header](https://capsule-render.vercel.app/api?type=waving&color=auto&height=200&text=Welcome!%20&fontSize=60&fontAlignY=40&desc=I'm%20joonho)


# PUBG_project

##### Task 목적 : 회귀모델을 활용해 우승자를 예상해야 함 <br/> EDA -> Preprocessing -> Feature Engineering -> 학습 -> Hyper-parameter Tuning -> Submission <br/>My part : 모든 과정 참여 <br/>Team : 5명

#### 개인 목적 : VIF 와 Feature Importance 에 대한 이해

## EDA
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
 

