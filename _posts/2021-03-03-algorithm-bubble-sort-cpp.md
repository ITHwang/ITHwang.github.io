---
layout: single
title: "버블 정렬(Bubble sort) with C++"
---

## Description
---
**버블 정렬**(Bubble sort)은 오름차순 기준으로 loop마다 가장 큰 숫자를 찾아 맨 오른쪽부터 정렬하는 알고리즘이다.

N개의 원소일 때 n-1만큼의 outer loop가 돌고, 그 안에서 inner loop가 아직 정렬되지 않은 배열(outer loop마다 오른쪽부터 자리가 정해짐)을 돌며 <strong><u>두 인접한 원소들의 크기를 비교한 후 swap</u></strong>한다.

오름차순 기준으로 loop가 돌면서 비교적 작은 숫자들은 점점 앞으로 가고, 비교적 큰 숫자들은 점점 뒤로 간다. 

아래 그림과 같이 맨 오른쪽부터 최종 위치가 확정되고 있으며, 원소의 이동이 마치 거품이 수면으로 올라오는 것처럼 보여 <strong>"버블"</strong>이라는 수식을 붙였다고 한다.

![bubble-sort-animation](https://github.com/ITHwang/ITHwang.github.io/blob/master/_images/20210303-Bubble_sort_animation.gif?raw=true)

- 시간 복잡도(Time Complexity)

    기본적으로 이중 loop를 돌고 있어 average나 worst의 경우 <strong>*O*(N<sup>2</sup>)</strong>의 복잡도를 가지지만, 모든 원소가 처음부터 순서대로 정렬되어있는 best의 경우 첫 번째 outer loop의 inner loop가 한 번만 돌아서 <strong><i>O</i>(N)</strong>의 복잡도를 가진다.(단, 모든 원소가 순서대로 정렬되어있을 경우 break하는 if 문이 필요하다.)  

- 공간 복잡도(Space Complexity)

    선택 정렬처럼 n 크기의 배열에서 동적으로 추가되는 가변 공간이 없고 자리만 바뀌므로, 상수인 고정 공간만 필요하여 <strong><i>O</i>(1)</strong>의 복잡도를 갖는다.

## Simulation
---
<body>
<code>13 23 11 7 5 15</code>를 오름차순으로 정렬할 경우,
<script src="https://github.com/ITHwang/ITHwang.github.io/blob/master/js/mermaid.min.js"></script>
<script>mermaid.initialize({startOnLoad:true});</script>

- 1st outer loop: 1~6번째 숫자들 중 최댓값을 6번째 자리에 배정
    - inner loop
        <div class="mermaid">
            graph LR;
            A(13)-->B(23);
            B-->C(11);
            C-->D(7);
            D-->E(5);
            E-->F(15);
        </div>

        <div class="mermaid">
            graph LR
            A(13)==Don't swap===B(23)---C(11)---D(7)---E(5)---F(15)
        </div>
        <div class="mermaid">
            graph LR
            A(13)---B(23)==swap===C(11)---D(7)---E(5)---F(15)
        </div>
        <div class="mermaid">
            graph LR
            A(13)---B(11)---C(23)==swap===D(7)---E(5)---F(15)
        </div>
        <div class="mermaid">
            graph LR
            A(13)---B(11)---C(7)---D(23)==swap===E(5)---F(15)
        </div>
        <div class="mermaid">
            graph LR
            A(13)---B(11)---C(7)---D(5)---E(23)==swap===F(15)
        </div>
        <div class="mermaid">
            graph LR
            A(13)---B(11)---C(7)---D(5)---E(15)---F(23)
        </div>
- 2nd outer loop: 1~5번째 숫자들 중 최댓값을 5번째 자리에 배정
    - inner loop
        <div class="mermaid">
            graph LR
            A(13)==swap===B(11)---C(7)---D(5)---E(15)---F(23)
        </div>
        <div class="mermaid">
            graph LR
            A(11)---B(13)==swap===C(7)---D(5)---E(15)---F(23)
        </div>
        <div class="mermaid">
            graph LR
            A(11)---B(7)---C(13)==swap===D(5)---E(15)---F(23)
        </div>
        <div class="mermaid">
            graph LR
            A(11)---B(7)---C(5)---D(13)==Don't swap===E(15)---F(23)
        </div>
        <div class="mermaid">
            graph LR
            A(11)---B(7)---C(5)---D(13)---E(15)---F(23)
        </div>
- 3rd outer loop: 1~4번째 숫자들 중 최댓값을 4번째 자리에 배정
    - inner loop
        <div class="mermaid">
            graph LR
            A(11)==swap===B(7)---C(5)---D(13)---E(15)---F(23)
        </div>
        <div class="mermaid">
            graph LR
            A(7)---B(11)==swap===C(5)---D(13)---E(15)---F(23)
        </div>
        <div class="mermaid">
            graph LR
            A(7)---B(5)---C(11)==Don't swap===D(13)---E(15)---F(23)
        </div>
        <div class="mermaid">
            graph LR
            A(7)---B(5)---C(11)---D(13)---E(15)---F(23)
        </div>
- 4th outer loop: 1~3번째 숫자들 중 최댓값을 3번째 자리에 배정
    - inner loop
        <div class="mermaid">
            graph LR
            A(7)==swap===B(5)---C(11)---D(13)---E(15)---F(23)
        </div>
        <div class="mermaid">
            graph LR
            A(5)---B(7)==Don't swap===C(11)---D(13)---E(15)---F(23)
        </div>
        <div class="mermaid">
            graph LR
            A(5)---B(7)---C(11)---D(13)---E(15)---F(23)
        </div>
- 5th outer loop: 1~2번째 숫자들 중 최댓값을 2번째 자리에 배정
    - inner loop
        <div class="mermaid">
            graph LR
            A(7)==swap===B(5)---C(11)---D(13)---E(15)---F(23)
        </div>
        <div class="mermaid">
            graph LR
            A(5)---B(7)---C(11)---D(13)---E(15)---F(23)
        </div>
</body>

## Pseudo-code
---
```cpp
(오름차순 기준)
for i=1 to n-1:
    for j=1 to n-i:
        if array[j]가 array[j+1]보다 크면,
            swap(array[j], array[j+1]);

    if 해당 outer loop에서 모든 원소가 위 조건절에 해당하지 않으면,
        break;
```

## Code
---
- 입력: 첫 번째 줄에 주어진 n만큼의 자연수들이 공백을 두고 입력된다.

    ex)

        6
        13 23 11 7 5 15

- 출력: 오름차순으로 정렬된 수열을 출력한다.

    ex)

        5 7 11 13 15 23

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main()
{
	ios_base::sync_with_stdio(false);
	cin.tie(0);

    //배열 받기
	int n;
	cin >> n;
	vector<int> v(n + 1);
	for (int i = 1; i <= n; i++) cin >> v[i];
	
	int cnt; //Outer loop마다 스왑되는 횟수
	for (int i = 1; i <= n - 1; i++)
	{
		cnt = 0;
		for (int j = 1; j <= n - i; j++)
		{
			if (v[j] > v[j + 1]) //현재 숫자가 바로 뒤에 있는 숫자보다 크면,
			{
                //swap
				int tmp = v[j];
				v[j] = v[j + 1];
				v[j + 1] = tmp;
				cnt++;
			}
		}
		if (cnt == 0) break; //조기에 배열이 정렬되면(swap이 한 번도 일어나지 않았으면) break
	}

	for (int i = 1; i <= n; i++) cout << v[i] << ' '; //정렬된 배열 출력

	return 0;
}
```

## Reference
- [위키백과: 버블정렬](https://ko.wikipedia.org/wiki/%EA%B1%B0%ED%92%88_%EC%A0%95%EB%A0%AC)
- [Youtube: 버블소트 거품정렬 5분만에 이해하기 - Gunny](https://www.youtube.com/watch?v=RCnyz-Bfkmc&t=192s)
- 인프런: it 취업을 위한 알고리즘 문제풀이(김태원) 34번 문제