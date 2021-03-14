---
layout: single
title: "중앙차분이 가장 정확한 이유\\text{(}테일러 급수로 증명\\text{)}"
use_math: true
---

## Introduction
---

**수치 미분**\(Numerical Differentiation)에는 **전진차분**(Forward Difference), **후진차분**(Backward Difference) 그리고 **중앙차분**(Central Difference)이 있다. 그 중 중앙차분의 오차가 가장 작다는 것을 각 미분법을 <u>테일러 급수로 근사화</u>하여 증명한다.

## 테일러 급수
---

**테일러 급수**(Taylor series)는 난해한 함수를 연산하기 쉽게 <u>특정 지점에서 다항식으로 근사화</u>하는 방법이다.

특정 지점 \(a\)에서 \(f(x)\)를 다항식으로 다음과 같이 근사화 할 수 있다.

\[f(x) = f(a) + f'(a)(x-a) + \frac{1}{2!}f'' (a)(x-a)^2 + \frac{1}{3!}f''' (a)(x-a)^3 + \cdots = \sum_{k=0}^{\infty} {\frac{f^{(k)}(a)}{k!}(x-a)^k}.\tag{1}\]

무한히 연산할 수는 없을 터, n까지 절단했을 때, 

\[f(x) = \sum_{k=0}^{n} {\frac{f^{(k)}(a)}{k!}(x-a)^k} + R_{n+1}(x)\]
\[, where\,R_{n+1}(x) = \frac{f^{n+1}(b)}{(n+1)!}(x-a)^{n+1}.\tag{2}\]

n 이후 나머지 항인 \(R_{n+1}(x)\)을 절단오차라고 하며, 이 때 b는 \([a, x]\) 또는 \([x, a]\)에 속하는 적당한 실수다.

## 전진차분 근사화
---

전진차분으로 미분하면, 

\[f'(x) = \lim_{h \to 0} {\frac{f(x+h) - f(x)}{h}}.\tag{3}\]

식 3에서 \(f(x+h)\)을 점 \(x\)에 대해서 제곱까지 테일러 급수로 근사화하면,

\[f(x+h) = f(x) + hf'(x) + \frac{h^2}{2}f''(c)\\,\,where\,c\,is\,some\,number\,between\,x\,and\,x+h.\tag{4}\]

식 4를 \(f(x)\)에 대해서 정리하면,

\[f'(x) = \frac{f(x+h) - f(x)}{h} - \frac{h}{2}f''(c).\tag{5}\]

즉, 식 3을 식 5로 근사화 할 수 있으며, \(O(h)\)만큼의 오차를 가진다.

## 후진차분 근사화
---

후진차분으로 미분하면, 

\[f'(x) = \lim_{h \to 0} {\frac{f(x) - f(x-h)}{h}}.\tag{6}\]

식 6에서 \(f(x-h)\)을 점 \(x\)에 대해서 제곱까지 테일러 급수로 근사화하면,

\[f(x-h) = f(x) - hf'(x) + \frac{h^2}{2}f''(c)\\,\,where\,c\,is\,some\,number\,between\,x-h\,and\,x.\tag{7}\]

식 7를 \(f(x)\)에 대해서 정리하면,

\[f'(x) = \frac{f(x) - f(x-h)}{h} + \frac{h}{2}f''(c).\tag{8}\]

즉, 식 6을 식 8로 근사화 할 수 있으며, 이것도 \(O(h)\)만큼의 오차를 가진다.

## 중앙차분 근사화
---

중앙차분으로 미분하면,

\[f'(x) = \lim_{h \to 0} {\frac{f(x+h) - f(x-h)}{2h}}.\tag{9}\]

식 9에서 \(f(x-h)\)와 \(f(x+h)\)을 점 \(x\)에 대해서 오차를 찾기 위해 세 제곱까지 테일러 급수로 근사화하면,

\[f(x+h) = f(x) + hf'(x) + \frac{h^2}{2}f''(x) + \frac{h^3}{3!}f'''(c_1)\,and\\
f(x-h) = f(x) - hf'(x) + \frac{h^2}{2}f''(x) + \frac{h^3}{3!}f'''(c_2),\\
where\,x \le c_1 \le (x+h)\,and\,(x-h) \le c_2 \le x.\tag{10}\]

식 10에서 \(f(x+h) - f(x-h)\)을 한 후 정리하면,

\[f'(x) = \lim_{h \to 0} {\frac{f(x+h) - f(x-h)}{2h}} - \frac{h^2}{3!}\frac{f'''(c_1) + f'''(c_2)}{2}.\tag{11}\]

즉, 식 9을 식 11로 근사화 할 수 있으며, 이는 **전진차분과 후진차분과 달리 \(O(h^2)\)만큼의 오차를 가지므로 가장 오차가 작음**을 알 수 있다.(\(h\)는 \(x_i\)와 \(x_{i+1}\)의 차로서 충분히 작은 수다.)

## Conclusion
---

따라서 보통 수치 미분을 구현할 때 중앙차분을 이용하여 기울기를 구한다. 물론 알고리즘에는 오차가 아예 없는 **해석 미분**(Analytical Differentiation)을 이용하나, 복잡해질 수록 버그가 발생할 확률이 높으므로 알고리즘의 결과값을 중앙차분으로 검산할 수 있다.

## Reference
---

- 밑바닥부터 시작하는 딥러닝3(사이토 고키 지음; 개앞맵시 옮김) step 4, step 27
- [위키백과: 테일러 급수](https://ko.wikipedia.org/wiki/%ED%85%8C%EC%9D%BC%EB%9F%AC_%EA%B8%89%EC%88%98)
- Todd Young, Martin J. Mohlenkamp. "Introduction to Numerical Methods and Matlab Programming for Engineers &#8211; Lecture 27: Numerical Differentiation." http://www.ohiouniversityfaculty.com/youngt/IntNumMeth/lecture27.pdf (2019.11)