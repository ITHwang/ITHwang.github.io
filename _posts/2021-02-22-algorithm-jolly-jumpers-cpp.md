---
layout: single
title: "Jolly Jumpers 문제(C++)"
---

<h2>Description</h2>
<p>N개의 정수로 이루어진 수열에 대해 서로 인접해 있는 두 수의 차가 1에서 N-1까지의 값을 모두 가지면 그 수열을 <strong>유쾌한 점퍼(jolly jumper)</strong>라고 부른다.</p>

<p>예를 들어 다음과 같은 수열에서 <code>1, 4, 2, 3</code> 앞 뒤에 있는 숫자 차의 절대 값이 각각 <code>3 ,2, 1</code>이므로 이 수열은 유쾌한 점퍼가 된다. 어떤 수열이 유쾌한 점퍼인지 판단할 수 있는 프로그램을 작성하라.</p>

<h2>Problem</h2>

<ul>
    <li>입력: 첫 번째 줄에 자연수 N(3<=N<=100)이 주어지고, 그 다음 줄에 N개의 정수가 주어진다. 정수의 크기는 int 형 범위 안에 있다.</li>
    <li>출력: 두 단어가 아나그램이면 <strong>YES</strong>를 출력하고, 아니면 <strong>NO</strong>를 출력한다.</li>
    <li>입력 예제</li>
    <pre><code>5<br>1 4 2 3 7</code></pre>
    <li>출력 예제</li>
    <code>YES</code>
</ul>

<h2>Solution</h2>

<ol type="I">
    <li>N-1만큼 for loop를 돌며 현재 입력 받은 값과 이전에 입력 받은 값의 차(절대값)를 check 배열에 표시한다. 이 때 두 값의 차는 0보다 크고 n보다 작아야 한다.</li>
    <li>For loop를 돌 때마다 이전 입력값을 현재 입력값으로 바꾸어 다음 loop에서의 이전 입력값이 되도록 한다.</li>
    <li>모든 loop에서 두 값의 차가 1~(n-1)에 있고 check 배열에서 중복되지 않으면 "YES"를 출력하고, 그 반대일 경우 "NO"를 출력하고 끝낸다.</li>
</ol>

<h2>Code</h2>

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main()
{
	ios_base::sync_with_stdio(false);
	cin.tie(0);

	int n, pre, now; //pre: 이전 입력값, now: 현재 입력값
	cin >> n;
	vector<int> ch(n); //n개의 정수 배열을 0으로 초기화 

    int res; //현재 입력값과 이전 입력값의 차(절대값) 
    cin >> pre;
	for (int i = 1; i < n; i++)
	{
		cin >> now;
		res = abs(now - pre);
        //두 값의 차가 1~(n-1)에 있고 중복되지 않으면,
		if (res > 0 && res < n && ch[res] == 0) ch[res]++;
		else
		{
			cout << "NO";
			return 0;
		}
		pre = now; //현재 입력값을 다음 loop의 이전 입력값으로 갱신
	}
	
	cout << "YES";
	return 0;
}
```

<h2>Reference</h2>
- 인프런: it 취업을 위한 알고리즘 문제풀이(김태원) 24번 문제