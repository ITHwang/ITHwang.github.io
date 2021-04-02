---
layout: single
title: "삽입 정렬(Insertion sort) with C++"
toc: true
toc_sticky: true
tags:
	- sort algorithm
    - cpp
categories:
    - Algorithm
---

## Description
---

**삽입 정렬**(Insertion sort)은 왼쪽부터 정렬된 partial array를 유지하며 오른쪽으로 한 칸씩 partial array를 늘려나가는 알고리즘이다.

매번 오른쪽에서 추가되는 하나의 원소를 partial array에 순서에 맞도록 삽입하게 된다.

따라서 선택 정렬이나 버블 정렬과 달리, outer loop마다 원소의 최종 위치가 정해지진 않고 array에 원소가 하나씩 더해진다.

아래 그림과 같이 왼쪽부터 하나씩 차례대로 정렬되는 모습을 볼 수 있다.(x축은 인덱스, y축은 크기)

![insertion-sort-animation](https://github.com/ITHwang/ITHwang.github.io/blob/master/_images/20210306-insertion-sort.gif?raw=true)

- 시간 복잡도(Time Complexity)
    - Best(모든 원소가 정렬되어 있는 경우): <strong><i>O</i>(N)</strong>
        > 추가되는 원소 e가 항상 partial array에서 가장 크기 때문에, 자리를 만들기 위해 e보다 큰 원소들을 한 칸씩 오른쪽으로 옮기는 inner loop가 수행되지 않는다.
        > 단, e가 partial array의 최댓값보다 클 경우 inner loop를 빠져나가는 조건문이 필요하다. 
    - Average & Worst: <strong>*O*(N<sup>2</sup>)</strong>
        > Best case를 제외하곤 원소 개수만큼 이중 for 문을 돈다.

- 공간 복잡도(Space Complexity): <strong><i>O</i>(1)</strong>
    > 선택 정렬이나 버블 정렬처럼 가변 공간이 발생하지 않고, 원소 개수만큼의 고정 공간만 필요하다.

## Simulation
---

`11 7 5 6 10 9`를 오름차순으로 정렬할 경우,

- 1st outer loop: 1 ~ 2번째까지 정렬
    - inner loop
        <div class="mermaid">
            graph LR
            a1(11)===b1(empty)===c1(5)===d1(6)===e1(10)===f1(9)
            subgraph pop
            b1-..->g1[(7)]
            end
        </div>
        <div class="mermaid">
            graph LR
            a2(11)-.copy.->b2(empty)===c2(5)===d2(6)===e2(10)===f2(9)
        </div>
        <div class="mermaid">
            graph LR
            a3(11)===b3(11)===c3(5)===d3(6)===e3(10)===f3(9)
            subgraph insert
            g2[(7)]-..->a3
            end
        </div>
        <div class="mermaid">
            graph LR
            a4(7)===b4(11)===c4(5)===d4(6)===e4(10)===f4(9)
        </div>
- 2nd outer loop: 1 ~ 3번째까지 정렬
    - inner loop
        <div class="mermaid">
            graph LR
            a1(7)===b1(11)===c1(empty)===d1(6)===e1(10)===f1(9)
            subgraph pop
            c1-..->g1[(5)]
            end
        </div>
        <div class="mermaid">
            graph LR
            a2(7)===b2(11)-.copy.->c2(empty)===d2(6)===e2(10)===f2(9)
        </div>
        <div class="mermaid">
            graph LR
            a3(7)-.copy.->b3(11)===c3(11)===d3(6)===e3(10)===f3(9)
        </div>
        <div class="mermaid">
            graph LR
            a4(7)===b4(7)===c4(11)===d4(6)===e4(10)===f4(9)
            subgraph insert
            g2[(5)]-..->a4
            end
        </div>
        <div class="mermaid">
            graph LR
            a5(5)===b5(7)===c5(11)===d5(6)===e5(10)===f5(9)
        </div>
- 3rd outer loop: 1 ~ 4번째까지 정렬
    - inner loop
        <div class="mermaid">
            graph LR
            a1(5)===b1(7)===c1(11)===d1(empty)===e1(10)===f1(9)
            subgraph pop
            d1-..->g1[(6)]
            end
        </div>
        <div class="mermaid">
            graph LR
            a2(5)===b2(7)===c2(11)-.copy.->d2(empty)===e2(10)===f2(9)
        </div>
        <div class="mermaid">
            graph LR
            a3(5)===b3(7)-.copy.->c3(11)===d3(11)===e3(10)===f3(9)
        </div>
        <div class="mermaid">
            graph LR
            a4(5)===b4(7)===c4(7)===d4(11)===e4(10)===f4(9)
            subgraph insert
            g2[(6)]-..->b4
            end
        </div>
        <div class="mermaid">
            graph LR
            a5(5)===b5(6)===c5(7)===d5(11)===e5(10)===f5(9)
        </div>
- 4th outer loop: 1 ~ 5번째까지 정렬
    - inner loop
        <div class="mermaid">
            graph LR
            a1(5)===b1(6)===c1(7)===d1(11)===e1(empty)===f1(9)
            subgraph pop
            e1-..->g1[(10)]
            end
        </div>
        <div class="mermaid">
            graph LR
            a2(5)===b2(6)===c2(7)===d2(11)-.copy.->e2(empty)===f2(9)
        </div>
        <div class="mermaid">
            graph LR
            a3(5)===b3(6)===c3(7)===d3(11)===e3(11)===f3(9)
            subgraph insert
            g2[(10)]-..->d3
            end
        </div>
        <div class="mermaid">
            graph LR
            a4(5)===b4(6)===c4(7)===d4(10)===e4(11)===f4(9)
        </div>
- 5th outer loop: 1 ~ 6번째(마지막)까지 정렬
    - inner loop
        <div class="mermaid">
            graph LR
            a1(5)===b1(6)===c1(7)===d1(10)===e1(11)===f1(empty)
            subgraph pop
            f1-..->g1[(9)]
            end
        </div>
        <div class="mermaid">
            graph LR
            a2(5)===b2(6)===c2(7)===d2(10)===e2(11)-.copy.->f2(empty)
        </div>
        <div class="mermaid">
            graph LR
            a3(5)===b3(6)===c3(7)===d3(10)-.copy.->e3(11)===f3(11)
        </div>
        <div class="mermaid">
            graph LR
            a4(5)===b4(6)===c4(7)===d4(10)===e4(10)===f4(11)
            subgraph insert
            g2[(9)]-..->d4
            end
        </div>
        <div class="mermaid">
            graph LR
            a5(5)===b5(6)===c5(7)===d5(9)===e5(10)===f5(11)
        </div>

## Code
---

- Input: 첫 번째 줄에 주어진 n만큼의 자연수들이 공백을 두고 입력된다.

    ex)

        6
        11 7 5 6 10 9

- Output: 오름차순으로 정렬된 수열을 출력한다.

    ex)

        5 6 7 9 10 11

- Solution

    ```cpp
    #include <iostream>
    #include <vector>
    using namespace std;

    int main()
    {
        ios_base::sync_with_stdio(false);
        cin.tie(0);

        //input
        int n;
        cin >> n;
        vector<int> v(n + 1);
        for (int i = 1; i <= n; i++) cin >> v[i];

        int tmp; //삽입할 숫자
        int j; //inner iter
        for (int i = 2; i <= n; i++) //1st랑 2nd부터 정렬
        {
            tmp = v[i]; //빼놓기
            j = i - 1;

            //tmp 이전 숫자들 중에 tmp보다 같거나 작은 숫자 v[j]를 만나거나,
            //tmp가 가장 작으면 break
            while (j >= 1 && v[j] > tmp)
            {
                v[j + 1] = v[j];
                j--;
            }

            //v[1]이나 v[j] 뒤에 삽입
            v[j + 1] = tmp;
        }

        //output
        for (int i = 1; i <= n; i++) cout << v[i] << ' ';

        return 0;
    }

    //while (j >= 1 && v[j] >= tmp)도 가능함. 다만, tmp와 같은 숫자 배열 앞에 tmp가 삽입됨.
    ```

## Reference
---

- [위키백과: 삽입 정렬](https://ko.wikipedia.org/wiki/%EC%82%BD%EC%9E%85_%EC%A0%95%EB%A0%AC)
- [Youtube: 인설션소트 삽입정렬 5분만에 이해하기 - Gunny](https://www.youtube.com/watch?v=iqf96rVQ8fY)
- 인프런: it 취업을 위한 알고리즘 문제풀이(김태원) 36번 문제