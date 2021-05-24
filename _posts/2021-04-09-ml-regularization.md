---
layout: single
title: "머신러닝 정규화 이해하기"
use_math: true
toc: true
toc_sticky: true
tags:
    - regularization
    - L1
    - L2
    - ridge
    - lasso
    - least squares method
    - linear regression
categories:
    - ML
---

## Introduction
---

그동안 머신러닝을 공부하면서 **정규화**와 관련된 내용이 나올 때마다 다급하게 블로그들을 뒤지기 일쑤였습니다.
따라서 그동안 찾아보았던 포스팅들을 기반으로 정규화에 대해 저의 글로 정리하려 합니다.

## 정규화?
---

**정규화**(\(Regularization\))란 모델이 미래에 주어질 미지의 데이터에도 정확히 예측하기 위해, <u>다시 말해 모델의 일반화 성능을 높이기 위해</u> 고안된 기법들의 총칭입니다.
수식적으로, 학습된 모델이 미래 데이터 \(x_0\)을 받아 예측했을 때 오차의 기대값은 다음과 같습니다.(\(손실함수가\,MSE일\,경우\))

<p style="text-align: center;"><img src="https://github.com/ITHwang/ITHwang.github.io/blob/master/_images/20210409-regularization-1.png?raw=true" alt="1st equation"></p>

실제값 \(y\)와 예측값 \(\hat {F}(x)\)의 오차를 제곱하여 평균을 낸 것입니다.
이 값을 최소화한다는 것은 미지의 데이터를 정확히 예측한다는 것이고, 그러한 모델을 두고 일반화가 잘 되었다고 말할 수 있을 것입니다.

식 1은 다음과 같은 형태로 분해될 수 있습니다.[증명](https://ratsgo.github.io/machine%20learning/2017/05/19/biasvar/)

<p style="text-align: center;"><img src="https://github.com/ITHwang/ITHwang.github.io/blob/master/_images/20210409-regularization-2.png?raw=true" alt="2nd equation"></p>

즉, 모델의 예측값 \(\hat {F}(x_0)\)의 편향과 분산이 작을 수록 모델은 더 일반적으로 예측을 수행할 수 있습니다.(\(\sigma^2은\,최적\,모델\,F*에\,대한\,이론적인\,오차입니다.\))
<u>정규화 기법들은 같은 오차에 대해서 편향이 다소 오르더라도 분산을 줄임으로써, 모델의 일반화를 도모합니다.</u>

## 일반 선형 회귀 모델
---

정규화를 적용하기 전에 **최소자승법**(\(Least\,Squares\,Method\))을 이용하여 에러를 최소화하는 회귀계수 \(\beta\)부터 구해보겠습니다.
회귀계수가 \(k\)개, 입력 벡터 **x**가 \(n\)개라면, 이를 예측한 것을 행렬로 다음과 같이 나타낼 수 있습니다.

<p style="text-align: center;"><img src="https://github.com/ITHwang/ITHwang.github.io/blob/master/_images/20210409-regularization-3.png?raw=true" title="source: https://ratsgo.github.io/machine%20learning/2017/05/22/RLR/"></p>

\(\hat {y} = X\beta\)임을 이용하여 최소자승법을 통해 MSE를 최소화하는 회귀계수 \(\beta\)를 구한다면,

<p style="text-align: center;"><img src="https://github.com/ITHwang/ITHwang.github.io/blob/master/_images/20210409-regularization-4.png?raw=true" title="source: https://ratsgo.github.io/machine%20learning/2017/05/22/RLR/"></p>

이며, 이 식을 \(\beta\)로 미분한 후 0으로 놓고 풀면 다음과 같이 최적의 회귀계수 \(\beta\)를 얻을 수 있습니다.

<p style="text-align: center;"><img src="https://github.com/ITHwang/ITHwang.github.io/blob/master/_images/20210409-regularization-5.png?raw=true" title="source: https://www.youtube.com/watch?v=JYOA2zNL9bw"></p>

- 오타: 마지막에
  
    \[(X^T X)^{-1} X^T y\]

    입니다.

이 \(\beta\)는 bias가 없는 추정량 가운데 variance가 가장 작다고 하여 **Best Linear Unbiased Estimator**라고 합니다. 
이제 앞으로 \(\beta\)와 variance를 줄이면서 일반화를 꾀해보겠습니다. 

## 릿지 회귀
---

**릿지 회귀**(\(Ridge Regression\))는 \(MSE\)를 최소화하면서 회귀 계수 \(\beta\)의 \(L_2\,norm\)을 제한하는 기법입니다. 
\(L_1,\,L_2,\,norm\)에 관하여는 [이곳](https://www.stand-firm-peter.me/2018/09/24/l1l2/)을 참고해주세요.

<p style="text-align: center;"><img src="https://github.com/ITHwang/ITHwang.github.io/blob/master/_images/20210409-regularization-6.png?raw=true" title="source: https://ratsgo.github.io/machine%20learning/2017/05/22/RLR/"></p>

이 또한 \(\beta\)에 대하여 미분한 후 0을 놓고 풀면 에러를 최소화하는 \(\beta\)를 구할 수 있습니다.

<p style="text-align: center;"><img src="https://github.com/ITHwang/ITHwang.github.io/blob/master/_images/20210409-regularization-7.png?raw=true"></p>

이 때 \(lambda\)를 통해 회귀 변수 \(\beta\)의 \(L_2\,norm\)을 제한합니다.
어떻게 제한하는지 알아보기 위해 \(\beta\)를 \([\beta_1,\,\beta_2]\)로 두 개만 놓고 \(MSE\)를 써보겠습니다.

<p style="text-align: center;"><img src="https://github.com/ITHwang/ITHwang.github.io/blob/master/_images/20210409-regularization-8.png?raw=true" title="source: https://ratsgo.github.io/machine%20learning/2017/05/22/RLR/"></p>

이는 \(A\beta_1^2+B\beta_1\beta_2+C\beta_2^2+D\beta_1+E\beta_2+F\) 형태인 **원추곡선**(\(conic\,equation\))이 됩니다.

판별식 \(B^2-4AC\)을 계산해보면,

\[4(\sum_{i=1}^{n} {x_{i1}x_{i2}})^2 - 4(\sum_{i=1}^{n} {x_{i1}^2})(\sum_{i=1}^{n} {x_{i2}^2})\]

인데, 코시-슈바르츠 부등식 \((a^2+b^2)\)\((x^2+y^2)\)\(\ge(ax+by)^2\)에 의해 0보다 작으므로 **타원** 형태가 됩니다.

<p style="text-align: center;"><img src="http://i.imgur.com/RBxwo0D.png" width="700px" title="source: imgur.com"></p>

정규화가 없을 경우의 \(\hat{\beta}^{LS}\)을 중심으로, \(\lambda\)가 커질 수록 같은 에러에 대해 남기는 \(\beta\)의 자취, 즉 타원이 점점 커집니다.
타원이 커지는 이유는, \(\lambda\)가 커지면서 같은 \(L_2\,norm\)에 대해 남기는 \(\beta\)의 자취인 파란색 원이 작아지기 때문입니다.

녹색 선은 파란색 원과 겹쳐야 \(\lambda\)가 내건 조건을 만족하는데, 그렇다고 최소 에러와는 최대한 멀어지면 안되니 파란색 원과 한 점에서 접하는 꼴을 보이는 것이죠.
<u>바로 그 점이 정규화에 걸린 상황에서 최소 에러를 내는 최적의 회귀 변수</u>인 것입니다.
\(\lambda\)가 커지면서 파란색 원이 작아질 수록, \(\beta\)의 \(L_2\,norm\)도 작아진다는 것은 오른쪽 그래프를 통해서도 알 수 있습니다.

이처럼 릿지 회귀는 회귀 변수의 크기를 줄여줌으로써, 특정 회귀 변수에 대한 과적합을 해결해줍니다. 즉 비슷한 미래 데이터에 대해서 보다 일관적인 예측을 할 수 있으므로 비교적 **stable**합니다.
다만 회귀 변수를 feature로 바꾸어 생각해본다면, 크기만 줄여주므로 <u>특정 회귀 변수에 대해 아예 무시하는 것은 불가능</u>합니다. 따라서 outlier인 미래 데이터가 주어졌을 때 이 데이터의 스코어를 높여주는 회귀 변수를 배제하지 않아 비교적 **non-robustness**합니다.
이외에 미분이 가능하여 최적의 회귀 변수를 바로 알아낼 수 있는 장점이 있습니다.

## 라쏘 회귀
---

**라쏘 회귀**(\(Least\,Absolute\,Shrinkage\,and\,Selection\,Operator\))는 릿지 회귀와 달리 회귀 변수 \(\beta\)의 \(L_1\,norm\)을 제약합니다.

<p style="text-align: center;"><img src="https://github.com/ITHwang/ITHwang.github.io/blob/master/_images/20210409-regularization-9.png?raw=true" title="source: https://ratsgo.github.io/machine%20learning/2017/05/22/RLR/"></p>

각 요소에 절대값을 취하여 더하는 \(L_1\,norm\)에서는 \(\beta\)에 대한 미분이 불가능하기 때문에 최소 에러를 내는 최적 회귀 변수를 바로 찾아낼 수 없습니다.

<p style="text-align: center;"><img src="http://i.imgur.com/xsa2fNQ.png" width="600px" title="source: imgur.com"></p>

같은 MSE에 대한 \(\beta\)의 자취는 릿지 회귀같이 타원이지만, 같은 \(L_1\,norm\)에 대한 \(\beta\)의 자취는 마름모 꼴이 됩니다.

\(\lambda\)가 제한하는 \(norm\)만큼 \(\beta\)를 줄이다보면 그 중 MSE가 최소인 곳은 마름모의 꼭짓점이 됩니다.
즉 어느 한 축에 대한 값이 0이 되면서, 그 축을 담당하고 있던 회귀 변수가 아예 제외되는 **feature selection** 효과가 있습니다.
따라서 sparse matrix와 같이 중요도가 낮은 피처나 회귀 변수가 많을 때 사용하는 것이 적절합니다. 

또한 미래에 outlier가 주어졌을 때, 이 데이터의 스코어를 높이는 피처를 무시하기 때문에 비교적 **robustness**합니다.
다만 비슷한 데이터의 스코어를 높이는 피처는 제약을 덜 받기 때문에, 미래에 비슷한 데이터가 나올 경우 다소 일관적이지 않아 비교적 **unstable**합니다.
또한 같은 \(\lambda\)일 때 릿지 회귀보다 제약 조건을 충족하기 어렵기 때문에(\(마름모의\,넓이가\,더\,좁음\)), \(\lambda\) 값에 민감한 편입니다.

## Conclusion
---

릿지 회귀와 라쏘 회귀를 표로 정리했습니다.

<table>
    <thead>
    <tr>
        <th></th>
        <th>릿지 회귀</th>
        <th>라쏘 회귀</th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <td><strong>정규화 시 사용되는 norm</strong></td>
        <td>L2</td>
        <td>L1</td>
    </tr>
    <tr>
        <td><strong>회귀 변수에 대한 미분</strong></td>
        <td>가능</td>
        <td>불가능</td>
    </tr>
    <tr>
        <td><strong>feature selection</strong></td>
        <td>불가능</td>
        <td>가능</td>
    </tr>
    <tr>
        <td><strong>stable</strong></td>
        <td>높음</td>
        <td>낮음</td>
    </tr>
    <tr>
        <td><strong>robustness</strong></td>
        <td>낮음</td>
        <td>높음</td>
    </tr>
    <tr>
        <td><strong>정규화 파라미터에 대한 민감도</strong></td>
        <td>낮음</td>
        <td>높음</td>
    </tr>
    <tr>
        <td><strong>특징</strong></td>
        <td>크기가 큰 변수를 먼저 줄임</td>
        <td>중요하지 않은 변수를 먼저 줄임</td>
    </tr>
    </tbody>
</table>

이처럼 릿지 회귀와 라쏘 회귀 모두 장단점이 뚜렷하기 때문에, 둘을 혼합하여 장점을 유연히 활용하는 **엘라스틱넷**(\(Elastic\,Net\))도 있습니다.
이는 제약할 때 \(L_1,\,L_2\,norm\) 모두 사용합니다.

## Reference
---

- [Bias-Variance Decomposition](https://ratsgo.github.io/machine%20learning/2017/05/19/biasvar/)
- [Regularized Linear Regression](https://ratsgo.github.io/machine%20learning/2017/05/22/RLR/)
- [5-1강. 스칼라를 벡터로 미분!](https://www.youtube.com/watch?v=JYOA2zNL9bw)
- [L1 & L2](https://www.stand-firm-peter.me/2018/09/24/l1l2/)