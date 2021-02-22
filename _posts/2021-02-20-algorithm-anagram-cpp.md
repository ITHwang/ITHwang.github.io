---
layout: single
title: "Anagram(아나그램) 문제(C++)"
---

## Description
알파벳의 나열 순서는 다르지만 그 구성이 일치하는 두 단어를 **Anagram**(아나그램)이라고 한다. 

예를 들어, `AbaAeCe`와 `baeeACA`는 알파벳의 나열 순서가 다르나, 그 구성을 살펴보면 A(2), a(1), b(1), C(1), e(2)로 모두 일치한다. 

즉, 한 단어의 구성을 재배열하면 다른 한 단어가 될 수 있는 것이 아나그램이다.

## Problem
- 입력: 첫 줄에 첫 번째 단어가 입력되고, 두 번째 줄에 두 번째 단어가 입력된다. 이 때, 단어의 길이는 **100**을 넘지 않는다.

- 출력: 두 단어가 아나그램이면 **YES**를 출력하고, 아니면 **NO**를 출력한다.

- 입력 예제
```
AbaAeCe
baeeACA
```
- 출력 예제 
```
YES
```

## Solution
1. 두 단어의 알파벳 개수를 저장할, ASCII table 크기(256)의 정수 배열 두 개를 선언한다.
2. 입력 받은 두 단어를 한 문자씩 읽으면서 읽은 문자에 해당하는 아스키 코드의 개수를 하나씩 더한다.
3. 각 아스키 문자의 개수가 저장된 두 정수 배열을 비교하여 모든 문자의 개수가 같으면 YES를 출력하고, 아니면 NO를 출력한다.

## Code

```cpp
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
using namespace std;

char str[101]; //입력 버퍼
int a[256], b[256]; //알파벳 개수를 저장할 정수 배열

int main()
{
	ios_base::sync_with_stdio(false);
	cin.tie(0);
	
	cin >> str;
	for (int i = 0; str[i] != '\0'; i++) a[str[i]]++; // 첫 번째 단어 내 알파벳 개수 세기
	cin >> str;
	for (int i = 0; str[i] != '\0'; i++) b[str[i]]++; // 두 번째 단어 내 알파벳 개수 세기

	for (int i = 0; i < 256; i++)
	{
		if (a[i] != b[i]) // 해당 아스키 문자의 개수가 다르면
		{
			cout << "NO";
			return 0;
		}
	}
	cout << "YES";
	return 0;
}
```

## Reference
- 인프런: it 취업을 위한 알고리즘 문제풀이(김태원) 16번 문제
