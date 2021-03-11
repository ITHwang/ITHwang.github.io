---
layout: single
title: "LRU 알고리즘(Least Recently Used Algorithm) with C++"
---

## Description
---

**LRU 알고리즘**(Least Recently Used Algorithm)은 memory를 paging으로 최적화할 때 사용하는 [page replacement algorithm](https://github.com/ITHwang/ITHwang.github.io/blob/master/_posts/2021-03-08-algorithm-page-replacement-algorithm.md) 중 하나다. page fault 및 page replacement가 발생했을 때, 메모리 최적화를 위해 말 그대로 <u>가장 오랫동안 참조되지 않은</u> 메모리를 disk로 swap out한다.

이 idea는 page replacement뿐만 아니라 용량 최적화가 필요한 케이스들에서 두루 활용된다.

## 응용 문제
---

![lru-1](https://github.com/ITHwang/ITHwang.github.io/blob/master/_images/20210307-lru-1.png?raw=true)

- Input: 첫 번째 줄에 캐시 크기와 작업 개수가 입력되고, 두 번째 줄에 작업 번호가 작업 개수만큼 주어진다.

    ex)

        5 9
        1 2 3 2 6 2 3 5 7

- Output: 마지막 작업 후 캐시 메모리의 상태를 가장 최근 사용된 작업부터 차례로 출력한다.

    ex)
    
        7 5 3 2 6

- Code

    ```cpp
    #include <iostream>
    #include <vector>
    using namespace std;

    int main()
    {
        ios_base::sync_with_stdio(false);
        cin.tie(0);

        int s, n; //캐시 메모리 크기, 작업 개수
        cin >> s >> n;
        vector<int> v(s);

        int tmp, pos; //작업할 번호, 작업할 번호가 캐시에 있을 때 캐시 내 위치 
        for (int i = 0; i < n; i++)
        {
            cin >> tmp;
            pos = -1;

            for (int j = 0; j < s; j++) //작업할 번호가 캐시에 있는지 확인, 있으면 위치를 pos에 copy
                if (tmp == v[j])
                {
                    pos = j;
                    break;
                }

            if (pos == -1) //작업할 번호가 캐시에 없으면, 모두 오른쪽으로 한 칸씩 옮기고 맨 왼쪽에 삽입
                for (int i = s - 2; i >= 0; i--) v[i + 1] = v[i];
            else //작업할 번호가 캐시에 있으면, 그 작업 번호의 위치부터 맨 앞까지 한 칸씩 옮기고 맨 왼쪽에 삽입 
                for (int i = pos - 1; i >= 0; i--) v[i + 1] = v[i];
            v[0] = tmp;
        }

        for (int i = 0; i < s; i++) cout << v[i] << ' '; //출력

        return 0;
    }
    ```

## Reference
---

- 인프런: it 취업을 위한 알고리즘 문제풀이(김태원) 37번 문제