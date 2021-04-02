---
layout: post
cover: 'assets/images/cover6.jpg'
navigation: True
use_math: True
title: "[JAVA] 유용 문법 모음"
date: 2020-08-03 10:18:00
tags: java
subclass: 'post tag-java'
logo: 'assets/images/ghost.png'
author: yj
categories: yj

---

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

## BufferedReader

* Scanner를 사용하여서 많은 데이터를 입력할 때 시간초과가 일어나기도 함. 

* 한꺼번에 버퍼에 저장했다가 한번에 읽어드리는 BufferedReader가 확연히 빠르게 동작함. 

```java
import java.io.*;
import java.util.StringTokenizer;

class Main{
  public static void main(String[] args){
    BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    String input = "";
    
    try{
      input = br.readLine();
    } catch(IOException e){}
    StringTokenizer st = new StringTokenizer(input);
    
    int n = Integer.parseInt(st.nextToken());
    int m = Integer.paresInt(st.nextToken());
    
    br.close();
  }
}
    
```

* BufferedReader는 항상 try-catch를 사용해서 읽음.
* 한줄 씩 밖에 읽을 수 없고, 반드시 String으로 들어오기 때문에 입력 데이터에 대한 가공이 필요함.
  * StringTokenizer로 원하는 문자 기준으로 나눌 수 있음.
  * Integer.parseInt를 사용하여 스트링을 정수로 변함.
  * 반드시 close() 해주어야 함. 

<br>

## Iterator

* 여러모로 유명하고 유용하나 Stack등에 대한 자료구조에 있어서 pop을 하지 않고 데이터 처음부터 읽을 수 있음.

```java
import java.util.Iterator;

class Main{
  public static void main(String[] args){
    public static Stack<Integer> st = new Stack<Integer>();
    st.push(1);
    st.push(2);
    st.push(3);
    
    Iterator value = st.iterator();
    while(value.hasNext()){
      System.out.println(value.next());
    }
  }
}
```



## 