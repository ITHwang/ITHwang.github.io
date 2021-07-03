---
layout: single
title: "이진 검색 - Java & JavaScript & Python"
use_math: true
toc: true
toc_sticky: true
tags:
  - java
  - js
  - py
categories:
  - Algorithm
---

## Code

---

- Java

    ```java
    package algorithm;

    import java.util.ArrayList;
    import java.util.Arrays;
    import java.util.List;
    import java.util.Scanner;

    public class BinSearch {
        // 요솟수가 n인 배열 a에서 key와 같은 요소를 이진 검색
        static int binSearch(List a, int n, int key) {
            int pl = 0; // 검색 범위의 첫 인덱스
            int pr = n - 1; // 검색 범위의 끝 인덱스

            while (pl <= pr) {
                int pc = (pl + pr) / 2;
                if ((int) a.get(pc) == key)
                    return pc; // 검색 성공
                else if ((int) a.get(pc) < key)
                    pl = pc + 1;
                else
                    pr = pc - 1;
            }

            return -1; // 검색 실패
        }

        public static void main(String[] args) {
            List arr = new ArrayList<Integer>(Arrays.asList(15, 27, 39, 77, 92, 108, 121));

            Scanner stdIn = new Scanner(System.in);
            System.out.print("검색할 값 : ");
            int k = stdIn.nextInt();

            int idx = binSearch(arr, arr.size(), k);

            if (idx == -1)
                System.out.println("그 값의 요소가 없습니다.");
            else
                System.out.println(k + "은(는) arr[" + idx + "]에 있습니다.");
        }

    }
    ```

- JavaScript

    ```js
    var arr = [15, 27, 39, 77, 92, 108, 121];
    var k = parseInt(prompt("검색할 값"));

    var idx = (()=>{
        var pl = 0;
        var pr = arr.length - 1;

        while (pl <= pr) {
            var pc = parseInt((pl + pr) / 2);

            if (arr[pc] == k) return pc;
            else if (arr[pc] < k) pl = pc + 1;
            else pr = pc - 1;
        }

        return -1;
    })();

    if (idx == -1) alert("그 값의 요소가 없습니다.");
    else alert(k + "(은)는 arr[" + idx + "]에 있습니다.");
    ```

- Python
  
    ```py
    def BinSearch(arr, num, key):
        pl = 0
        pr = num - 1

        while pl <= pr:
            pc = (pl + pr) // 2
            if arr[pc] == key: return pc
            elif arr[pc] < key: pl = pc + 1
            else: pr = pc - 1
        
        return -1

    arr = [15, 27, 39, 77, 92, 108, 121]
    key = int(input("검색할 값 : "))
    idx = BinSearch(arr, len(arr), key)

    if idx == -1: print("그 값의 요소가 없습니다.")
    else: print(key, "은(는) arr[", idx, "]에 있습니다.")
    ```

## Reference

---

- Do it! 자료구조와 함께 배우는 알고리즘 입문 자바 편: 실습 3-4