---
layout: post
cover: 'assets/images/cover6.jpg'
navigation: True
use_math: True
title: "JPA 연관관계 설정 코딩하기-1"
date: 2020-09-24 10:18:00
tags: backend
subclass: 'post tag-backend'
logo: 'assets/images/ghost.png'
author: yj
categories: yj
---
<br>
### 실습을 통해서 JPA 연관관계 Intellij 프로젝트에 적용하기

테이블을 생성하게 되면 거기에 맞는 entity를 프로젝트에 추가해 주어야 한다.

<br>

#### Entity 생성

1. Java → model 패키지 → Entity 패키지 → 새로운 테이블에 대한 Class 생성
2. **@Data** 와 **@Entity annotation**을 추가
3. 테이블에 있는 컬럼들과 같은 이름을 가진 변수들 생성.
4. Primary 키(Id)에 대해서 **@Id** 와 **@GeneratedValue(strategy=GenerationType.IDENTITY)** 를 추가
5. 모든 생성자, 기본 생성자에 대한 annotation을 추가:

    @AllArgsConstructor @NoArgsConstructor

#### Repository 생성

- 각 Entity 당 repository 가 있어야 한다. 

1. Java → model 패키지 → Repository 패키지 → ItemRepository,,등등 이름을 가진 Interface 생성
2. **@Repository** 추가
3. 아래 JpaRepository extend. 

```java
public interface ItemRepository extends JpaRepository<T, idType> {}
```

#### Entity와 Repository 동작확인

1. test → java → repository → ItemRepositoryTest/OrderDetailRepositoryTest 생성
2. **extends StudyApplicationTests** 상속 받기
3. test 할 repository 변수 생성하고, **@Autowired** 추가

```java
@Autowired
private ItemRepository itemRepository;
```

4. test 할 메소드들 만들고, @Test annotation 반드시 달기.

```java
ex. create 일 경우,

@Test
public void create() {
	Item item = new Item();
	item.setName("노트북");
	item.setPrice(100000);
	item.setContent("삼성 노트북");

	Item newItem = itemRepository.save(item);
	Assert.assertNotNull(newItem);
}
```

<br>

<br>

- **참고**: Intellij 에서 Junit 클래스 Assert를 사용하기 위해서는 Gradle에 다음 dependency를 추가해야 한다.

```java
testImplementation 'org.junit.jupiter:junit-jupiter-api:5.6.0'
testRuntimeOnly 'org.junit.jupiter:junit-jupiter-engine'
```