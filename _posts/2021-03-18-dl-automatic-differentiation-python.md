---
layout: single
title: "자동 미분(feat. 웬거트 리스트)"
use_math: true
---

## Introduction
---

현대 딥러닝 프레임워크 내에서는 유저가 기울기를 직접 계산하면서 역전파를 구현하지 않는다. 자동으로 기울기를 이끌어내는 뉴럴 네트워크 연산을 지원하기 때문이다. 즉 \(loss function\,L\)부터 그 아래 편미분들까지 자동으로 미분하는 것을 **자동 미분**\(Automatic\,Differentiation;\,AD\)이라고 하며, 이를 구현하기 위해 **웬거트 리스트**\(wengert\,list\)를 이용한다. 따라서 본문에서는 웬거트 리스트를 이용해 자동 미분을 구현해보고 자동 미분의 장점에 대해 다룬다.

## Wengert list
---

- Definition

    구글링해도 정의가 나오지 않아 당황했다... 다만 스택오버플로우에서 비슷한 주제로 나눈 QnA를 찾았고, <u>"웬거트 리스트란 연산들이 이루어지는 순서를 기록하는 <strong>녹음 테이프</strong>다"</u>라는 답변이 있었다.
    
    쉽게 말해 연산 순서, 각 연산의 input과 output 등을 기억하여 역전파에 활용하겠다는 것이다. 제대로 된 정의와 설명은 \(\text{Wengert, Robert Edwin. "A simple automatic derivative evaluation program." Communications of the ACM 7.8 (1964): 463-464.}\)에서 찾아볼 수 있다. ~~1960년대 논문이라 열람하기도 힘들고 부담스러웠다...~~

- Example

    예를 들어 아래와 같은 함수 \(f\)가 있을 때,

    \[f(x_1,x_2) = (x_1 + x_1 + x_2)^2 = (2x_1 + x_2)^2\tag{1}\]

    덧셈 두 번과 제곱 한 번으로 총 세 연산이 이루어지며, output도 세 개다.

    \[z_1 = add(x_1,x_1)\tag{2}\]
    \[z_2 = add(z_1,x_2)\tag{3}\]
    \[f = square(z_2)\tag{4}\]


    첫 번째 식을 보면 **변수** \(x_1\) 두 개와 **연산자** \(add\)에 의해 새로운 **변수** \(z_1\)가 생겼고, 두 번째 식에서 또 다른 **변수** \(x_2\)와 \(z_1\)가 **연산자** \(add\)에 들어가서 또 새로운 **변수** \(z_2\)가 생겼다. 

    변수 입장에서 연산자는 자신을 만들어주는 **창조자**일 것이며, 연산자 입장에서 변수는 **입출력 값**일 것이다. 이와 같이 여러 연산이 연결되어 있는 합성함수 속 변수들과 연산자들의 관계를 중심으로 연결하는 리스트가 **웬거트 리스트**다. 

## Implementation of AD in python
---

\(Automatic\,Differentiation(AD)\)은 두 단계로 진행된다. 첫째로 함수 \(f(x_1,\ldots,x_k)\)를 웬거트 리스트로 저장하며, 둘째로 웬거트 리스트를 이용하여 함수와 그 기울기를 계산한다.

1. 웬거트 리스트로 변환 

    식 2, 3, 4를 튜플로 \((z, g, (y_1,\dots,y_k))\)와 같이 나타낼 수 있다. \(y_1,\ldots,y_k\)는 입력값, \(g\)는 연산자, 그리고 \(z\)는 출력값이다.

    ```py
    Equation_1 = [("z1", "add", ("x1", "x1")),
                  ("z2", "add", ("z1", "x2")),
                  ("f", "square", ("z2",))]
    ```

    연산자 \(g\)는 정의와 함께 딕셔너리로 저장한다.

    ```py
    G = {"add" : lambda a,b: a+b,
         "square" : lambda a : a*a}
    ```

    역전파 계산을 위해 미분 연산자도 저장한다. \(add\)의 미분 값은 1이고, \(square\)의 미분 값은 2*input이므로,

    ```py
    DG = {"add" : [(lambda a, b: 1), (lambda a, b: 1)],
          "square": [lambda a: 2*a]}
    ```

2. 웬거트 리스트를 이용하여 연산

    - 웬거트 리스트 생성
        
        튜플로 변환된 **웬거트 리스트**와 딕셔너리로 변환된 **연산자 및 미분 연산자**로 클래스를 생성한다.

    ```py
    class Wengert_list:
        def __init__(self, equation, operator, d_operator): #웬거트 리스트, 연산자, 미분 연산자
            self.equation = equation
            self.operator = operator
            self.d_operator = d_operator
            self.value_list = {} #각 연산의 결과값 저장
            self.delta_list = {} #각 미분 연산의 결과값 저장
            self.output_name = None #연산 최종값 저장
    ```

    - 순전파

        입력값으로 웬거트 리스트에 따라 연산하면서 결과값을 저장한다.

    ```py
    def eval(self, x1, x2):
        #input 저장
        self.value_list["x1"] = x1
        self.value_list["x2"] = x2

        #순서대로 연산
        for eq in self.equation:
            op = self.operator[eq[1]] #연산자 정의 참조
            args = tuple(map(lambda a : self.value_list[a], eq[2])) #변수의 값 참조

            #연산 후 저장
            if len(args) == 1: #square
                self.value_list[eq[0]] = op(args[0])
            elif len(args) == 2: #add
                self.value_list[eq[0]] = op(args[0], args[1])

        self.output_name = self.equation[-1][0] #최종값 저장

        return self.value_list[self.output_name]
    ```

    - 역전파

        최종값 \(f\)부터 순전파의 역순으로 미분 연산하면서 결과값을 저장한다.
    
    ```py
    def backprop(self):
        self.delta_list[self.output_name] = 1 #최종값 f에 대하여 df/df = 1

        for eq in reversed(self.equation):
            args = tuple(map(lambda a : self.value_list[a], eq[2])) #변수의 값 참조

            for i in range(len(eq[2])): #미분 연산: 입력 변수 개수만큼 연산한다.
                op = self.d_operator[eq[1]][i] #연산자 정의 참조
                arg_name = eq[2][i] #입력 변수명 참조

                if arg_name not in self.delta_list: 
                    self.delta_list[arg_name] = 0
                
                #d(최종값)/d(입력변수 값) 갱신
                if eq[1] == "square":
                    self.delta_list[arg_name] = \
                    self.delta_list[arg_name] + self.delta_list[eq[0]] * op(args[0])
                elif eq[1] == "add":
                    self.delta_list[arg_name] = \
                    self.delta_list[arg_name] + self.delta_list[eq[0]] * op(args[0], args[1])
    ```

    - 그래프

        <div class="mermaid">
            graph LR
            A[x1]-- x1 -->B((+))-- z1 -->C((+))-- z2 -->D((^2))-- f -->E[f]
            A-- x1 -->B
            F[x2]-- x2 -->C
            E-. df/df .->D -. df/dz2 .->C-. df/dz1 .->B-. df/dx1 .->A
            C-. df/dx2 .->F
            B-. df/dx1 .->A
        </div>

3. Example

    ```py
    w = Wengert_list(Equation_1, G, DG)
    w.eval(3, 7) #169

    w.backprop()
    print(w.delta_list) #{'f': 1, 'z2': 26, 'z1': 26, 'x2': 26, 'x1': 52}
    ```

    <div class="mermaid">
        graph LR
        A[3]-- 3 -->B((+))-- 6 -->C((+))-- 13 -->D((^2))-- 169 -->E[169]
        A-- 3 -->B
        F[7]-- 7 -->C
        E-. 1 .->D -. 26 .->C-. 26 .->B-. 26 .->A
        C-. 26 .->F
        B-. 26 .->A
    </div>

## Advantage of AD
---

**미분을 알아서 해준다!** 이는 chain rule을 지키며 일일이 손 아프게 코딩해야 했던 기존 미분과 뚜렷이 구분되는 점이다.

아래와 같은 연산 리스트가 있을 때,

\[z_1 = f_1(z_0)\]
\[z_2 = f_2(z_1)\]
\[z_3 = f_3(z_2)\]
\[\cdots\]
\[z_m = f_m(z_{m-1}) \tag{5}\]

기존 미분으로 \(\frac{dz_m}{dz_0}\)을 찾기 위해 chain rule을 적용하면,

\[\frac{dz_m}{dz_0} = \frac{dz_m}{dz_{m-1}}\frac{dz_{m-1}}{dz_{m-2}}\cdots\frac{z_2}{z_1}\frac{z_1}{z_0} \tag{6}\]

이며, 런타임 이전에 이를 `backward()`에 수동적으로 코딩해야 한다. 

이러한 기존 방식을 **static programming**이라고 부르며, 변수로 미리 미분 식을 만들어 놓아서 **symbolic differentiation**이라 부르기도 한다. 즉 input이 아직 들어가지 않아 순전파도 진행되지 않은 상태에서 역전파 준비까지 하는 것이다.

이는 input과 계산 그래프의 개수가 늘어날 때마다 chain rule에 의한 역전파 코드를 새로 짜야 하기 때문에 번거롭고 비효율적이다. 

자동 미분은 단순 미분 식이 아닌 <u>미분을 계산하기 위한 알고리즘</u>이라 할 수 있다.

식 6에서,

\[\frac{dz_m}{dz_0} = \frac{dz_m}{dz_{m-1}}\frac{dz_{m-1}}{dz_0} \tag{7}\]

을 도출할 수 있으며, 식을 단순화 하기 위해 \(h_{i,j}\)을 \(\frac{dz_i}{dz_j}\)이라 하고, \(z_0\)에서의 \(\frac{dz_i}{dz_j}\) 값을 \(h_{i,j}(z_0)\)라고 정의하면 다음과 같다.

\[h_{m,0}(z_0) = f' _m(z_m)h_{m-1,0}(z_0)\]
\[h_{m,0}(z_0) = f' _m(z_m)f' _{m-1}(z_{m-1})h_{m-2,0}(z_0)\]
\[h_{m,0}(z_0) = f' _m(z_m)f' _{m-1}(z_{m-1})f' _{m-2}(z_{m-2})h_{m-3,0}(z_0)\]
\[\cdots\]
\[h_{m,0}(z_0) = f' _m(z_m)f' _{m-1}(z_{m-1}) \cdots f' _2(z_2)h_{1,0}(z_0)\]
\[h_{m,0}(z_0) = f' _m(z_m)f' _{m-1}(z_{m-1}) \cdots f' _2(z_2)f' _1(z_1) \tag{8}\]

우리가 자동 미분으로 `backward()`를 실행하여 \(h_{m,0}(z_0)\)을 구하고자 한다면, 식 8에서의 연산 순서는 다음과 같다.

\[h_{m,0}(z_0) = (((f' _m(z_m)f' _{m-1}(z_{m-1}))f' _{m-2}(z_{m-2})) \cdots f' _2(z_2))f' _1(z_1) \tag{9}\]

즉 앞 순서에서 연산된 미분값을 활용하여 각 파라미터에 대한 출력의 미분값을 손쉽게 구할 수 있다. 예를 들어 \(h_{m,10}\)을 구할 때 앞서 연산된 \(h_{m,11}\)에다가 \(\frac{dz_{11}}{dz_{10}}\)만 계산해서 곱해주면 되는 것이다. 이때 \(h_{m,11}\)는 런타임에 동적으로 정의된 것이므로 자동 미분을 **동적 계산 그래프** 또는 **Define-by-run**이라고 부르기도 한다. Dynamic Programming과 매우 유사하다! \(\frac{dz_{11}}{dz_{10}}\)와 같이 그때마다 계산하는 미분은 각 함수의 `backward()`에 해당하므로 for loop이나 recursive programming으로 짜주면 자동으로 미분할 수 있다.

## Conclusion
---
자동 미분은 Pytorch, Tensorflow2와 같은 현대 딥러닝 프레임워크에서 적극적으로 활용되고 있다. 특히 Pytorch에서는 중간 결과마다 계산 그래프 내 <u>연산과 관련이 있는 부분만 저장</u>하여, 독립적으로 계산 그래프들이 자유롭게 섞이거나 병합될 수 있도록 했다. 이는 하나의 그래프가 끝났을 때 자동으로 메모리가 해제되어 메모리 최적화에도 수월하다고 한다.

## Reference
---

- [Where is Wengert List in TensorFlow? - stackoverflow](https://stackoverflow.com/questions/43863111/where-is-wengert-list-in-tensorflow)
- [William W. Cohen, "Automatic Reverse-Mode Differentiation: Lecture Notes"](http://www.cs.cmu.edu/~wcohen/10-605/notes/autodiff.pdf)
- [Paszke, Adam, et al. "Automatic differentiation in torch"](https://openreview.net/pdf?id=BJJsrmfCZ)
- 사이토 고키, 『밑바닥부터 시작하는 딥러닝3』, 개앞맵시, 한빛미디어, 2020, 73~79쪽