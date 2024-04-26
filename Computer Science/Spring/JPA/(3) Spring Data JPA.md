# 문제
1. 아래의 JPQL은 N:1 관계를 나타낸다 ( O, X )
	> select e, d
	> from Employee e
	> join e.department d
	
2. 아래의  JPQL은 1:N 관계를 나타낸다 ( O, X )
	> select d, e
	> from Department d
	> left join fetch d.Employees e
	
3. 아래의 JPQL은 N:M 관계를 나타낸다. ( O, X )
	> select s, c
	> from Student s
	> join fetch s.courses c
	
	> select c, s
	> from Course c
	> join fetch c.students s
	
4. 아래의 해당 함수를 이용하여 특정 email을 가지는 User가 존재하는지 확인 할 수 있다. (O, X)
	> @Query("select count(u) from User u where u.email = :email")
	> Long countByEmail(String email);
	
5. JPQL에서 집계함수 Max(), Min()은 Number에 관련된 자료형만 집계할 수 있다. ( O, X )

# 정답과 해설
1. 아래의 JPQL은 N:1 관계를 나타낸다 ( O )
- 특정 직원은 하나의 부서를 가질 수 있다. 그러므로 직원과 부서는 N:1 관계이다. 위의 JPQL은 특정 직원을 기준으로 inner join을 시도 하고 있다. 그럼으로 N:1 관계를 나타낸다고 할 수 있다.
	

2. 아래의  JPQL은 1:N 관계를 나타낸다 ( O )
- 특정 부서는 여러명의 직원을 가질 수 있다. 그러므로 부서와 직원은 1:N 관계이다. 위의 JPQL은 특정 직원을 기준으로 left join fetch를 시도하고 있다. 그럼으로 1:N 관계를 나타낸다고 할 수 있다.
	@Entity
	public class Employee {
	    @Id
	    @GeneratedValue(strategy = GenerationType.IDENTITY)
	    private Long id;
	    
	    @ManyToOne
	    @JoinColumn(name = "department_id")
	    private Department department;
	}

	@Entity
	public class Department {
	    @Id
	    @GeneratedValue(strategy = GenerationType.IDENTITY)
	    private Long id;
	
	    @OneToMany(mappedBy = "department")
	    private List<Employee> employees = new ArrayList<>();
	}

3. 아래의 JPQL은 N:M 관계를 나타낸다. ( O )
- 특정 학생은 여러개의 강좌를 수강할 수 있고, 특정 강좌에는 여러명의 학생이 존재할 수 있다. 위의 JPQL은 특정 학생을 기준으로 여러 강좌를 가지고 오고, 특정 강좌를 기준으로 여러 학생들을 가지고 오는 관계를 나타내므로 N:M 관계를 나타낸다고 할 수 있다.
	@Entity
	public class Student {
	    @Id
	    @GeneratedValue(strategy = GenerationType.IDENTITY)
	    private Long id;
	
	    @ManyToMany
	    @JoinTable(
	        name = "student_course",
	        joinColumns = @JoinColumn(name = "student_id"),
	        inverseJoinColumns = @JoinColumn(name = "course_id")
	    )
	    private List<Course> courses = new ArrayList<>();  // 빈 리스트로 초기화
	}
	
	@Entity
	public class Course {
	    @Id
	    @GeneratedValue(strategy = GenerationType.IDENTITY)
	    private Long id;
	
	    @ManyToMany(mappedBy = "courses")
	    private Set<Student> students = new HashSet<>();
	}

4. 아래의 해당 함수를 이용하여 특정 email을 가지는 User가 존재하는지 확인 할 수 있다. ( O )
- 위의 count() 집계함수를 통해 특정 email을 가지는 User의 갯수를 알 수 있다. 이 갯수가 0이면 존재하지 않는것이며, 0이상이면 존재하는 것이니 존재성을 확인 할 수 있다.

5. JPQL에서 집계함수 Max(), Min()은 Number에 관련된 자료형만 집계할 수 있다. ( X )
- max(), min() 집계함수는 숫자와 관련된 자료형 이외에도 String, Data에 대한 자료형에 대해서도 사용이 된다.
