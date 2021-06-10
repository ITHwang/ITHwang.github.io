---
layout: single
title: "보초법 - JavaScript"
use_math: true
toc: true
toc_sticky: true
tags:
  - js
categories:
  - Algorithm
---

## Introduction

---

맨 앞부터 순서대로 검색하는 **선형 검색**(\(Linear\,Search\))은 반복문을 수행할 때 두 가지 종료 조건을 판단합니다.

1. 검색할 값을 발견하지 못 하고 배열의 끝을 지나간 경우
2. 검색할 값과 같은 요소를 발견한 경우

**보초법**(\(sentinel\,method\))을 이용하면 첫 번째 종료 조건을 두 번째 종료 조건에 포함시켜 비용을 줄일 수 있습니다.

## While & For

---

while로 구현하면 다음과 같습니다.

- while

  ```js
  //arr: 배열
  //length: 배열의 길이
  //key: 찾고자 하는 요솟값

  function seqSearchWhile(arr, length, key) {
    var i = 0;

    while (true) {
      if (i == length) return -1; //종료 조건1
      if (arr[i] == key) return i; //종료 조건2
      i++;
    }
  }
  ```

무한 반복문까지 따지면 조건 판단 횟수는 3회입니다.
이를 for로 보다 간결하게 구현할 수 있습니다.

- for

  ```js
  //arr: 배열
  //length: 배열의 길이
  //key: 찾고자 하는 요솟값

  function seqSearchFor(arr, length, key) {
    for (var i = 0; i < length; i++) if (a[i] == key) return i;
    return -1;
  }
  ```

조건 판단 횟수를 2회로 줄였지만, 종료 조건 개수는 같습니다.

## 보초법

---

보초법은 찾고자 하는 요소를 배열 맨 끝에 저장하여, 무조건 종료 조건2를 만족시킵니다.
다만 반환된 인덱스 값을 조사하여 종료 조건1의 기능을 수행합니다.

- sentinel

  ```js
  //arr: 배열
  //length: 배열의 길이
  //key: 찾고자 하는 요솟값

  function seqSearchSen(arr, length, key) {
    var i = 0;

    arr[length] = key; //배열 맨 끝에 key 저장

    while (true) {
      if (arr[i] == key) break; //종료 조건2
      i++;
    }

    return i == n ? -1 : i; //인덱스가 맨 끝의 인덱스면 검색 실패
  }
  ```

  조건 판단 횟수는 2회로 for문과 같지만, 종료 조건은 하나로 줄었습니다.

## Reference

---

- 시바타 보요, 『Do it! 자료구조와 함께 배우는 알고리즘 입문 자바 편』, 강민, 이지스퍼블리싱(주), 2018, 100~107쪽
