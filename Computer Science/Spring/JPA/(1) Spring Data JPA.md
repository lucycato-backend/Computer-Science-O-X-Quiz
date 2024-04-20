# 문제
1. ORM(Object Relational Mapping)은 관계형 데이터 베이스를 객체 지향 적으로 연결해 주는
	기술이다. ( O / X )

2. JDBC(Java Database Connectivity)는 관계형 데이터베이스에서만 사용될 수 있다. ( O / X )

3. JPA(Java Persistence API)는 MongoDB에서도 사용이 가능하다. ( O / X )

4. Spring Data JPA에서 EntityManager는 Entity의 생명주기를 관리하는 역할을 한며, EntityManger를 통해 데이터베이스의 연산을 수행하고 Entity 객체에 관계형데이터 베이스의 데이터를 매핑시킨다. ( O / X )

5. Spring Data JPA를 사용하여 데이터베이스를 조작하는 방법에는 NativeQuery, QueryDSL, JPQL 3가지가 있다. ( O / X )

# 정답과 해설
1. ORM(Object Relational Mapping)은 관계형 데이터 베이스를 객체 지향 적으로 연결해 주는
	기술이다. ( O )

- ORM(Object Relational Mapping) 기술은 RDB(Relational Database)와 객체 지향 프로그래밍 언어 (OOP)의 간극을 메우는 기술이다. ORM을 사용함으로써, 개발자는 데이터베이스의 테이블을 객체 지향 프로그래밍 언어의 클래스로 매핑 할 수 있게 되며, 이를 통해 데이터를 객체 형태로 쉽게 조회, 생성, 수정, 삭제할 수 있다. 이러한 접근 방식은 RDB를 OOP 관점으로 해석하고 조작할 수 있게 해주며, 통합된 관점을 기반으로 유지보수와 생산성을 향상시킨다.

2. JDBC(Java Database Connectivity)는 관계형 데이터베이스에서만 사용될 수 있다. ( X )

- JDBC(Java Database Connectivity)는 데이터베이스 연결을 위한 표준 API이다. JDBC는 관계형 데이터베이스를 기반으로 설계 되었으나, MongoDB와 같은 NoSQL 데이터 베이스에 접근하기 위한 JDBC 드라이버도 존재한다. 그러므로 관계형 데이터베이스에만 사용이 된다는 설명은 틀린 설명이다. JDBC는 데이터베이스 종류에 관계없이 일관된 접근을 가능하게 하는 추상화된 인터페이스를 제공해준다.

3. JPA(Java Persistence API)는 MongoDB에서도 사용이 가능하다. ( X )

- JPA(Java Persistence API)는 Java의 표준 ORM이다. ORM은 RDB와 OOP 사이의 간극을 메우는 기술로 NoSQL인 MongoDB에서는 사용할 수 없다. 즉, JPA는 관계형 데이터 베이스를 대상으로 한 기술이다. 참고로 NoSQL과 OOP 사이의 간극을 메우는 기술로는 OGM(Object Grid Mapping)이라는 기술로 따로 존재한다. (Hibernate OGM 참고)

4. Spring Data JPA에서 EntityManager는 Entity의 생명주기를 관리하는 역할을 한며, EntityManger를 통해 데이터베이스의 연산을 수행하고 Entity 객체에 관계형데이터 베이스의 데이터를 매핑시킨다. ( O )

- 엔터티의 생명주기 상태
	- new: 
		Entity가 생성되었지만 아직 EntityManager에 의해 관리되지 않는 상태이다. 이 상태의 Entity는 persistence context에 속하지 않으며 데이터베이스에 반영되지 않는다.
	 - managed:
		EntityManager에 의해 관리되는 상태이다. 이 상태는 persistence context에 추가된 상태며 Transaction Commit이 수행이 되면 해당 Entity의 변경사항을 기반으로 데이터베이스에 자동으로 반영이 된다.
	- detached:
		EntityManager의 관리 범위에서 벗어난 상태이다. persistence context가 종료되엇거나, 명시적으로 detach 했을때 이러한 상태가 된다. detached 상태에서는 Entity의 변경사항이 데이터베이스에 반영되지 않는다.
	- removed:
		persistence context에 Entity가 삭제될 것으로 표시하는 상태이다. Transaction Commit이 수행되면 해당 Entity에 매핑된 Column은 삭제된다.
	
* Spring Data JPA를 기반으로한 설명
	- create:
		JpaRepository의 save 함수를 호출하면, 영속성 컨텍스트에 해당 엔터티 인스턴스가 존재하는지 확인한다. 존재하지 않으면 Entity의 상태를 managed로 변경하고 영속성 컨텍스트에 추가하고, 작업이 모두 완료되어 Transaction이 종료되면 데이터베이스에 새롭게 값을 추가한다.
	 - read:
		JpaRepository의 find 함수를 호출하면, 영속성 컨텍스트에 해당 Entity 인스턴스가 존재하는지 확인한다.
		1. 존재한다.
		 JPA는 데이터베이스에 접근하지 않고 영속성 컨텍스트의 엔터티 인스턴스를 반환한다.
		2. 존재하지 않는다.
			JPA는 데이터베이스에 접근하여 엔터티 인스턴스를 생성한다. 이때 이 엔터티의 상태는 managed 상태가 되며 영속성 컨텍스트에 추가하고 그 값을 반환한다.
	- update:
		JpaRepository의 save 함수를 호출하면, 영속성 컨텍스트에 해당 엔터티가 존재하는지 확인한다. 존재한다면 작업이 모두 완료되어 Transaction이 종료될때 해당 엔터티의 반영된 값을 기반으로 데이터베이스의 값을 업데이트 한다.
	- delete:
		JpaRepository의 delete 함수를 호출하면, Entity의 상태를 remove 상태로 변경 후 작업이 모두 완료되면 데이터베이스에서 해당 Entity의 값을 삭제
	
	* 영속성 컨텍스트 (persistence context) 생명주기
		Transaction이 종료 (Commit or Rollback)될때까지 유지된다.
		- @Transactional Annotation을 명시적으로 설정했을때
			- 함수가 모두 종료될때까지 영속성 컨텍스트 유지
		- @Transactional Annotation을 명시적으로 설정하지 않았을때
			- save(), delete(), find() 함수 개별적으로 영속성 컨텍스트 유지

5. Spring Data JPA를 사용하여 데이터베이스를 조작하는 방법에는 NativeQuery, QueryDSL, JPQL 3가지가 있다. ( X ) 

- Spring Data JPA를 사용하여 데이터베이스를 조작하는 방법에는 NativeQuery, JPQL, QueryDSL이외에도 Repository Interface 메소드 등 여러 방법이 있다. 각 방법은 특정한 상황이나 요구 사항에 따라 적절히 선택하여 사용하는것이 중요하다.