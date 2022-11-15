![header](https://capsule-render.vercel.app/api?type=waving&color=auto&height=200&text=Welcome!%20&fontSize=60&fontAlignY=40&desc=I'm%20joonho)


# PUBG_project

##### Task 목적 : 회귀모델을 활용해 우승자를 예상해야 함 <br/> Preprocessing(EDA) -> Feature Engineering -> Modeling(Hyper-parameter Tuning) -> Submission <br/>My part : 모든 과정 참여 <br/>Team : 5명

#### 개인 목적 : VIF 와 Feature Importance 에 대한 이해
<br/><br/>
## Preprocessing
<img src="https://user-images.githubusercontent.com/103080228/201855833-a35b3c13-0c89-4ded-a3ef-4a7393c016ab.jpg"  width="600" height="300">

* 결측치 처리 
* Feature 별 분석
<br/><br/>  
## Feature Engineering
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
<br/><br/>

				
		
* Feature Importance
<img src="https://user-images.githubusercontent.com/103080228/201831156-b39d4319-03e9-45dc-8963-a50f8485cee6.jpg"  width="600" height="300">
<br/>
---- Feature Importance 는 믿을 수 있을까?
<br/><br/>

* CART (Classification and Regression Tree)
  * 노드가 2개의 child 노드를 가지는 binary tree를, 불순도(impurity) 지표를 기준으로 생성해 나가는 알고리즘
  * 목적이 분류(Classification)일 때에는 불순도 지표로 Gini 계수 및 엔트로피를 이용
  * 목적이 회귀(Regression)일 때에는 MSE(Mean Square Error) 등을 이용<br/>--> 분산을 감소시키는 방향으로 노드를 형성<br/> 이 과정에서, 불순도를 가장 크게 감소시키는 변수의 중요도가 가장 크게 됩니다.<br/> CART 알고리즘은 바로 이 불순도를 이용해서 가장 중요한 변수들을 찾아냅니다.

* 불순도
  * 클래스가 섞이지 않고 분류가 잘 되었을 수록, 불순도가 낮음 <br/>반면 클래스가 섞여 있고, 반반인 경우에는, 불순도가 높음 <br/>의사결정나무 모델은 이 불순도가 낮아지는 방향으로 학습을 함
* GINI INDEX
  * 지니계수는 통계적 분산 정도를 정량화해서 표현한 값, 0과 1사이의 값을 가짐 <br/> 지니계수가 높을 수록 잘 분류되지 못한 것
* 트리들을 샘플도, 변수들도 랜덤으로 뽑아서 생성한 것을 몇백개 이상 합쳐서 얻은<br/> 변수 중요도는 충분히 신뢰할 만한 것.<br/>하지만 'scikit-learn의 디폴트 랜덤 포레스트 Feature Importance는 다소 biased하다’고 합니다.<br/>특히, 랜덤 포레스트는 연속형 변수 또는 카테고리 개수가 매우 많은 변수,<br/> 즉 ‘high cardinality’ 변수들의 중요도를 더욱 부풀릴 가능성이 높다고 합니다. <br/>왜 이런 결과가 나오는지는 정확히 알 수 없으나, cardinality가 큰 변수일 수록, <br/>노드를 쨀 게 훨씬 더 많아서 노드 중요도 값이 높게 나오는 게 아닐까 싶습니다. <br/>(출처 : https://soohee410.github.io/iml_tree_importance)
<br/>

## Modeling
<img src="https://user-images.githubusercontent.com/103080228/201857154-a78cce20-bc05-4fd4-83b4-6abe78b915f7.jpg"  width="600" height="300"><br/>

* Hyper-parameter Tuning using Optuna

	* Optuna는 하이퍼파라미터 최적화 태스크를 도와주는 프레임워크입니다.<br/>파라미터의 범위를 지정해주거나, 파라미터가 될 수 있는 목록을 설정하면 매 Trial 마다 <br/>파라미터를 변경하면서, 최적의 파라미터를 찾습니다.<br/><br/>
-- suggest_int : 범위 내의 정수형 값을 선택합니다.<br/>
n_estimator = trial.suggest_int('n_estimator', 100, 500)<br/><br/>
-- suggest_categorical : List 내의 데이터 중 선택을 합니다.<br/>
criterion = trial.suggest_categorical('criterion', ['gini', 'entropy']<br/><br/>
-- suggest_uniform : 범위 내의 균일 분포를 값으로 선택합니다.<br/>
subsample = trial.suggest_uniform('subsample', 0.2,0.8)<br/><br/>
-- suggest_discrete_uniform : 범위 내의 이산 균등 분포를 값으로 선택합니다.<br/>
max_faatures = trial.suggest_discrete_uniform('max_features', 0.05,1,0.05)<br/><br/>
-- suggest_loguniform : 범위 내의 로그 함수 선상의 값을 선택합니다.<br/>
learning_rate = trial.suggest_loguniform('learning_rate' : 1e-6, 1e-3)<br/>

* optuna 시각화툴

	* 하이퍼파라미터별 중요도 확인 : optuna.visualization.plot_importances(study)
	* 하이퍼파라미터 최적화 과정 확인 : optuna.visualization.plot_optimization(study)<br/>(출처 : https://ssoonidev.tistory.com/107)

* K-fold cross validation
