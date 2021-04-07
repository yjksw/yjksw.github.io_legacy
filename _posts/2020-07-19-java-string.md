---
layout: post
cover: 'assets/images/cover6.jpg'
navigation: True
title: "[JAVA] String vs. StringBuilder vs. StringBuffer" 
date: 2020-07-19 10:18:00
tags: java
subclass: 'post tag-java'
logo: 'assets/images/ghost.png'
author: yj
categories: yj
---

<br>
코딩 연습을 하면서 백준 알고리즘 문제를 풀다가 재귀에 대해서 복습할 겸 백준 하노이 타워 문제를 풀었는데 시간초과 오류가 떴다. <small> [문제 바로가기](https://www.acmicpc.net/problem/11729) </small> 

하노이 탑은 매우 단순한 논리이고, 이전에 했던 것을 복습하는 차원에서 반복하는 것이었기 때문에 시간초과 오류가  떠서 의아했다. 여기저기 찾아본 결과 output을 출력하는데 String 변수에 <mark>+</mark> 를 사용하여서 이어 붙여 결과값을 출력했더니 시간초과가 발생했다고 한다. 그럼 String 변수는 왜 시간 초과를 발생시키며, 해결할 수 있는 방법은 뭘까? 

자바를 배운지는 오래 되었지만 한번도 Java api 를 들여다 본 적은 없는데 거기서 확인해보면 java.lang의 패키지의 Class 중, String 이외에도 **StringBuffer, StringBuilder**가 있는 것을 확인할 수 있다. 

**<small>Java 개발자라면 익숙한 class 들을 고민 없이 쓰기 보다 고민하여 효율적인 것을 택해보자!</small>**



---



### String vs. StringBuffer vs. StringBuilder 

다음은 Java API에서 찾은 String, StringBuffer, StringBuilder class 내용이다. 매우 방대한 내용을 담고 있으니 존재하고 있다는 것만 인증하고 넘어가보자. 

<img src="https://user-images.githubusercontent.com/63405904/113874297-9dc7e300-97f0-11eb-9525-8538f6c91ff5.png" style="zoom:50%;" />



<br>

<img src="https://user-images.githubusercontent.com/63405904/113874434-c18b2900-97f0-11eb-9d64-d2a9910dea23.png" style="zoom:50%;" />



<br>

<img src="https://user-images.githubusercontent.com/63405904/113874481-ccde5480-97f0-11eb-9546-d366c2923b2e.png" style="zoom:50%;" />



API의 초반만 살펴보면 String과 나머지 두 class의 차이는 별로 없어보인다. 하지만 자바에서 String을 사용할 때, 값을 추가하고 싶거나 이어붙이고 싶을 때 간편하게 + 를 사용하여서 이어붙인다. 하지만 이렇게 이어 붙이기 보다 StringBuilder, StringBuffer를 사용하라고 한다. *왤까?* 



자, 다음 코드를 들여다 보자. 이제는 돌아가는 코드가 아니라 *효율적인 코드*에 집중하는 **프로**다움을 발휘 할 때이다. 

```java
String str1 = "one";
String str2 = "two";

System.out.println("str1: "+ str1.hashCode());
System.out.println("str2: "+ str2.hashCode());

str1 = str1+str2;
System.out.println("str1: "+ str1.hashCode());

StringBuffer str3 = new StringBugger();
System.out.println("str3: "+ str3.hashCode());

str3.append("three");
System.out.println("str3: "+ str3.hashCode());

>RESULT:
	str1: -1823841245
  str2: -1823841244
  str1: 833872391
  str3: 1956725890
  str3: 1956725890

```



위 코드를 확인해보면, String 클래스로 정의한 str1 과 str1 는 새로운 값을 할당할 때마다 주소값이 새로 생성된다. str1에 str2를 **'+'**를 사용해서 이어붙였을 때에도 또 새로운 주소가 생성되었다. 하지만 StringBuffer 클래스를 사용하여서 생성하고, memory에 append 하는 방식으로 문자열을 추가하였을 때 동일한 주소값을 사용하는 것을 확일할 수 있다.(즉, 클래스를 직접생성하는 것이 아님.) **StringBuffer나 StringBuilder 클래스를 사용할 때는 String 클래스를 생성할 때 method와 variable들을 생성하면서 소요되는 시간이 단축된다.** 

이것이 문제를 재귀함수로 풀때 수십 번을 넘어서 수백 번 String을 더하면서 해당 수많은 주소값이 Stack에 쌓이며 시간과 메모리를 낭비하여서 시간초과 오류가 떴던 것이다. 



---



### String Class는 새로운 주소를 할당

그렇다면 왜 String class는 새로운 주소를 할당하는 것일까? Java에서 제공하는 String 클래스의 내부코드를 들여다보면 다음과 같다. 

```java
public final class String implements java.io.Serializable, Comparable<String>, CharSequence {
  private final chat value[]; 
  ...
  ...
  ...
}
```



즉, String은 사실 value라는 char 형 배열이고 private final char 형으로 선언이 되어 있다. 여기서 private으로 보호되고 있고 final로 변경이 불가능하기 때문에 String에 추가할 때 사실은 변경이 되는 것이 아니라 새로운 주소에 새로 저장하는 원리로 작동하고 있는 것이다. 



---



### StringBuffer vs. StringBuilder

그렇다면 나의 경우 하노이 문제를 풀 때 StringBuffer와 StringBuilder 중 어떤 것을 사용해야하는 걸까? 두 클래스의 차이를 한번 살펴보자. 



<img src="https://user-images.githubusercontent.com/63405904/113874560-e4b5d880-97f0-11eb-9128-09b7516f2a48.png" style="zoom:30%;" />



<img src="https://user-images.githubusercontent.com/63405904/113874646-f9926c00-97f0-11eb-8db4-143c02f89102.png" style="zoom:30%;" />



관련된 사항이 있는 API 문서 부분에 빨간색으로 밑줄을 쳐 놓았다. 보면 StringBuffer는 synchronization을 하기 때문에 Multi-thread 환경에서 사용하고 있다면 더 안전하다. StringBuilder는 반면 *'multiple threads에서는 안전하지 않다'* 라고 분명히 명시해 놓으면서 synchronization을 하지 않는다고  명시해 놓았다.**<small> (synchronization이 필요하면 반드시 StringBuffer를 사용하라는 당부와 함께 말이다.) </small>** 

단순히 성능만 비교한다면 실험을 통해 다음과 같은 결과를 참고할 수 있다. 

<img src="https://user-images.githubusercontent.com/63405904/113874722-0c0ca580-97f1-11eb-9e31-3509d138eef9.png" style="zoom:40%;" />



위 표는 각각 StringBuffer와 StringBuilder를 사용하여서 append 를 여러번 반복했을 때 나온 수치이다. Single-thread를 사용했을 때도 StringBuilder의 시간적인 면에서의 성능이 StringBuffer보다 뛰어났다. 이것은 StringBuffer에서 행하는 동기화(Synchronization)에 의한 차이로 해석할 수 있다. 

---



### 결론

하노이 문제를 해결할 때 필자의 경우 Multi-thread 환경이 아니기 때문에 시간적인 측면에서 더 뛰어난 StringBuilder를 사용해 append 하여 결과를 출력하기로 했다. 결과는 성공이다! 

















<small>[참고자료]: https://novemberde.github.io/2017/04/15/String_0.html, https://www.journaldev.com/538/string-vs-stringbuffer-vs-stringbuilder </small><br><small>[이미지 출처]: https://docs.oracle.com/javase/7/docs/api/, https://www.journaldev.com/538/string-vs-stringbuffer-vs-stringbuilder </small>

