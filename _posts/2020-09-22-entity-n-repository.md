---
layout: post
cover: 'assets/images/cover6.jpg'
navigation: True
use_math: True
title: "[백엔드] Entity 생성과 Repository"
date: 2020-09-22 10:18:00
tags: backend
subclass: 'post tag-backend'
logo: 'assets/images/ghost.png'
author: yj
categories: yj
---

<br>

#### Naming Convention

**Camel Case** : 단어 첫 문자는 소문자, 띄어쓰기 대신 대문자로 구분함. JAVA 변수 선언 시 사용함.

- ex. phoneNumber

**Snake Case** : 단어는 모두 소문자, 띄어쓰기 대신(_)로 구분함. DB 칼럼에 사용. 

- ex. phone_number, created_at

API 정의하기에 따라 다르지만, 주로 API 통신 규격에는 구간에서는 Snake Case를 많이 사용함. 

#### Entity

JPA에서 entity는 DB의 테이블과 매우 유사하다. (DB Table == JPA Entity)

- Entity: JPA에서 테이블을 자동으로 생성해주는 기능 존재.

[Entity](https://www.notion.so/4a74e1bd924f484cace3700cc1d12605)

#### Entity 설정하기

1. 이전에 SQL에서 만든 table 이름과 동일한 이름의 class 를 생성함. 
2. @Entity annotation을 통해서 해당 클래스가 Entity임을 명시함.  
3. DB table에 있는 변수과 같은 이름을 가진 변수를 선언함(변수-camelCase, 칼럼-snake_case)
4. @ID 와 @GeneratedValue를 통해서 primary key 설정을 해줌. 

![image](https://user-images.githubusercontent.com/63405904/114304509-82214d00-9b0e-11eb-9949-81d6094e631f.png){: width="80%"}

<br>

#### Repository

따로 쿼리문을 작성하지 않아도 기본적인 인터페이스로 CRUD를 작성할 수 있다. 

- 사용 방법: 매우 간단함. 아래와 같이 인터페이스에 @Repository annotation을 달아 주고, JpaRepository<T, ID> 을 달아 주면 된다.



```java
@Repository
public interface UserRepository extends JpaRepository<User,Long> {
	//여기서 <T, ID> => T는 어떤 타입의 Entity 인지 / ID 는 식별자의 type에 대해서 적음. 
}
```