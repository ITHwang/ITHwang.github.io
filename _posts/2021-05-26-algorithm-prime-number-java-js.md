---
layout: single
title: "소수 찾기(prime number) - Java & JavaScript"
use_math: true
toc: true
toc_sticky: true
tags:
  - java
  - js
categories:
  - Algorithm
---

1000 이하의 소수 모두 나열하기

## Code

---

- Java

  ```java
  public class PrimeNumber {
      public static void main(String[] args) {
          int counter = 0; // 연산 횟수(복잡도 측정)
          int ptr = 0; // 찾은 소수의 개수
          int[] prime = new int[500]; // 소수 저장하는 배열

          prime[ptr++] = 2; // 홀수만 검색하기 위해 미리 넣어둠

          for (int n = 3; n <= 1000; n += 2) { // 홀수만 검색
              boolean flag = false;

              for (int i = 0; prime[i] * prime[i] <= n; i++) {
                  counter += 2; // 연산 두 번 함
                  if (n % prime[i] == 0) {
                      flag = true;
                      break;
                  }
              }

              if (!flag) {
                  prime[ptr++] = n;
                  counter++; // 위 for 문에서 마지막으로 조건 따질 때 했던 곱셈 연산
              }
          }

          for (int i = 0; i < ptr; i++)
              System.out.println(prime[i]);

          System.out.println("연산 횟수: " + counter);

      }

  }
  ```

- JavaScript

  ```js
  var counter = 0;
  var ptr = 0;
  var prime = new Array(500);

  prime[ptr++] = 2;

  for (var n = 3; n <= 1000; n += 2) {
  var flag = false;

  for (var i = 0; prime[i] * prime[i] <= n; i++) {
      counter += 2;
      if (n % prime[i] == 0) {
      flag = true;
      break;
      }
  }

  if (!flag) {
      prime[ptr++] = n;
      counter++;
  }


  console.log("연산 횟수: " + counter);
  ```

1. 어떤 정수 /(n/)에 대하여, /(n/)의 제곱근 이하의 어떤 소수로도 나누어떨어지지 않으면 n은 소수다.
2. 소수 중에 2를 제외하고 모두 홀수다.

## Reference

---

- Do it! 자료구조와 함께 배우는 알고리즘 입문 자바 편: 실습 2-11
