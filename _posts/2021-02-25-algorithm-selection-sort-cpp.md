---
layout: single
title: "선택 정렬(Selection sort) with C++"
---

## Description
---
**선택 정렬**(Selection sort)은 오름차순 기준으로 loop마다 가장 작은 숫자를 찾아 가장 왼쪽 index부터 swap하는 정렬 알고리즘이다.

Loop마다 n개의 element의 모두 확인하면서 최솟값을 찾기 때문에, 아래 그림과 같이 왼쪽부터 하나씩 최종 위치가 확정된다.

![selection-sort-animation](../_images/20210225-selection-sort-animation.gif)

- 시간 복잡도(Time Complexity)

    왼쪽부터 하나씩 자리를 배정하는 outer loop 안에 모든 숫자를 확인하며 최솟값을 찾는 inner loop가 있기 때문에, 모든 case에서 <strong>*O*(N<sup>2</sup>)</strong>의 복잡도를 갖는다.

- 공간 복잡도(Space Complexity)
    n 크기의 배열에서 동적으로 추가되는 공간이 없고 자리만 바뀌므로, 상수인 고정 공간만 필요하여 <strong>*O*(1)</strong>의 복잡도를 갖는다.

## Example
---
`[6, 4, 3, 2]`의 나열이 있을 때,
- Iter 1: **첫 번째 자리**에 배치할 숫자 결정

    최솟값 2이 있는 네 번째 자리와 첫 번째 자리를 바꾼다.

    <code>[<strong>2</strong>, 4, 3, <strong>6</strong>]</code>

- Iter 2: **두 번째 자리**에 배치할 숫자 결정

    정렬된 첫 번째 자리의 숫자를 제외한 나머지 숫자 중 최솟값 3이 있는 세 번째 자리와 두 번째 자리를 바꾼다. 

    <code>[2, <strong>3</strong>, <strong>4</strong>, 6]</code>

- Iter 3: **세 번째 자리**에 배치할 숫자 결정

    정렬된 숫자들(2, 3)을 제외한 나머지 숫자 중 최솟값 4가 있는 세 번째 자리와 세 번째 자리를 바꾼다. 바꾸려는 자리가 같으므로 그대로 둔다.

- Iter 4: **네 번째 자리**에 배치할 숫자 결정

    정렬된 숫자들(2, 3, 4)을 제외한 나머지 숫자 중 최솟값 6이 있는 네 번째 자리와 네 번째 자리를 바꾼다. 바꾸려는 자리가 같으므로 그대로 둔다.

- 결과 

    `[2, 3, 4, 6]`


## Pseudo-code
---
```
(오름차순 기준)
for i=1 to n:
    array[i+1]와 array[n] 사이에서 가장 작은 array[j]가 있다면,
    array[i]와 array[j]를 swap한다.
```

## Problem: 3등의 성적은?
---
    N명의 수학 성적이 주어지면 그 중 3등인 수학 성적을 출력하시오.
    
    만약 학생들의 점수가 100점이 3명, 99점이 2명, 98점이 5명, 97점이 3명일 경우, 98점이 3등인 점수다.

- 입력

    첫 번째 줄에 자연수 N(1<=N<=100)이 주어진다.

    두 번째 줄에 N개의 수학 성적 점수가 공백을 사이에 두고 입력된다. 수학 성적 점수는 100점 만점 기준으로 입력된다.

    Ex)

        7
        80 96 75 82 96 92 100

- 출력

    3등인 점수를 출력한다.

    Ex)

        92

## Solution
---

1. 점수에 등수를 매겨야 하기에 내림차순을 적용한다. 즉, 왼쪽부터 최댓값을 찾아 정렬한다.
2. n 크기의 배열에 주어진 수학 점수들을 담는다.
3. Outer loop를 돌 때마다 최댓값이 있는 idx를 찾는다.
4. 이 idx와 해당 outer loop에서 배정하려는 index의 value를 바꾼다.
5. 정렬된 배열을 loop로 돌면서 세 번째로 큰 점수를 찾아 출력한다.

## Code
---

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main()
{
	ios_base::sync_with_stdio(false);
	cin.tie(0);

	int n;
	cin >> n;
	vector<int> v(n + 1);
	for (int i = 1; i <= n; i++) cin >> v[i]; //배열에 주어진 점수들을 담는다.

	for (int i = 1; i <= n; i++)
	{
		int idx = i; //해당 outer loop의 최댓값

		for (int j = i + 1; j <= n; j++)
			if (v[idx] < v[j]) idx = j; //당시 최댓값보다 큰 숫자가 나오면 최댓값 갱신

        //자리 바꾸기
		int tmp = v[i];
		v[i] = v[idx];
		v[idx] = tmp;
	}

	int cnt = 1;
	for (int i = 2; i <= n; i++)
	{
		if (v[i] < v[i - 1]) cnt++; //왼쪽부터 하나씩 볼 때마다 값이 내려가면 count
		if (cnt == 3) //세 번째로 큰 점수가 나오면 출력
		{
			cout << v[i];
			break;
		}
	}

	return 0;
}
```

## Reference
- [위키백과: 선택정렬](https://ko.wikipedia.org/wiki/%EC%84%A0%ED%83%9D_%EC%A0%95%EB%A0%AC)
- [유튜브: 셀렉션소트 선택정렬 5분만에 이해하기 - Gunny](https://www.youtube.com/watch?v=jtxwQ7ChiII)
- 인프런: it 취업을 위한 알고리즘 문제풀이(김태원) 33번 문제