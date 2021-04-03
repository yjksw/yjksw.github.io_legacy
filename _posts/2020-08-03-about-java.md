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

<br>

## 자바 2차원 배열 정렬하기 

자바에서는 Arrays.sort() 함수를 통해서 손쉽게 배열을 정렬할 수 있다. 하지만 Comparator를 상속 받아서 배열을 정렬하는 이 메소드를 제대로 이해하기 위해서는 상속받은 Comparator 인터페이스의 성질과 override 하고 있는 메소드 들에 대해서 잘 이해하는 것이 좋다.

다음을 자바에서 2원 배열을 먼저 1번째 원소에 대해서 정렬하고, 1번째 원소가 같을 때 2번째 원소의 값에 대해서 정렬하는 것에 대한 코드이다. 알고리즘 문제를 풀면서 다차원 배열을 정렬할 때 매우 유용하므로 기록한다. 

```java
import java.util.Arrays;
import java.util.Comparator;

class Main{
  public static void main(String[] args){
    int[][] arr = new int[10][2];
    
    //위의 2차원 배열 arr에 대해서 1번째 원소에 대해서 정렬하고, 같다면 2번째 원소에 대해서 정렬하기
    Arrays.sort(arr, new Comparator<int[]>(){
      public int compare(int[] t1, int[] t2){
        if(t1[0] == t2[0])
          return t1[1] - t2[1];
        else
          return t1[0] - t2[0];
      }
    });
  }
}
```

<br>

## Comparator 사용

위에서 2차원 배열을 정렬할 때 잠깐 언급하기는 했지만, 어떠한 클래스 변수등에 대해서 특별한(흔치 않은) 기준으로 정렬을 하고 싶을 때 사용하기 좋은 자바 문법이다. 

어떤 기준으로 더 작으면 음수, 같으면 0, 더 크다면 양수를 리턴하여 정렬을 할 수 있도록 해주는 역할을 한다. 

주로 객체를 정렬할 때 유용하게 사용된다.

1. Class 로 따로 만들어서 `new`를 통해서 생성하여 사용할 수 있다.
2. main 안에서 하나의 변수처럼 만들어서 적용할 수 있다. 

```java
import java.util.Comparator;

public static void main(String[] args){
  List<Point> arr = new ArrayList<>();
  Point p1 = new Point(1, 2);
  Point p2 = new Point(2, 1);
  
  arr.add(p1);
  arr.add(p2);
  
  Collections.sort(arr, new xComparator()); //1번 경우
  
  Comparator<Point> yComparator = new Comparator<Point>() {
    public int compare(Point p1, Point, p2){
      return p1.y - p2.y;
    }
  };
  
  Collections.sort(arr, yComparator); //2번 경우
  
  return;
}

class xComparator implements Comparator<Point> {
  public int compare(Point p1, Point p2){
    return p1.x - p2.x;
  }
}
class Point{
  int x, y;
  
  public Point(int x, int y){
    this.x = x;
    this.y = y;
  }
}
```

 