# WIL3

이번주 GDSC BE Study 과제는 필수 조건이 많네.. 어느정도 안다고는 생각하지만 막상 설명해보라니 설명하기가 어렵다. 이 기회에 잘 정리해서 설명해보자.

아! 다음 과제부터는 새 책을 나가니 새 책을 구해보자. 은근 설레기도하고? ㅋㅋㅋ 암튼 이번주 WIL을 정리해보자.

# **1. 의존과 의존성**

### **의존**

객체지향에서 의존은 신뢰와 관련되어 있다. 특정 클래스 A가 원하는 기능을 하기 위한 코드를 작성할 때, 다른 클래스 B의 Method나 interface를 참조하여 작성한다고 하자. 해당 클래스가 제대로 작동하기 위해선, 자신이 참조한 다른 클래스의 method나 interface가 제대로 동작해야한다. 이들이 제대로 동작하지 않으면 자신 클래스에서 작성할 기능도 제대로 동작하지 않기 때문이다. 즉, A클래스는 B에 의존하고 있는 것이다. 따라서, 의존의 다른 의미는 신뢰를 뜻한다. A가 B클래스를 신뢰하고 B의 무언가를 참조했기에 의존의 다른말은 신뢰이다.

다음 코드의 예시를 보자.

```java
public class MemberService{
	private MemberRepository memberRepository = new MemoryMemberRepoisitory;
}
```

MemberService 클래스가 제대로 작동하기 위해선 MemberRepository 클래스, MemoryMemberRepository 클래스가 제대로 원하는 기능을 하는지 여부가 중요하다. 즉, MemberService 클래스는 MemberRepository와 MemoryMemberRepoisitory에 의존하고 있는 것이다.

![https://blog.kakaocdn.net/dn/bAtRkF/btr8XyU7Hh7/4JWqjcjRXYNd2k46u4kKY1/img.png](https://blog.kakaocdn.net/dn/bAtRkF/btr8XyU7Hh7/4JWqjcjRXYNd2k46u4kKY1/img.png)

### **의존성 : Dependability**

위에서 의존에 대한 언급한 바와 같이 신뢰성이 높음을 의미한다. 의존성은 범위가 넓은 의미인데 아래 5가지의 성질을 만족하면 의존성을 만족한다고 이야기한다.

- Availability : 사용 가능성, 사용자가 서비스에 언제든 접근이 가능해야하는 것을 의미한다.
- reliability : 한 번 시작한 서비스가 중단되지 않고 계속 실행됨을 의미한다.
- security : 보안성, 외부의 공격에 대해서 방어가 가능하다.
- safety : 안전성, 이 서비스를 사용하더라도 다른 사고가 발생하지 않는다.
- intergrity : 무결성, 데이터, 시스템을 내가 마지막 수정한 내역 그대로 보존이 되는 것을 의미함. 즉, 승인된 (유용한) 연산으로만 데이터, 시스템 수정이 가능하고 그 외의 방법으로는 불가능한 것을 의미한다.

# **2. @Autowired 의존성 주입**

### **의존성 주입**

객체지향 프로그램을 만들다보면 클래스들간의 의존 관계가 자연스럽게 발생한다. 해당 의존관계를 코드내에서 구체적으로 명시할 경우, 아래에서 언급할 DIP에 위배되는 경우가 발생한다. 따라서, **외부**에서 클래스간의 관계를 지정해주는 것을 의존성 주입(Dependency Injection) DI라고 한다.

의존성 주입에는 스프링 공식 문서 기준 2가지 방법이 존재한다.

1. 생성자 주입 (Constructor-based Injection) : 생성자 호출시에 의존관계를 결정하는 방법

2. 수정자 주입 (Setter-based Injection) : Setter를 따로 설정해 의존관계를 결정하는 방

### **@Autowired란?**

스프링 컨테이너에 있는 빈들 사이의 의존관계를 자동으로 주입해주는 어노테이션이다.

스프링 빈을 등록 후, @Autowired 어노테이션이 붙은 메소드가 자동으로 호출되고 이어서 의존관계가 자동으로 주입된다.

해당 어노테이션을 활용해 3가지 방법으로 의존관계를 설정할 수 있다.

**1. 생성자 주입**

객체의 생성자를 통해 의존관계를 설정하는 방법이다. 생성자 위에 @Autowired 어노테이션을 추가한다.

(단, 생성자가 하나일땐 적을 필요없다.)

```java
@Component
public class OrderServiceImpl implements OrderService {
	private final MemberRepository memberRepository;
	private final DiscountPolicy discountPolicy;
	@Autowired
	public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
            this.memberRepository = memberRepository;
            this.discountPolicy = discountPolicy;
	}
}
```

- 객체가 생성될때 딱 한번 호출되기때문에 의존관계가 절대 변하지 않는다. 따라서, 필수적이고 확실한 의존관계일때 사용한다.
- 의존 관계에 있는 객체를 final로 생성할 수 있어 생성자를 무조건 사용해야한다는 점, 즉 객체를 호출하지 않을 가능성을 배제한다.

위와 같은 장점이 존재해 현재 강력히 추천되고 있다.

**2. 필드 주입**

```java
@Controller
public class FieldInjectionController {

    @Autowired
    private FieldInjectionService fieldInjectionService;
}
```

위 코드와 같이 필드에 바로 @Autowired를 붙여 의존관계를 설정하는 방법이다.

편리하지만, 가독성이 떨어지며 의존관계가 가변적이라는 단점이 있다.

(뒤에 다른 부분에서 의존관계를 다르게 설정하면 변한다!)

**3. 수정자(Setter) 주입**

setter를 작성하고 그 위에 @Autowired 어노테이션을 활용한 방법이다.

(굳이 setXXX의 이름을 가진 메소드일 필요는 없지만 그래도 해당 이름의 메소드를 사용한다.)

의존성이 고정되지 않고 **선택적이고 가변적**일때 사용하는 방법이다.

해당 방법을 사용하면 다음과 같이 setter를 public으로 설정해야한다. 생성자가 아닌 일반 클래스의 메소드이기 때문에 외부에서 접근할 수 있고, 수정할 수도 있다.

```java
@Component
public class OrderServiceImpl implements OrderService {
	private MemberRepository memberRepository;
	private DiscountPolicy discountPolicy;
	@Autowired
	public void setMemberRepository(MemberRepository memberRepository) {
	    this.memberRepository = memberRepository;
	}
	@Autowired
	public void setDiscountPolicy(DiscountPolicy discountPolicy) {
	    this.discountPolicy = discountPolicy;
	}
}
```

즉, 수정 가능성이 열려있다.

단, 의존성 주입대상 필드에 final을 선언할 수 없다는 단점이 있다.

또한 실제 개발에서 대부분의 의존관계는 변경될일이 거의 없기에 굳이 사용할 이유가 없다.

이렇게 3가지의 방법이 존재하며 현재는 생성자 주입을 권고하고 있다.

# **3. DIP**

**DIP (Dependency Inversion Principle) : 의존관계 역전 원칙**

- 프로그래머는 추상화에 의존해야지, 구체화에 의존하면 안된다. 즉, 구현 클래스에 의존하는 것이 아닌 인터페이스에 의존하라는 의미이다. 즉, 프로그래머의 코드가, 인터페이스에만 의존하는 것이 아닌 구현체에 의존하는 것을 의존관계가 역전된 DIP라고 이야기한다.

좋은 객체지향 프로그래밍을 위해선 클래스간의 의존관계가 명시되고 다형성을 적극 활용해야한다. 이 때 역할과 구현을 분리해야한다. 예를 들어 MemberService를 위해서 Member들에 대한 정보를 알아야한다. 해당 정보를 알기 위해선 Member들의 데이터를 저장한 MemberRepository를 참고(의존)해야한다.

이 때 MemberRepository를 어느 DB로 정할지, 로컬에서 구현을 할지는 여타 개발 상황에 따라 달라질 수 있다. 이 내용이 달라진다고해서 MemberService의 코드를 바꾸면 그게 효율적이고 객체지향적인 프로그래밍일까? 아니다.

MemberRepository 인터페이스를 만든 후, MemberService는 해당 인터페이스만 참조해서 코드를 작성하고 실제 어떤 인터페이스 구현체를 사용할지는 따로 지정해주어야한다.

따라서, 클래스간의 의존 관계를 설정할 때에는 인터페이스(추상화)에 의존하며 구현체(구체화)에는 의존하면 안됨을 의미한다.

아래 코드를 보자.

```java
public class MemberService{
	private MemberRepository memberRepository = new MemoryMemberRepoisitory;
}
```

해당 MemberService는 사실 MemberRepository를 직접 선택하면서 인터페이스인 MemberRepository와 구현체인MemoryMemberRepository에 둘다 의존하고 있다. 즉, DIP에 위배된 것이다.

DIP를 지키기 위해선 코드를 다음과 같이 수정해 인터페이스에만 의존하게 해주어야한다.

```java
public class MemberService{
	private MemberRepository memberRepository;
}
```

# **4. 스프링 빈과 스프링 컨테이너란?**

### **스프링 빈**

스프링 빈이란?

스프링 컨테이너가 관리하는 자바 객체를 빈(Bean)이라고 한다.

스프링은 자신이 사용할지도 모르는 객체들을 미리 스프링 컨테이너에 등록하고 꺼내어 사용한다.

이 객체들을 Bean이라한다.

Bean을 등록하기 위해선 두 가지 방법이 존재한다.

**1. 어노테이션 사용하기**

- @Component 기반의 어노테이션을 사용하여 등록한다. (ex : @Controller, @Service, @Repository)

**2. 빈 설정 파일에 직접 등록하기**

- @Configuration이 붙은 설정 파일에 @Bean 어노테이션을 사용하여 직접 등록한다.

### **스프링 컨테이너**

컨테이너란?

외부에서 객체를 생성하고 관리하며 **객체들간의 의존관계를 주입하는 객체를** DI컨테이너, 컨테이너라고 함.

스프링 컨테이너란?

스프링이 사용할지도 모르는 객체들(Bean)을 등록 후, 이들을 생성하고 관리하며 의존 관계를 주입하는 컨테이너를 스프링 컨테이라고 함.

스프링 컨테이너에는 크게 두 가지가 있다.

1. BeanFactory : Bean을 관리하기 위한 다양한 메소드들이 구현되어있다.

2. ApplicationContext : BeanFactory를 상속 받은 객체로 해당 객체를 실제로 스프링 컨테이너라고 부른다. BeanFactory의 빈 관리 기능을 모두 포함하고 있으며, Application개발에 도움이 되는 다양한 기능을 추가로 구현하고 있다.

# **5. JDBC와 JPA?**

### **JDBC**

Java Database Connectivity의 약자로 자바 프로그램이 데이터베이스와 연결되어 데이터를 주고 받을 수 있게 해주는 인터페이스를 의미함.

즉, 자바 프로그램과 데이터베이스가 데이터를 주고 받기 위해 사용하는 API이다.

해당 방법은 현재는 잘 사용하지 않는 방법으로 반복적으로 코드를 작성해야한다는 단점이 있다.

이와 같은 반복 코드를 해결하기 위해 JdbcTemplate이 등장했지만 여기서도 sql을 직접 작성해줘야한다는 단점이 있다.

### **JPA란?**

Java Persistence API의 약자로 자바 표준명세서이다.

자바 어플리케이션에서 관계형 데이터베이스를 사용하기 위해 정의한 인터페이스로 구현체가 필요한데 Hibernate, Elipse Link등의 구현체가 존재함.

JPA를 사용하면 위에서 Jdbc를 사용했을때의 단점인 반복적인 코드 작성, SQL 작성 문제를 해결해준다.

Jpa에선 기본적인 CRUD기능에 관한 SQL을 처리해주는데 따라서 개발자는 sql과 데이터 중심의 설계가 아닌 객체 중심으로 설계를 할 수 있어 개발 생산성이 높아진다.

또한 Sql 관련 코드를 Jpa가 대신 처리해주기에 유지보수가 쉬워진다는 장점 또한 존재한다.

**Spring Data JPA**

Spring에서 JPA를 사용할땐, Spring Data JPA 모듈을 사용하는데 Hibernate를 한 번 더 감싼 느낌이다.

JPA <- HIbernate <- Spring Data JPA

이렇게 감싼 이유는

**1. 구현체 교체의 용이성**

내부에서 구현체 매핑을 진행하기 때문에 Hibernate에서 다른 구현체를 사용할때 쉽게 교체할 수 있다.

**2. 저장소 교체의 용이성**

관계형 데이터베이스 -> MongoDB로 교체할때 dependencies만 교체해주면 된다.

이는 Jpa보다 상위 개념이므로 Jpa를 우선 익힌 후에 사용하도록 하자.