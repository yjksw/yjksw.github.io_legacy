---
layout: post
cover: 'assets/images/cover6.jpg'
navigation: True
title: "[알고리즘] 세그먼트 트리를 활용한 히스토그램 문제 풀이_2"
date: 2020-09-10 10:18:00
tags: algorithms
subclass: 'post tag-algorithms'
logo: 'assets/images/ghost.png'
author: yj
categories: yj
---
앞서 [히스토그램 문제](https://www.acmicpc.net/problem/6549)에 대한 접근 방법을 간단하게 설명하고 세그먼트 트리를 히스토그램에 맞추어서 설명했다. 이번 글에서는 구체적으로 어떻게 세그먼트 트리를 구현하여 히스토그램 문제를 푸는데까지 이어지는지 다루어 보도록 하겠다. 

이 문제는 레벨이 높은 문제이긴 하지만 아이디어 자체가 굉장히 어렵거나 하진 않다. 다만 시간 복잡도 측면에서 효율적으로 접근하기 위해 세그먼트 트리를 활용하는게 좀 낯설어서 어려웠던 것 같다. 



###### Segment Tree 구현

Segment Tree를 구현할 때 배열을 사용해서 구현하도록 할텐데 segment tree는 다음과 같은 성질을 가지고 있다. 

* 세그먼트 트리는 거의 Full Binary Tree(비슷한 형태를 지님)의 모습을 하고 있다. 
* 왼쪽 자식: 부모노트 * 2
* 오른쪽 자식: 부모노드 * 2 + 1 
* 높이: lgN

배열을 통해서 tree를 구현하려면 사전에 tree의 노드 갯수를 파악해서 배열의 크기를 지정해야한다. 위의 성질들을 이용하면 해당 tree의 크기를 계산할 수 있다. 예를 들어 기존 배열의 개수가 **2의 제곱인 경우**에는 높기가 lgN 이므로 필요한 노드의 갯수는 `2*N-1` 이다. **2의 제곱이 아닌 경우**에는 N보다 큰지만 가장 가까운 2의 제곱을 찾으면 된다. 따라서 그 경우 노드의 갯수는 `2*2^(lgN+2)-1`이 된다. 

이렇게 크기를 지정해서 배열을 생성한 이후에 재귀 함수를 사용해서 이전 포스트에서 이야기 했던 부분을 구현하면 된다. 재귀를 잘 이해했다면 segment tree 생성은 크게 어렵지 않다. 



**코드: **

먼저 segment tree를 저장할 배열 공간을 할당한다.  다음은 구현할 때 유용한 몇가지 JAVA 함수를 소개해준다. 

* Ceil: 올림 숫자
* Math.log10(n)/Math.log10(2) = log2n

Segment Tree의 index는 1부터 시작해야 한다. 그래야지 `2*i, 2*i+1`로 왼쪽 자식노드와 오른쪽 자식노드를 구별할 수 있다. 

```java
static void main(String[] args){
  ...
  int height = (int)Math.ceil(Math.log10(n)/Math.log10(2));
  int size = (int)Math.pow(2, height+1);
  
  int[] binTree = new int[size];
  init(0, n-1, 1);
}
```

다음은 segment tree에 값을 할당하는 부분이다. 

```java
static void init(int start, int end, int index){
  if(start==end) {
    binTree[index] = start;
    return;
  }
  init(start, (start+end)/2, index*2);
  init((start+end)/2+1, end, index*2+1);
  if(value[binTree[index*2]]<=value[binTree[index*2+1]])
    binTree[index] = binTree[index*2];
  else
    binTree[index] = binTree[index*2+1];
  return;
}
```



###### Segment Tree 탐색

Segment Tree를 생성했으면 이제 각 구간을 순회하며 해당 구간의 최소값을 구해야 한다. 일반적인 세그먼트 트리의 예시에서 구간합을 구할 경우 각 segment tree에 있는 값의 합을 구하면 되지만, 히스토그램 문제에서는 최소값을 찾아야 하니, 한번 더 참조해야 하는 부분이 있다. 

이 부분은 세그먼트 트리에 대해서 설명해놓은 [백준 블로그](https://www.acmicpc.net/blog/view/9)를 참조하면서 이해 했는데 매우 잘 설명이 되어 있다. 세그먼트의 해당 노드가 담당하고 있는 구간을 [start, end]로, 합을 구하는 목적 구간을 [left, right]로 놓았을 때 다음 4가지 경우가 있다. 

1. 합을 구해야하는 [left, right]와 현재 노드가 담당하고 있는 [start, end]가 겹치지 않는 경우
2. 합을 구해야하는 [left, right]가 현재 노드가 담당하고 있는 [start, end]를 완전히 포함하는 경우
3. 현재 노드가 담당하고 있는 [start, end]가 [left, right]를 완전히 포함하는 경우
4. [left, right]와 [start, end]가 겹쳐져 있는 경우 (1, 2, 3을 제외한 나머지)

위의 4가지 경우에 대해서 다음과 같이 처리한다. 

```
1번 경우: if(left > end || right < start) 
	- 겹치지 않으므로 탐색할 필요 없음
2번 경우: if(left <= start && end <= right)
	- 해당 노드의 값을 리턴함
3,4번 경우:
	- 각각 왼쪽, 오른쪽 자식 노드에서 탐색함. 
```

히스토그램 문제에서는 최소값을 찾아야 하는 것이기 때문에 왼쪽과 오른쪽 자식 노드로 나누어서 들어갈 때, 배열을 한번 더 참조해서 구간에서 최종 최소값이 있는 위치를 찾아야 한다. 



**코드:**

```java
static int findMin(int start, int last, int left, int right, int index){
  if(left>last || right<start)
    return -1;
  if(left<=start && right>=last)
    return binTree[index];
  else {
    int temp1 = findMin(start,(start+last)/2, left, right, index*2);
    int temp2 = findMin((start+last/2)+1, last, left, right, index*2+1);
    if(temp1 == -1)
      return temp2;
    else if(temp2 == -1)
      return temp1;
    else {
      if(value[temp1] <= value[temp2])
        return temp1;
      return temp2;
    }
  }
}
```



###### 히스토그램 풀이

위의 세그먼트 트리 생성과 탐색 방법을 사용해서 최소값을 찾는 부분을 구현했다면 이제 답을 구현하도록 해보자. 앞의 포스트에서 언급했던 방법은 아래이다. 

> 먼저 [문제](https://www.acmicpc.net/problem/6549)의 해결 방법을 요약하면 다음과 같다. 
>
> > 1. 히스토그램 중, 높이가 가장 낮은 min 값과 해당 너비값을 곱하여 넓이를 구함. 
> > 2. 해당 최소값을 기준으로 히스토램을 나누어서 1번을 반복함. 
> > 3. 더 이상 나눌 수 없을 때까지 반복하며 매번 넓이의 max 값을 업데이트 함. 

위의 방법이 분할정복인 이유는 반복적으로 나뉘어지는 구간에서의 직사각형을 계속 비교하면서 최대 크기를 찾기 때문이다. 세그먼트 트리에 저장된 최소값의 위치를 활용해서 해당 기준으로 나누고, 나눈 구간에서의 직사각형 넓이 구할 때 사용하도록 한다. 



**코드: **

```java
static void solve(int startIndex, int lastIndex){
  long area = 0;
  if(startIndex == lastIndex){
    area = value[startIndex];
    if(area>max)
      max = area;
    return;
  }
  int minIndex = findMin(0, n-1, startIndex, lastIndex, 1);
  long min = value[minIndex];
  
  if(area>max)
    max = area;
  
  if(minIndex == startIndex)
    solve(minIndex+1, lastIndex);
  else if(minIndex >= lastIndex)
    solve(startIndex, minIndex-1);
  else {
    solve(startIndex, minIndex-1);
    solve(minIndex+1, lastIndex);
  }
}
```







**<small>[참고 자료]: https://www.acmicpc.net/blog/view/12, https://www.crocus.co.kr/648, https://www.acmicpc.net/blog/view/9 </small>**

