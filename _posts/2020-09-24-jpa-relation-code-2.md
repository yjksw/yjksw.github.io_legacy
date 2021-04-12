---
layout: post
cover: 'assets/images/cover6.jpg'
navigation: True
use_math: True
title: "JPA 연관관계 설정 코딩하기-2"
date: 2020-09-24 10:18:00
tags: backend
subclass: 'post tag-backend'
logo: 'assets/images/ghost.png'
author: yj
categories: yj
---
<br>

### Annotation으로 연관관계 설정하기

- 목표는 1:N 관계 설정이다.
- 현재 상태:
    1. User와 OrderDetail이라는 테이블이 1:N 관계가 설정되어 있다. 

<br>

#### User Table 과 OrderDetail Table 1:N 관계 설정하기

- OrderDetail 테이블의 입장에서 ManyToOne 이므로,
1. OrderDetail 에 **private Long UserId → private User user** 로 변경.
2. 해당 변수 위에 **@ManyToOne** 추가

- User 테이블의 입장에서 OneToMany 이므로,
1.  User 클래스에 orderDetailList를 받을 List<OrderDetail> 변수를 생성. 

    private List<OrderDetail> orderDetailList;

2. **@OneToMany**를 변수 위에 추가.

    @OneToMany(fetch = FetchType.LAZY, mappedBy = "user")

    OrderDetail이라는 클래스 안에 "user"라는 변수와 매칭시키겠다는 것. 

---

**연관관계 설정 간단 정리:** 

1. 먼저 변수가 있는 Class에 가서 현재 있는 Class 기준으로 어떤 관계인지 파악 후, 해당 변수 위에 annotation을 추가함. 
2. 이후 Long 등이 아니라, 객체로 연결해야 함. 
3. One 입장인 Class에 가서 상대는 Many이기 때문에 List로 받는다는 변수를 추가함. 
4. Annotation을 추가하고, fetch type, mapped by 등등을 추가함.

<br>

#### 연관관계 설정 Test 하기

- UserRepositoryTest에 가서 read() 함수에 대한 test를 작성해보자.

```java
@Test
    @Transactional
    public void read() {
        Optional<User> user = userRepository.findById(4L);

        user.ifPresent(selectUser-> {
            selectUser.getOrderDetailList().stream().forEach(detail -> {
                Item item = detail.getItem();
                System.out.println(item);
            });
        });
    }
```

- 이렇게 하면, item에 대한 결과값이 String으로 나오게 됌

> 주의사항: Lombok을 사용할 때, 여기서 toString을 사용하게 되면, OrderDetail class에서 User, Item에 대한 객체에 대해서도 toString을 실행하기 때문에 overflow가 발생해서 상충하여 오류가 남. 

**OrderDetail** 에서 다음을 추가해서 제외 시키는 제어문을 넣어주어야 함. 

@ToString(exclude={"user", "item");