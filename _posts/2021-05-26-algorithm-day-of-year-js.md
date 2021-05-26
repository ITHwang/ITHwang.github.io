---
layout: single
title: "한 해의 경과 일 수 - JavaScript"
toc: true
toc_sticky: true
tags:
  - js
categories:
  - Algorithm
---

주어진 날짜를 기준으로 그 해의 경과 일 수를 구합니다.

## Code

---

- JavaScript

  ```js
  //각 달의 일수
  var mdays = [
    [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31], //평년
    [31, 29, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31], //윤년
  ];

  //서기 year년은 윤년인가?(윤년: 1 / 평년: 0)
  //1. 4로 나누어떨어지면 윤년
  //2. 4, 100으로 나누어떨어지면 평년
  //3. 4, 100, 400으로 나누어떨어지면 윤년
  function isLeap(year) {
    return (year % 4 == 0 && year % 100 != 0) || year % 400 == 0 ? 1 : 0;
  }

  function dayOfYear(y, m, d) {
    d = parseInt(d); //입력 받은 숫자의 type은 string
    while (m >= 2) {
      d += mdays[isLeap(y)][m - 2];
      m--;
    }
    return d;
  }

  alert("그 해 경과 일수를 구합니다.");

  do {
    let year = prompt("해를 입력하세요.");
    let month = prompt("월를 입력하세요.");
    let day = prompt("일를 입력하세요.");

    console.log(`그 해 ${dayOfYear(year, month, day)}일째입니다.`);

    var retry = prompt("한 번 더 할까요? (1.예/0.아니오)");
  } while (retry == 1);
  ```

1. prompt로 입력 받으면 string입니다.
2. `console.log` 시 c언어 스타일의 포맷팅도 가능하고 백틱과 `$`으로도 가능합니다.

## Reference

---

- Do it! 자료구조와 함께 배우는 알고리즘 입문 자바 편: 실습 2-13
