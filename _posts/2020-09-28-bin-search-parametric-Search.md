---
layout: post
cover: 'assets/images/cover6.jpg'
navigation: True
use_math: True
title: "[알고리즘] 이분 탐색(Binary Search)응용 파라메트릭서치(Parametric Search)"
date: 2020-09-28 10:18:00
tags: algorithms
subclass: 'post tag-algorithms'
logo: 'assets/images/ghost.png'
author: yj
categories: yj
---


백준 이분탐색 알고리즘 [문제](https://www.acmicpc.net/problem/1654)를 풀다가 **Parametric Search**라는 새로운 개념을 접하게 되었다. 처음 이 랜선 자르기 문제를 접했을 때, 어느 부분에서 이분탐색을 응용해야하는 건지 감이 잡히지 않았다. 아마도 sorting된 특정한 input에 특정한 값을 탐색하는 분야로만 이분탐색을 한정지어서 생각했기 때문에 그 틀에서 벗어난 응용을 생각하기 힘들었던 것 같다. 이런 것을 보면 아직 알고리즘 쪽으로 한참은 더 발전해야 한다고 생각한다. 



### Parametric Search(파라메트릭 서치)

##### 이진탐색과의 차이점:

* 주어진 일련의 값들이 아니라, 주어진 범위 내에서 원하는 값이나 특정한 조건에 일치하는 값을 찾아내는 알고리즘.

  Ex. 이진 탐색 - 1~9에서 3이라는 값을 찾아내는 알고리즘

  ​	   파라메트릭 서치 - 1~9 범위에서 어떠한 조건을 만족하는 3을 찾아가는 알고리즘.

##### 장점: 

Parametric Search를 사용하면 최적화 문제를 결정 문제로 바꾸어 풀 수 있는 장점이 있다. 

* ex. 최대값, 최소값을 찾는 문제 -> 특정 값이 어떤 조건을 만족하는지 확인하는 문제. 



### Parametric Search를 응용한 [랜선 자르기] 풀이

* **[문제 분석]**: 오영식은 K개의 각기 다른 길이를 지닌 랜선을 가지고 있다. 이 랜선들을 가지고 N 개의 랜선을 만들고 싶을 때, N개 혹은 이상의 랜선을 맨들 수 있는 랜선의 최대 길이는 무엇인가?

* **[입력]**: 랜선 갯수 k, 만들고 싶은 랜선 갯수 n

  ​			k 번 동안 각 랜선의 길이 

* **[문제풀이]**: 

  1. 가장 긴 랜선의 길이를 범위로 <1~랜선 길이>를 범위로 parametric search를 한다. 
  2. 이분 탐색과 동일하게 탐색하지만, 같을 경우 해당 mid 값을 return 하는 것이 아니라, mid+1 부터 추가적으로 탐색을 진행해야 한다. 
     * 이것은 특정한 값을 찾는 것이 아니라, 최대 길이를 찾고 싶은 것이기 때문에 추가탐색을 해서 최대로 갈 수 있는 범위 까지 탐색해야하기 때문이다. 

  ** 주의 사항: index가 합쳐지면서 long을 넘기 때문에 left, right, middle은 long을 사용해야 한다. 



#### [코드]

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.IOException;
import java.util.StringTokenizer;
import java.util.Arrays;

class Main{
  public static void main(String[] args) throws IOException{
    BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    StringTokenizer st = new StringTokenizer(br.readLine());
    
    int k, n;
    k = Integer.parseInt(st.nextToken());
    n = Integer.parseInt(st.nextToken());
    
    int[] input = new int[k];
    for(int i=0;i<k;i++){
      input[i] = Integer.parseInt(br.readLine());
    }
    Arrays.sort(input);
    
    long left = 1;
    long right = input[k-1];
    long middle = 0;
    
    while(left<=right) {
      middle = (left+right)/2;
      int count = 0;
      for(int i=0;i<k;i++){
        count += input[i]/middle;
      }
      if(count<n){
        right = middle-1;
      } else {
        left = middle+1;
      }
    }
    System.out.println(right);
  }
}
```



