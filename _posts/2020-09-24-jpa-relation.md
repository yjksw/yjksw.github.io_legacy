---
layout: post
cover: 'assets/images/cover6.jpg'
navigation: True
use_math: True
title: "JPA의 연관관계 설정"
date: 2020-09-24 10:18:00
tags: backend
subclass: 'post tag-backend'
logo: 'assets/images/ghost.png'
author: yj
categories: yj
---
<br>
![image](https://user-images.githubusercontent.com/63405904/114411529-37273880-9be7-11eb-8acf-9af446138fe8.png)

### 관계 예시

![image](https://user-images.githubusercontent.com/63405904/114411303-02b37c80-9be7-11eb-937a-c8fd90afae89.png)

- 한명의 사람이 여러 아이템을 쇼핑할 때
- 1대다 관계

![image](https://user-images.githubusercontent.com/63405904/114411402-1b239700-9be7-11eb-8f48-141af4cfed11.png)

- Item table에 user_id가 들어가면 한 아이템당 하나의 유저id 만 쓸 수 있으므로 불가하다.
- User 테이블에 item_id가 들어가면 한 유저당 아이템을 1개밖에 못사기 때문에 불가하다.

**해결 방법**

![image](https://user-images.githubusercontent.com/63405904/114411426-1eb71e00-9be7-11eb-920a-f3319c9936da.png)

- 중간에 '주문내역' 테이블을 생성하여 두 가지를 연결시켜 줄 수 있다.
- 유저 한명당 여러가지 주문 내역을 가질 수 있기 때문에 1대N 관계로 존재한다. 한 유저가 몇개의 아이템을 샀는지 확인할 수 있다.
- 아이템에 대해서도 1대N 관계를 가지면 해당 아이템을 몇명의 유저들이 주문했는지 등에 대한 정보를 알 수 있으므로 효율적이다.

#### MySQL Workbench에서 ERD 로 테이블 생성하기

상단에 Database → Reverse Engineer → Continue 누리고 Execute → ERD 할 수 있는 다이어그램 생성. 

- Reverse Engineer: 테이블 → ERD
- Forward Engineer: ERD → 테이블

#### 실습: User 와 Item 테이블을 연결하는 주문내역 database 생성하기

1. Item 테이블과 order_detail 테이블을 필요한 columns 과 맞게 생성한다. 

2. order_detail에 user_id와 item_id 칼럼을 직접 생성해 줄 수 있지만 ERD 관계선을 사용해서 정의하도록 한다 ⇒ 점선 1:N 관계로 User:order_detail & item:order_detail을 연결해준다.

    - 실선(Identifying): 식별관계

    부모 테이블의 PK가 자식 테이블의 FK/PK 가 되는 경우

    부모가 있어야지 자식이 생기는 경우

    - 점선(Non-identifying): 비식별관계

    부모 테이블의 PK가 자식 테이블의 일반속성인 경우

    부모가 없어도 자식이 생기는 경우

3. Forward Enginner를 통해서 ERD 다이아그램을 테이블로 생성한다. 

> ERD를 통해서 데이터베이스를 생성하는 방법이고, 직접 테이블을 생성해도 가능하다!