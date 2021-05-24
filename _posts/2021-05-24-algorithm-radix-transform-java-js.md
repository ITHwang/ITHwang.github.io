---
layout: single
title: "기수 변환 - Java & JavaScript"
toc: true
toc_sticky: true
tags:
    - java
    - js
categories:
    - Algorithm
---

양의 정수와 진수를 입력 받아 n진수로 변환한다.

## Code
---

- Java

    ```java
    import java.util.Scanner;

    public class CardConvRev {

        // x: 변환될 10진수, r: r진수로 변환, d: 변환된 숫자 담는 배열
        static int cardConvR(int x, int r, char[] d) {
            int digits = 0; // 변환을 위해 나눌 때마다 증가시킬 수 -> 결국 자릿수가 됨
            String dchar = "0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ"; // 나눈 나머지로 참조할 배열

            do {
                d[digits++] = dchar.charAt(x % r);
                x /= r;
            } while (x != 0); // 몫이 0이 될 때까지

            return digits;
        }

        public static void main(String[] args) {
            Scanner in = new Scanner(System.in);
            int no; // 변환될 10진수
            int cd; // 기수
            int dno; // 변환 후 자릿수
            int retry; // retry flag
            char[] cno = new char[32]; // 변환된 숫자 담는 배열

            System.out.println("10진수를 기수 변환합니다.");

            do {
                do {
                    System.out.print("변환하는 음이 아닌 정수 : ");
                    no = in.nextInt();
                } while (no < 0);

                do {
                    System.out.print("어떤 진수로 변환할까요? (2~36) : ");
                    cd = in.nextInt();
                } while (cd < 2 || cd > 36);

                dno = cardConvR(no, cd, cno);

                System.out.print(cd + "진수로는 ");
                for (int i = dno - 1; i >= 0; i--) {
                    System.out.print(cno[i]);
                }
                System.out.println("입니다.");

                System.out.print("한 번 더 할까요? (1.예/0.아니오) : ");
                retry = in.nextInt();
            } while (retry == 1);

        }

    }
    ```

- JavaScript

    ```js
    function cardConvR(x, r, arr){
        var digits = 0;
        const dchar = "0123456789ABCDEFGHIJKLMNOPQRSTUVWSYZ";

        do {
            arr[digits++] = dchar[x % r];
            x = parseInt(x / r);
        } while(x != 0);

        return digits;
    }

    alert("10진수를 기수 변환합니다.");
    var cno = new Array(32);

    do {
        do {
            var no = prompt("변환하는 음이 아닌 정수 : ");
        } while (no < 0);

        do {
            var cd = prompt("어떤 진수로 변환할까요? (2~36) : ");
        } while (cd < 2 || cd > 36);

        const dno = cardConvR(no, cd, cno);

        let buf = "";
        for (var i = dno - 1; i >= 0; i--){
            buf += String(cno[i]);
        }

        console.log(cd + "진수로는 " + ans + "입니다.");

        var res = prompt("한 번 더 할까요? (1.예/0.아니오) : ");
    } while (res == 1);
    ```

자바스크립트의 나눗셈 연산은 자바와 달리 소수점까지 계산한다. 
따라서 `ParseInt()`를 이용해 정수부를 구한다.

`console.log()`는 무조건 한 줄 출력이라, 문자열 버퍼에 담아 출력했다. 자바스크립트에서 숫자를 문자열로 바꾸기 위해 `String(x)` 또는  `x.toString()`을 이용한다.

## Reference
---

- Do it! 자료구조와 함께 배우는 알고리즘 입문 자바 편: 실습 2-8