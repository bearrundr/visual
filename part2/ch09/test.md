# Ch09_Robust_Regression
Encaion  
Thursday, November 27, 2014  
<<< robust regression >>>

이번 9장은 여러분이 앞에서 선형회귀를 공부했다는 가정 하에 진행할 수 있는 챕터 입니다.
비록 선형 회귀에 대한 이해가 부족하더라도 최대한 쉽게 이해할 수 있도록 자세한 설명을 달겠습니다.

선형 회귀는 분석하고자 하는 데이터 변수들 간의 선형성을 보려고 하는 분석입니다.
선형(linear)과 비선형(non-linear)라는 것이 있는데요. 
가장 간단한 예제. (좀 억지지만.. 이해바랍니다)
선형 : 1+1=2 와 같이 결과가 단순한 더하기 빼기 정도로 계산이 되는 것
비선형 : 1+1=귀요미 라는 요상한 값이 나오는 것입니다.
비선형이라고 수식어가 붙어있으면 실제 현상을 좀 더 정확히 분석할 수 있지만 미적분의 끝을 달리는 매우 어려운 것들이 많습니다.
일단 우리는 보기도 쉽고 계산하기도 쉬운 '선형'회귀분석에 대하여 조금 더 알아보도록 합시다.


사실 여러분은 선형회귀라는 분석을 이전에 다른 이름으로 많이 접해왔습니다.
추세선(Trend-line)인데요, 보통 주식에서 그래프를 그린 다음
그래프를 관통하는 직선을 쭉~ 그으면서 이 주식은 가격이 오를 것입니다! 라고 할 때 쓰는 그 선이 바로
'선형회귀선'
입니다.
그런데 이런 선형회귀 분석에서도 단점이 있습니다.
그 중 하나가 특정한 이상치(값이 상대적으로 엄청 크거나 작은 경우)에 영향을 많이 받는다는 것인데요.
이제 공부할 코드를 통해서 선형회귀 결과가 얼마나 많이 영향을 받는지 알아보고
단점을 보완하는 로버스트 선형회귀에 대해서 알아보도록 하겠습니다.

set.seed는 난수(random number)를 생성하기 위하여 지정해주는 난수 테이블의 번호 입니다.
난수 생성시 seed를 설정하고 이를 공유하면 다른 사람들이 난수를 사용한 통계 또는
각종 계산을 실습 할 때 같은 결과를 얻을 수 있습니다. 
즉, 안의 seed 숫자는 아무거나 넣으셔도(자연수) 상관은 없습니다만, 자신의 계산 결과가 남과 똑같은지 확인하시려면
seed 숫자를 통일하셔야 합니다.
그런데 중간에 여러가지 다른 명령어를 넣다 보면 seed 숫자가 바뀌어 버립니다.(정확히 어떤 작업인지는 잘 모름...)
그래서 확률 계산 등을 하기 직전에 다시 set.seed() 명령어를 써주는 것이 좋습니다.



```r
set.seed(1234567)
x = seq(1,10)
y = 2.5 + 0.5*x + rnorm(10,0,1)
y[10] = -10  # 마지막 열번째 값을 -10으로 바꿔줍니다.
```


"par(mfrow=c(2,3))"

위 코드는 plot 창을 분할해서 왼쪽부터 오른쪽으로 새로운 plot을 채우는 것입니다.
명령어는 mfrow=c(세로, 가로) 입니다.

![](test_files/figure-html/unnamed-chunk-2-1.png) 

y의 식에 x를 대입한 결과를 출력하고 y축의 출력 범위는 -10 부터 10 까지 입니다.


```r
library(MASS)
```


```r
m0 = lm(y ~ x)  
```

y(종속변수), x(독립변수)로 만들어진 선형회귀 모형입니다.

```r
m1 = rlm(y ~ x)
```

후버의 M-추정을 사용한 로버스트 선형회귀 모형입니다.

```r
m2 = lqs(y ~ x)
```

LTS(Least Trimmed Squares, 최소 절삭 제곱) 회귀 모형입니다.


앞에서 만든 회귀모형 3개의 결과(회귀선, 계수)를 나타내어봅시다.


![](test_files/figure-html/unnamed-chunk-7-1.png) 

m0 모델의 계수(beta값)를 그립니다.

![](test_files/figure-html/unnamed-chunk-8-1.png) 

m1 모델의 계수(beta값)를 그립니다.

![](test_files/figure-html/unnamed-chunk-9-1.png) 

m2 모델의 계수(beta값)를 그립니다.


그래프를 보시면 선형 회귀는 -10을 가지는 한 점에 의해서 그 기울기가 상당히 내려간 것을 볼 수 있습니다.
하지만 여러분이 점의 분포를 보면 딱 아실 수 있듯이 군집의 추세가 내려가는 것을 결코 아니라는 거죠.
상승하는 두 점선을 로버스트 회귀선이 그려진 것인데요, 파란색 점선과 빨간색 점선을 구분해서 보시면 되겠습니다.

그래서 뭐가 제일 좋은데!?!? 

일단 이 데이터에서는 선형회귀보다 로버스트 회귀가 좋지만, 이상치 값에 대한 탐구도 이루어져야 하며
해당 데이터가 가지는 속성과 추출한 샘플의 숫자에는 문제가 없었는지 확인을 한 다음에 최종적으로 판단을 내려야 합니다.

다음에는 후버의 M-추정, LMS(least median of squares), LTS(least trimmed squares)에 대하여 알아봅시다.
후버의 M-추정은 특정 잔차가 상수값 보다 크면 2차식을 1차식으로 바꾸어 덜 민감하게끔 해준다.. 라고 하는데요
그냥 이 방법은 너무 튀는 값들을 조금 상쇄시켜(제곱근) 회귀식의 이상치에 대한 민감도를 떨어뜨리는 효과를 내는 것입니다.



```r
psi.0 = function(z){z^2}
psi.1 = function(z){c = 1.345; ifelse(abs(z)<c, z^2, c*(2*abs(z)-c))}
z = seq(-6,6,0.1)
y.0 = psi.0(z)
y.1 = psi.1(z)
```


![](test_files/figure-html/unnamed-chunk-11-1.png) 


책 164페이지에 MAD라는 것이 언급 되는데요
MAD(median of absolute deviations)는 절대 편차(?)의 중앙값이라는 것입니다.
그런데 이게 틸다(~)가 위에 붙어있는 이유는 계산시 나누어주는 값인 0.675가 추정치라는 점입니다.
정규분포의 Z의 값에 절대값을 취했기 때문에 정규분포에서 0을 기준으로 반으로 접은(대략) 형태의 분포가 되는 것이죠
그래서 그 값들의 중앙값이 0이 아닌 0.675가 되는 것이며 이 값은 정확한 값이 아니라 추정치 입니다.

이제 다음 로버스트 회귀에 대해서 알아보도록 합시다.
이번에는 LMS와 LTS라는 것에 대하여 알아볼텐데요,

LMS(Least Median of Squares)는 계산할 때 몬테칼로(Monte-Carlo) 반복시행으로 얻어진 결과값을 모아서 최종 결과를 산출합니다.
이는 특정 확률을 설정하고 여러번 반복시행을 하여 (예를 들어 동전던지기를 수십번 시행) 계산을 하기 때문에 확률에 확률을 더하는 꼴이라
예측에는 신뢰도가 약간 떨어질 수 있다고 말하기도 합니다. 물론 시행 횟수가 엄청 많으면 또 달라지겠죠.



```r
set.seed(1234567)
x = seq(1,10)
y = 2.5 + 0.5*x +rnorm(10,0,1)
x[10] = 100
y[10] = -100
```


```r
m1 = rlm(y ~ x); m1
```

```
## Call:
## rlm(formula = y ~ x)
## Converged in 8 iterations
## 
## Coefficients:
## (Intercept)           x 
##   10.244545   -1.094401 
## 
## Degrees of freedom: 10 total; 8 residual
## Scale estimate: 3.67
```

```r
m2 = lqs(y ~ x, method="lms"); m2
```

```
## Call:
## lqs.formula(formula = y ~ x, method = "lms")
## 
## Coefficients:
## (Intercept)            x  
##      4.1039       0.2368  
## 
## Scale estimates 0.4394 0.3132
```

![](test_files/figure-html/unnamed-chunk-14-1.png) 


LTS(Least Trimmed Squares)는 계산시 잔차가 큰 관측치를 제거하고 나머지 값(관측치)로 계산하는 것이 특징입니다.
예를 들어서 A반 남학생들의 1년간 시험 점수 추이를 계산해본다고 합시다.
결과를 산출할 때, 중간에 전염병에 걸려 시험을 치루지 못하여 0점 처리된 학생의 점수는 빼는 것이죠.
어떻게 보면 회귀분석을 하기 이전에 이상치를 제거하는 것과 이후에 제거하는 것의 차이라고도 할 수 있는데
전자와 후자의 의미는 좀 다릅니다. 한마디로 필요충분조건이 만족이 되지 않는 것이죠.
전자는 그냥 말이 안되는 것을 빼는 것이 보통이고, 후자는 통계 계산에서 유의하지 않을 것 같은 값들을 빼는 것이죠.
다시 LTS로 돌아와서.. 제거되는 잔차가 큰 관측치의 수는 분석가가 임의로 설정합니다.
책에는 계산에 사용할 관측치를 75% 정도로 설정하면 추정치의 정확도가 높아진다고 하는 이론적 연구결과가 있다고 합니다.
하지만 분석에 사용하는 데이터의 변수가 많으면서 관측치가 적은 경우는 좋은 분석 결과를 기대하기 힘들 것입니다.



```r
m2a = lqs(y ~ x, method="lts"); m2a
```

```
## Call:
## lqs.formula(formula = y ~ x, method = "lts")
## 
## Coefficients:
## (Intercept)            x  
##      3.7625       0.2723  
## 
## Scale estimates 1.017 1.101
```

```r
m2b = lqs(y ~ x, method="lts", quantile=8); m2b
```

```
## Call:
## lqs.formula(formula = y ~ x, quantile = 8, method = "lts")
## 
## Coefficients:
## (Intercept)            x  
##      3.5330       0.2368  
## 
## Scale estimates 1.204 1.134
```


![](test_files/figure-html/unnamed-chunk-16-1.png) 


앞에서 단순선형회귀를 했으니 다중선형회귀를 실습해봅시다.
다중선형회귀는 분석에 사용되는 변수의 개수가 두 개 이상인 경우를 말합니다.


```r
library(MASS)
data(stackloss)
attach(stackloss)
```

```
## The following object is masked _by_ .GlobalEnv:
## 
##     stack.loss
## 
## The following object is masked from package:datasets:
## 
##     stack.loss
```

```r
str(stackloss)
```

```
## 'data.frame':	21 obs. of  4 variables:
##  $ Air.Flow  : num  80 80 75 62 62 62 62 62 58 58 ...
##  $ Water.Temp: num  27 27 25 24 22 23 24 24 23 18 ...
##  $ Acid.Conc.: num  89 88 90 87 87 87 93 93 87 80 ...
##  $ stack.loss: num  42 37 37 28 18 18 19 20 15 14 ...
```

```r
m1 = lm(stack.loss ~ Air.Flow + Water.Temp + Acid.Conc.); m1
```

```
## 
## Call:
## lm(formula = stack.loss ~ Air.Flow + Water.Temp + Acid.Conc.)
## 
## Coefficients:
## (Intercept)     Air.Flow   Water.Temp   Acid.Conc.  
##    -39.9197       0.7156       1.2953      -0.1521
```

다중선형회귀

```r
m2 = rlm(stack.loss ~ Air.Flow + Water.Temp + Acid.Conc.); m2
```

```
## Call:
## rlm(formula = stack.loss ~ Air.Flow + Water.Temp + Acid.Conc.)
## Converged in 9 iterations
## 
## Coefficients:
## (Intercept)    Air.Flow  Water.Temp  Acid.Conc. 
## -41.0265311   0.8293739   0.9261082  -0.1278492 
## 
## Degrees of freedom: 21 total; 17 residual
## Scale estimate: 2.44
```

후버의 M-추정을 사용한 로버스트 회귀


```r
m3 = lqs(stack.loss ~ Air.Flow + Water.Temp + Acid.Conc., method="lms"); m3
```

```
## Call:
## lqs.formula(formula = stack.loss ~ Air.Flow + Water.Temp + Acid.Conc., 
##     method = "lms")
## 
## Coefficients:
## (Intercept)     Air.Flow   Water.Temp   Acid.Conc.  
##  -3.425e+01    7.143e-01    3.571e-01    1.753e-16  
## 
## Scale estimates 0.5514 0.4798
```

LMS 회귀


```r
m4 = lqs(stack.loss ~ Air.Flow + Water.Temp + Acid.Conc., method="lts", quantile=16); m4 
```

```
## Call:
## lqs.formula(formula = stack.loss ~ Air.Flow + Water.Temp + Acid.Conc., 
##     quantile = 16, method = "lts")
## 
## Coefficients:
## (Intercept)     Air.Flow   Water.Temp   Acid.Conc.  
##   -35.22037      0.81681      0.45690     -0.07759  
## 
## Scale estimates 1.459 1.306
```

LTS 회귀


상자그림을 보면서 각 회귀식의 잔차의 형태를 볼 수 있습니다. 상자그림의 크기와 이상치의 개수를 유심히 보시면 됩니다.
![](test_files/figure-html/unnamed-chunk-21-1.png) 

여기부터는 그냥 느낌만 가지고 가시죠.

```r
psi.1 = function(z){c = 1.345; ifelse(abs(z)<c, z, ifelse(z >c, c, -c))}
w.1 = function(z){c = 1.345; ifelse(abs(z)<c, 1, c/abs(z))}
z = seq(-6, 6, 0.1)
y.1 = psi.1(z)
y.2 = w.1(z)
```

![](test_files/figure-html/unnamed-chunk-23-1.png) 






