---
layout: post
cover: 'assets/images/cover6.jpg'
navigation: True
use_math: True
title: 순열과 조합
date: 2020-09-12 10:18:00
tags: algorithms
subclass: 'post tag-algorithms'
logo: 'assets/images/ghost.png'
author: yj
categories: yj
---


Back-tracking 알고리즘을 공부할 때 제일 먼저 구현하는 것이 순열과 조합이다.

Back-tracking 알고리즘에 대해서 입문하고 감을 잡기 위해서 시작하기 좋은 코드이다. 따라서 순열과 조합을 구하는 코드를 보고 외워서 머릿속에 저장해두는 것을 추천한다. 

> 순열과 조합의 차이점:
>
> - 순열: 순열은 순서가 있는 조합이다.(A Permutation is an ordered Combination)
> - 조합: 조합은 순서를 생각하지 않고 선택만 한다.



##### 순열 코드

```java
//Back-tracking 알고리즘
import java.util.Scanner;
import java.util.Stack;
import java.util.Iterator;
class Main{
  public static Stack<Integer> st = new Stack<>();
  public static int cnt = 0;
  
  public static void main(String[] args){
    Scanner sc = new Scanner(System.in);
    
    int n = sc.nextInt();
    int m = sc.nextInt();
    sc.close();
    DFS(n,m);
  }
  
  public static void DFS(int num, int count){
    if(cnt==count){
      print();
      return;
    }
    for(int i=1;i<=num;i++){
      if(st.search(i) != -1)
        continue;
      st.push(i);
      cnt++;
      DFS(num, count);
      st.pop();
      cnt--;
    }
  }
 
  public static void print(){
    Iterator value = st.iterator();
    while(value.hasNext()){
      System.out.printf("%d ", value.next()); 
    }   
    System.out.print("\n");
  }
}
```



##### 조합 코드

```java
//Back-tracking 알고리즘
import java.util.Scanner;
import java.util.Stack;
import java.util.Iterator;

class Main{
  static Stack<Integer> st = new Stack<>();
  static int cnt = 0;
  
  public static void main(String[] args){
    Scanner sc = new Scanner(System.in);
    int n = sc.nextInt();
    int m = sc.nextInt();
    sc.close();
    
    DFS(1, n, m);
  }
  
  public static void DFS(int idx, int num, int count){
    if(cnt == count){
      print();
      return;
    }
    for(int i=idx;i<=num;i++){
      if(st.search(i)!=-1)
        continue;
      st.push(i);
      cnt++;
      DFS(i+1, num, count);
      st.pop();
      cnt--;
    }
  }
  
  public static void print(){
    Iterator value = st.iterator();
    while(value.hasNext()){
      System.out.printf("%d ", value.next());
    }
    System.out.print("\n");
  }
}
```

