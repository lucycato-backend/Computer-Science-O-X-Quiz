# 문제
1. @OneToMany 관계에서는 항상 연관관계의 주인이 되는 쪽에 mappedBy 속성을 사용해야
	한다. ( O / X )

2. @ManyToOne 관계에서는 기본 패치 전략이 FetchType.LAZY이다. ( O / X )

3. @ManyToOne 어노테이션을 사용하는 엔티티는 연관관계의 주인이 되며, 외래 키를 관리한다. ( O / X )

4. JPA에서 @OneToMany 관계를 설정할 때, 컬렉션 타입은 List만 사용할 수 있다. ( O / X )

5. 연관관계 매핑에서 @JoinColumn 어노테이션은 연관관계의 주인이 아닌 쪽에서만 
	사용될 수 있다. ( O / X )


# 정답과 해설

1. @OneToMany 관계에서는 항상 연관관계의 주인이 되는 쪽에 mappedBy 속성을 사용해야 한다. ( X )

- 연관 관계의 주인 즉 외례키를 가지고 있는 Entity이다. 연관 관계의 주인이 아닌 쪽에서 mappedBy를 설정하여 연관 관계 매핑해야 한다.

2. @ManyToOne 관계에서는 기본 패치 전략이 FetchType.LAZY이다. ( X )

- 기본 패치 전략은 FetchType.EAGER이다. FetchType.LAZY는 지연로딩을 하게 해주는 설정이며 FetchType.EAGER는 즉시로딩을 하는 설정이다. 

3. @ManyToOne 어노테이션을 사용하는 엔티티는 연관관계의 주인이 되며, 
	외래 키를 관리한다.  ( O )

-  외래 키를 관리하는 엔터티가 연관관계의 주인이라고 할 수 있다.

4. JPA에서 @OneToMany 관계를 설정할 때, 컬렉션 타입은 List만 사용할 수 있다. ( X )

- Set, Map과 같은 다양한 컬렉션 타입을 사용할 수 있다.

5. 연관관계 매핑에서 @JoinColumn 어노테이션은 연관관계의 주인이 아닌 쪽에서만 
	사용될 수 있다. (X)

- @JoinColumn는 외래 키를 매핑하는데 사용되는 어노테이션이다. 주로 연관관계의 주인이 되는 쪽에서 사용이 된다. 그러니 주인이 아닌 쪽에서만 사용된다는 설명은 틀린 설명이다.


# 추가설명

### JPA 기본 개념
- 매핑
1. 단방향 매핑
	- 단 방향 참조
2. 양방향 매핑
	- 양 방향 참조

- 주요 어노테이션
1. @OneToOne (1:1)
2. @OneToMany (1:N)
3. @ManyToOne (N:1)
4. @ManyToMany (N:M)
5. @Fetch
	- EAGER과 다르게 특정 쿼리를 통해서 즉시로딩을 도모할 수 있다. 쿼리 최적화 및 효율성을 각 상황에 도입해야 할때 중요한 개념이다.

- 주요 개념
7. 연관 관계 주인 설정 (mappedby)
	- 주로 외래키를 가지는 쪽이 연관관계의 주인이 된다.

8. 지연로딩 (LAZY) vs 즉시로딩 (EAGER) (fetch)
	- 지연 로딩을 하면 연관관계가 매핑된 객체를 사용할때 객체를 로딩하여 효율성을 모색할 수 있다. 그러나 EAGER로 설정하면 처음부터 로딩을 한다.

9. 영속성 전파 (casacade) 
	- 부모의 상태 변화가 자식의 상태변화 까지 영향을 주느냐에 대한 속성이다.

10. JPQL (Java Persistence Query Language) vs SQL (Struct Query Language)
	- JPQL을 사용하면 JOIN FETCH와 같은 문법을 통해서 즉시로딩과 지연로딩을 디테일하게 관리를 할 수 있지만 NativeQuery(SQL)을 사용하면 즉시로딩으로 매핑된다는 점을 주의 해야 한다.