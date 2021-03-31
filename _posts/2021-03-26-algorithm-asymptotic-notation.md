---
layout: single
title: "점근 표기법 총망라!"
use_math: true
---

## Introduction
---

우리는 알고리즘이 얼마나 효율적으로 짜여졌는지 확인하기 위해 <u>input size에 따른 running time</u>을 측정합니다.
가끔 이를 정밀하게 측정하긴 하지만, 보통 정밀한 running time에 큰 의미를 두지 않습니다.
결국 이는 input size에 가장 많은 영향을 받으므로, <u>input size가 증가할 때 running time이 어떻게 증가하는지</u>에 대해 아는 것만으로도 효율성을 파악하기에 충분하기 때문이죠.

input size \(n\) 외에 running time에 영향을 주는 요소들을 모두 상수 \(c\)로 취급한다면, running time을 \(n\)에 대한 함수 \(f(n)\)=\(an^2 + bn + c\)로 표현할 수 있을 것입니다.
그리고 굉장히 큰 \(n\)에 대해서 \(f(n)\)은 \(n^2\)에 가장 많이 영향을 받으므로 \(n^2\)에 대한 함수로 축약할 수 있을 텐데, 이 때 사용하는 것이 **점근 표기법**(\(Asymptotic\,notation\))입니다.

저는 \(f(n)\)=\(O(n^2)\)과 같이 점근 표기법 중 빅-오 표기법만 사용해왔으나, 최근에 표기 의도(\(\text{어떤 running time을 의미하는지}\))에 따라 사용할 수 있는 다양한 표기법에 대해 알게 되었고 이에 대해 정리하려 합니다. 

## \(\Theta\)-notation
---

우리가 점근 표기법을 사용했다면, 사실 그 표기에 부합하는 함수들은 다수이므로 집합으로 묶을 수 있습니다.
따라서 점근 표기법을 집합으로 본다면 **\(\Theta\)-notation**이라는 집합의 조건 제시법은 다음와 같습니다.

\(\Theta(g(n))\) = {\(f(n)\): \(there\,exist\,positive\,constants\, c_1,\,c_2,\,and\,n_0\,such\,that\, 0 \le c_1g(n)\)\(\le f(n)\)\(\le c_2g(n)\)\(\,for\,all\, n \ge n_0\)}

즉 \(f(n)\)이 \(n_0\) 이상의 모든 지점에서 \(0 \le c_1g(n)\)\(\le \(f(n)\)\(\le c_2 g(n)\)을 충족한다면 \(\Theta(g(n))\)이라고 표기할 수 있습니다.

<p style="text-align: center;"><img src="https://github.com/ITHwang/ITHwang.github.io/blob/master/_images/20210326-asymptotic-notation-1.png?raw=true" alt="Theta-notation"></p>

충분히 큰 \(n\)에 대해서 상한선 \(c_2g(n)\)과 하한선 \(c_1g(n)\) 사이에 \(f(n)\)이 존재한다면 \(f(n)\)은 집합 \(\Theta(g(n))\)의 원소가 되므로 \(f(n)\)\(\in\)\(\Theta(g(n))\)이라고도 쓸 수 있습니다.
이 때 우리는 \(g(n)\)이 \(f(n)\)에 대해서 \(asymptotically\,tight\,bound\)되었다고 말합니다.

또한 \(\Theta(g(n))\)을 정의하기 위해서 그 안에 속한 \(f(n)\)이 \(asymptotically\,nonnegative\) 해야 합니다. 
다시 말해 충분히 큰 모든 \(n\)에 대해서 항상 양수여야 하며, 그렇지 않다면 \(\Theta(g(n))\)은 공집합이 될 것입니다. 이 조건은 다른 점근 표기법들에서도 적용됩니다.

만약 \(f(n)\)=\(an^2 + bn + c\)을 \(\Theta\)-notation으로 표기한다면, \(0 \le c_1n^2 \le an^2 + bn + c \le c_2n^2\,for\,all\,n \ge n_0\)에 알맞은 \(c_0,\,c_1,\,n_0\)을 찾을 수 있을 것이고 \(f(n)\)=\(\Theta(n^2)\)이라고 표기할 수 있습니다.
이런 식으로 우리는 모든 다항식 \(p(n)\)=\(\sum_{i=0}^{d} {a_in^i}\)에 대해서 \(p(n)\)=\(\Theta(n^d)\)와 같이 표기하는 것으로 일반화할 수 있습니다.(\(단,\,a_i는\,정수이며\,a_d는\,0보다\,큽니다.\))


샌드위치 형식으로 \(f(n)\)을 점근적 표기하는 \(\Theta\)-표기법은 average running time을 표기할 때 주로 사용됩니다.

## O-notation
---

\(\Theta\)-notation에서 lower bound를 없앤 것이 **\(O\)-notation**입니다. 
Upper bound만 남으므로 이 표기법을 조건 제시법으로 표현하면 다음과 같습니다.

\[O(g(n)) = \{f(n): \text{there exist positive constants \(c\) and \(n_0\) such that \(0 \le f(n) \le cg(n)\) for all \(n \ge n_0\)}\}\]

\(n_0\)보다 큰 모든 \(n\)에 대해서 \(f(n)\)이 \(cg(n)\) 이하라면 \(f(n)\)은 \(O(g(n))\)의 원소가 됩니다.

<p style="text-align: center;"><img src="https://github.com/ITHwang/ITHwang.github.io/blob/master/_images/20210326-asymptotic-notation-2.png?raw=true" alt="Big-oh-notation"></p>

그림을 통해 직관적으로 \(O\)-notation이 \(\Theta\)-notation보다 덜 엄격한 집합임을 알 수 있습니다.
즉 \(\Theta\)-notation이 \(O\)-notation의 부분집합이므로 \(\Theta(g(n)) \subseteq O(g(n))\)이라고 할 수 있겠습니다.

일부 문헌에서 \(O\)-notation도 \(\Theta\)-notation과 같이 asymptotic tight bound로 기술하고 있으나, 제가 참고한 \(Introduction\,to\,algorithms\,3rd\,edition(Cormen\,et\,al.)\)의 저자는 \(O\)-notation을 **asymptotic upper bound**로 기술하고 있으며, 그는 이를 asymptotic tight bound와 구분해야 한다고 말합니다.

\(O\)-notation은 worst case일 때 발생하는 running time보다 같거나 큰 upper bound를 둠으로써, 모든 \(n\)에 부합하는 점근적 표기가 가능하므로, worst case에 사용하는 것이 적절합니다.
쉽게 말해 <u>아무리 큰 running time이라도 uppper bound보다 같거나 작음을 보장</u>합니다.

예를 들어 삽입 정렬의 worst-case running time을 \(\Theta(n^2)\)라고 표기한다면 running time이 \(\Theta(n)\)인 경우(\(\text{모든 원소가 올바르게 정렬되어 있는 경우}\))를 포함하지 않기 때문에 모든 \(n\)에 대해서 성립하지 않는 반면, \(O\)-notation은 모든 \(n\)에 대해서 성립합니다.
따라서 "삽입 정렬의 running time은 \(O(n^2)\)다"보다 <strong>"삽입 정렬이 worst case일 경우 running time은 \(O(n^2)\)다"</strong>가 더 정확한 표현일 것입니다.

## \(\Omega\)-notation
---

\(O\)-notation과 반대로, **\(\Omega\)-notation**은 asymptotic lower bound를 제공합니다.
조건 제시법은 다음과 같습니다.

\[\Omega(g(n)) = \{f(n): \text{there exist positive constants \(c\) and \(n_0\) such that \(0 \le cg(n) \le f(n)\) for all \(n \ge n_0\)}\}\]

\(n_0\)보다 큰 모든 \(n\)에 대해서 \(f(n)\)이 \(cg(n)\) 이상이라면 \(f(n)\)은 \(\Omega(g(n))\)의 원소가 됩니다.

<p style="text-align: center;"><img src="https://github.com/ITHwang/ITHwang.github.io/blob/master/_images/20210326-asymptotic-notation-3.png?raw=true" alt="Big-omega-notation"></p>

\(\Omega(g(n))\) 또한 \(\Theta(g(n))\)보다 덜 엄격한 집합이므로 \(\Theta(g(n)) \subseteq \Omega(g(n))\)이 성립합니다. 
따라서 지금까지 살펴본 세 점근 표기법을 통해 다음과 같은 결론이 도출됩니다.

> Theorem: For any two functions \(f(n)\) and \(g(n)\), we have \(f(n) = \Theta (g(n))\) if and only if \(f(n) = O (g(n))\) and \(f(n)=\Omega (g(n))\).

best case일 경우, \(\Omega\)-notation을 통해 running time보다 같거나 작은 lower bound를 두면 모든 \(n\)에 부합하므로, best case에 대해 이를 사용하는 것이 적절합니다.
즉 <u>아무리 작은 running time이라도 lower bound보다 같거나 큼을 보장</u>합니다.
 
삽입 정렬의 worst-case running time을 \(\Omega(n^2)\)라고 표기한다면 running time이 \(\Theta(n)\)인 경우(\(모든\,원소가\,올바르게\,정렬되어\,있는\,경우\))를 포함하지 않기 때문에 모든 \(n\)에 대해서 성립하지 않을 것입니다.
그러나 best-case running time을 \(\Omega(n)\)라고 한다면 이는 \(\Theta(n)\)도 포함하기 때문에 모든 \(n\)에 대해서 성립합니다. 따라서 <strong>"삽입 정렬이 best case일 경우 running time은 \(\Omega(n)\)이다"</strong>라고 말할 수 있습니다.

## o-notation and \(\omega\)-notation
---

경우에 따라 \(O\)-notation이 제공하는 asymptotic upper bound는 다소 엄격할 수도 있습니다.(\(물론\,\Theta(g(n))\,보다는\,아니지만요.\)) 
그래서 만약 \(f(n) = 2n\)에 대해 upper bound를 더 올린다면 \(2n = O(n^2)\)과 같이 표현 할 수 있을 것입니다. 
다만 이와 같이 <u>점근적으로 엄격하지 않은 upper bound</u>라면 소문자로 바꿔서 \(2n=o(n^2)\)라고 표기하는데, 이를 **\(o\)-notation**이라고 합니다.

\(O\)-notation과 가장 큰 차이점은 \(O(g(n))\)에서 upper bound \(cg(n)\)를 성립하는 양수 c가 **한정적**이나, \(o(g(n))\)에서는 **모든 양수**가 c가 될 수 있습니다.
방금 예시에서 \(f(n)=2n\)에 대해 upper bound를 \(cn^2\)으로 두었는데, \(c\)는 0보다 크기만 하면 절대 \(f(n)\)보다 작을 수 없습니다.

\(o\)-notation이 타이트한 upper bound를 높인 것처럼, \(\Omega\)-notation이 제공하는 asymptotic lower bound를 더 느슨하게 낮춘다면 소문자로 바꿔서 \(f(n) = \omega (g(n))\)과 같이 표기하는데 이를 **\(\omega\)-notation**이라고 합니다.
예시로 \(f(n) = n^2/2\)에 대해서 \(f(n) = \omega(n)\)라고 표기할 수 있습니다.

두 표기법 모두 bound \(cg(n)\)보다 크게 상회하거나 하회하는 함수들을 원소로 가지고 있기 때문에, \(f(n) \in \omega (g(n))\)과 \(g(n) \in o (f(n))\)은 <strong>필요충분조건</strong>입니다. 

## Comparing functions
---

지금까지 알아본 표기법들 사이에는 몇 가지 관계적인 특성을 찾아볼 수 있습니다.

- **Transitivity**(\(이행성\))

    \[f(n) = \Theta(g(n)) \,and\, g(n) = \Theta(h(n)) \,imply\, f(n) = \Theta(h(n)),\]
    \[f(n) = O(g(n)) \,and\, g(n) = O(h(n)) \,imply\, f(n) = O(h(n)),\]
    \[f(n) = \Omega(g(n)) \,and\, g(n) = \Omega(h(n)) \,imply\, f(n) = \Omega(h(n)),\]
    \[f(n) = o(g(n)) \,and\, g(n) = o(h(n)) \,imply\, f(n) = o(h(n)),\]
    \[f(n) = \omega(g(n)) \,and\, g(n) = \omega(h(n)) \,imply\, f(n) = \omega(h(n)).\]

    어떤 표기법을 사용하든 \(h(n)\)의 차수가 \(g(n)\)의 차수보다 같거나 크기 때문에 성립합니다.

- **Reflexivity**(\(반영성\))

    \[f(n) = \Theta(f(n)),\]
    \[f(n) = O(f(n)),\]
    \[f(n) = \Omega(f(n)).\]

    타이트한 표기법들의 경우 \(f(n)\)과 \(g(n)\)이 같아도 성립합니다.

- **Symmetry**(\(대칭성\))

    \[f(n) = \Theta(g(n)) \,if\,and\,only\,if\, g(n) = \Theta(f(n)).\]

    \(f(n)\)이 \(c_1g(n)\)과 \(c_2g(n)\)으로 묶인다면, 그 반대도 성립합니다. 다른 표기법들은 상한선이나 하한선이 없어 임의의 \(n\)에 대해서 대칭성을 유지할 수 없습니다.

- **Transpose symmetry**(\(전치 대칭성\))

    \[f(n) = O(g(n)) \,if\,and\,only\,if\, g(n) = \Omega(f(n)),\]
    \[f(n) = o(g(n)) \,if\,and\,only\,if\, g(n) = \omega(f(n)).\]

    다만 상한선을 만드는 함수 \(g(n)\)과 하한선을 만드는 함수 \(f(n)\)을 바꿔준다면 \(\Theta\)-notation 외 다른 표기법들도 대칭을 이룰 수 있습니다.

이와 같은 특성들을 바탕으로 우리는 두 함수 \(f\)와 \(g\)의 점근적 비교를 두 실수 \(a\), \(b\)에 빗대어 표현할 수 있습니다.

\[f(n) = O(g(n)) \,is\,like\, a \le b,\]
\[f(n) = \Omega(g(n)) \,is\,like\, a \ge b,\]
\[f(n) = \Theta(g(n)) \,is\,like\, a = b,\]
\[f(n) = o(g(n)) \,is\,like\, a < b,\]
\[f(n) = \omega(g(n)) \,is\,like\, a > b.\]

## Conclusion
---

자주 쓰이는 \(O\)-notation과 더불어 다양한 점근 표기법에 대해 살펴보았습니다. 효율성에 중요한 척도가 되는 running time을 케이스와 표기 의도에 따라 적절히 표현하는 데에 많은 도움이 되길 희망합니다!

## Reference
---

Thomas H. Cormen et al., \(\text{Introduction to Algorithms 3rd Edition}\), The MIT Press, 2009, 43-52.