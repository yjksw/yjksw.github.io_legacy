​---
layout: post
cover: 'assets/images/cover6.jpg'
navigation: True
title: "[JAVA] 유용 문법 모음" 
date: 2020-08-03 10:18:00
tags: java
subclass: 'post tag-java'
logo: 'assets/images/ghost.png'
author: yj
categories: yj
​---

<br>

> Java를 사용해서 코딩 테스트 준비 겸 알고리즘 문제를 풀어보다 알아두면 유용한 자바 문법이나 코드 등등을 맥락없이 적어놓는 메모장 같은 페이지.

## StringBuilder & StringBuffer

알고리즘은 맞지만 시간초과가 걸릴 때 유용함. 

* String 변수에 '+' 연산자를 써서 이어붙이면 매번 새로운 메모리를 할당하기 때문에 시간초과가 걸림.
* StringBuilder 를 사용하면 동일한 메모리에 append 하므로 시간측면에서 유용함. 

```java
StringBuilder sb = new StringBuilder();
sb.append("Hello ");
sb.append("My name is Yun");
sb.append("\n");
System.out.println(sb);
```

<br>

