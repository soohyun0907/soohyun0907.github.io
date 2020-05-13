---
title: "JavaScript 기초1"
excerpt: "자료형(Data Type)"

categories:
 - Front-End
tags:
 - Web
 - JavaScript
 - Front-End
layout: single
---
> [Inflearn : WEB2-JavaScript](https://www.inflearn.com/course/web2-javascript/dashboard) 강의를 듣고 이를 기반으로 작성된 포스트입니다.

- 프로그래밍? 프로그램(시간의 순서)이라는 순서를 만드는 것.
- WEB = HTML + JavaScript
  - 웹의 특성 : 사용자와 상호작용 하면서도 검색 엔진을 통해 검색한다는 점.
- HTML : 정적이다. 한 번 화면에 출력되면 언제나 그 모습 그대로이다.
  - 웹 페이지를 묘사하는 언어로 시간의 순서에 따라 작성할 필요가 없어서 프로그래밍 언어로 부르지 않는다.
- JavaScript : 동적으로 **<u>사용자와 상호작용</u>**을 하기 위해 생겨난 언어.
  - 사용자와 상호작용을 위해서 시간의 순서에 따라서 웹 브라우저의 여러 기능이 실행되어야 하기 때문에 컴퓨터 프로그래밍 언어라고 볼 수 있다.
  - HTML 위에서 작동한다.
  - 작성방법 : <script\></script\> 사이에 작성

# 데이터 타입

![JavaScript_DataType](https://user-images.githubusercontent.com/33771279/80908461-fb3eee80-8d5a-11ea-91d2-37507d09d4aa.jpg)

## 문자열

- 문자 데이터 타입 : "Hello World" or 'Hello World'
- [자바스크립트 문자열 메소드](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)

## 변수와 대입 연산자

- var x = 1; x에 1을 대입한다는 뜻.
- var은 variable이라는 뜻.

## 비교 연산자

- Boolean을 만들어내는 연산자이다.

- === (일치 연산자) : 자료형 변환 없이 두 연산자가 엄격히 같은지 판별
- !== (불일치 연산자) : 두 연산자가 같지 않거나, 같은 자료형이 아닐 때 true 반환
- == (동등 연산자) : 두 연산자의 자료형이 같지 않은 경우 같아지도록 변환 후 엄격 비교. 피연산자가 모두 객체라면 내부 참조를 보고 둘 다 메모리의 같은 객체를 바라보고 있는지 판별
- != (부등 연산자)
