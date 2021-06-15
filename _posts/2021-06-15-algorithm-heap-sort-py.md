---
layout: single
title: "힙 정렬 직접 구현하기 - Python"
toc: true
toc_sticky: true
tags:
  - py
categories:
  - Algorithm
---

## Introduction

---

**힙 정렬**을 파이썬으로 직접 구현해보겠습니다.
힙 정렬에 관한 자세한 설명은 [heejeong Kwon 님의 글](https://gmlwjd9405.github.io/2018/05/10/algorithm-heap-sort.html)로 대신하겠습니다.
아래 코드 구현은 **최대 힙**(max heap)을 기준으로 작성했습니다

## 직접 구현하기

---

max heap으로 정렬된 배열 *arr*에 원소 *item*을 추가할 경우 아래 함수를 사용합니다.

- Code

  ```py
  def insert_max_heap(arr, item):
    # 처음 원소를 추가할 경우, 1부터 index를 참조하기 위해 arr[0]에 더미 원소를 추가한다.
    if (len(arr) == 0): arr.append(None)

    child_idx = len(arr) # 마지막 원소의 index, 맨 아래부터 parent와 비교하며 올라간다.
    parent_idx = int(child_idx / 2)
    arr.append(None) # 참조 가능한 index 늘리기

    # item이 들어갈 자리(child_idx)가 root가 아니며, item이 parent보다 크다면,
    while (child_idx != 1) and (item > arr[parent_idx]):
        arr[child_idx] = arr[parent_idx] # parent 끌어내리기
        # update for next step
        child_idx = parent_idx
        parent_idx = int(parent_idx / 2)

    # item가 들어갈 자리(child_idx)가 root이거나, item이 parent보다 같거나 작으면,
    arr[child_idx] = item
  ```

반대로 맨 앞에 위치한 최댓값을 pop할 경우 아래 함수를 이용합니다.

- Code

  ```py
  def delete_max_heap(arr):
    item = arr[1] # 꺼낼 원소
    if len(arr) == 1: return item # 배열에 더미 원소만 남으면(다 꺼냈으면) 바로 반환

    temp = arr.pop() # 마지막 원소를 꺼내 root로 두고, child와 비교하며 내려보낸다.
    parent_idx = 1
    child_idx = 2

    while child_idx <= len(arr) - 1: # 아래에 비교할 child가 있다면,
        # 두 child node 중 '더 큰 node' 찾기
        if (child_idx < len(arr) - 1) and (arr[child_idx] < arr[child_idx + 1]):
            child_idx += 1

        # 마지막 원소가 '더 큰 node'보다 같거나 크면,
        if (temp >= arr[child_idx]): break

        # 마지막 원소가 '더 큰 node'보다 작으면,
        arr[parent_idx] = arr[child_idx] # '더 큰 node'를 위로 올린다.

        # update for next step
        parent_idx = child_idx
        child_idx *= 2

    arr[parent_idx] = temp

    return item
  ```

`insert_max_heap()` 메서드를 이용하여 정렬되지 않은 배열을 max heap으로 정렬하는 함수도 구현할 수 있습니다.

- Code

  ```py
  def heap_sort(arr):
    tmp = []

    for i in range(len(arr)):
        insert_max_heap(tmp, arr[i])

    for i in range(len(tmp)-1):
        arr[i] = tmp[i + 1]
  ```

사용 예는 다음과 같습니다.

- Code

  ```py
  arr = [4, 1, 6, 15, 20, 2, 11, 9, 7, 12]

  heap_sort(arr)
  print(arr)

  # Output :

  # [20, 15, 11, 9, 12, 2, 4, 1, 7, 6]
  ```

다만 파이썬은 max heap 또는 min heap을 위한 `heapq` 내장 모듈이 있습니다.(기본적으로 min heap입니다.)
사용법은 [Dale Seo 님의 글](https://www.daleseo.com/python-heapq/)을 참고해주세요.

더 나아가 `heapq`를 이용한 `PriorityQueue`의 설명도 [Dale Seo 님의 또 다른 글](https://www.daleseo.com/python-priority-queue/)로 대신하겠습니다. 감사합니다!

## Reference

---

- [[알고리즘] 힙 정렬(heap sort)이란](https://gmlwjd9405.github.io/2018/05/10/algorithm-heap-sort.html)
- [[파이썬] heapq 모듈 사용법](https://www.daleseo.com/python-heapq/)
- [파이썬의 우선순위 큐(PriorityQueue) 사용법](https://www.daleseo.com/python-priority-queue/)
