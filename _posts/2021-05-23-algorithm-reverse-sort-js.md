---
layout: single
title: "역순 정렬 - JavaScript"
toc: true
toc_sticky: true
tags:
    - js
categories:
    - Algorithm
---

```js
function swap(arr, idx1, idx2){
    var tmp = arr[idx1];
    arr[idx1] = arr[idx2];
    arr[idx2] = tmp;
}

function reverse(arr){
    for (let i = 0; i < arr.length / 2; i++){
        swap(arr, i, arr.length - i - 1);
    }
}

const num = prompt("요솟수를 입력하세요.");
let arr = new Array(num);

for (let i = 0; i < num; i++){
    arr[i] = prompt((i + 1) + "번째 요소를 입력하세요.");
    console.log(arr[i]);
}

reverse(arr);

console.log("요소를 역순으로 정렬했습니다.");
for (let i=0; i < num; i++){
    console.log(arr[i]);
}
```

## Reference
---

- Do it! 자료구조와 함께 배우는 알고리즘 입문 자바 편: 실습 2-6