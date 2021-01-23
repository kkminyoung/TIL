# IOC
- Inversoin of Control : 제어권의 역전
- 일반적인 경우 원래 의존성에 대한 제어권을 자기 자신이 가진다.

```java
class OwnerController {
    private final OwnerRepository owners = new OwnerRepository();
}
```

OwnerController에서 사용할 Repository 객체를 직접 생성하지 않고, 생성자를 통해 바깥쪽에서 주입한다. 
Controller 객체를 만들 때 무조건 repository 객체가 있어야 하기 때문에, 이 객체가 없으면 객체 생성 자체가 불가능하다.

DI(의존성 주입)를 이용하면 개발자가 실수로 owners를 초기화 하지 않아서 NPE가 발생하는 문제를 예방할 수 있다.

```java
class OwnerController {
    private final OwnerRepository owners;

    public OwnerController(OwnerRepository clinicService, VisitRepository visits) {
        this.owners = clinicService;
        this.visits = visits;
    }
}
```

### loC 컨테이너
- 빈(bean)을 만들고 엮어주며 제공해준다.
- 빈이란? IoC 컨테이너 안에 등록된 객체들(클래스 옆에 초록색 아이콘 붙은 클래스들!)
- IoC 컨테이너 내부에서 OwnerController 객체를 만들어주고, OwnerRepository 타입의 객체도 만들어 준다.
- OwnerController, OwnerReposiotry는 모두 ApplicationContext 내에서 만들어지는 빈이다.

- 의존성 주입은 Bean끼리만 가능하다.

```java
@Autowired
ApplicationContext applicationContext;

@Test
public void getBeanTest() {
    OwnerRepository repository = applicationContext.getBean(OwnerRepository.class);
    Assertions.assertNotNull(repository);
}
```

```java
private final ApplicationContext applicationContext;

public OwnerController(OwnerRepository owners, VisitRepository visits, ApplicationContext applicationContext) {
    this.owners = owners;
    this.visits = visits;
    this.applicationContext = applicationContext;
}

@GetMapping("/bean")
@ResponseBody
public String bean() {
    return "bean : " + applicationContext.getBean(OwnerController.class);
}

```

- Bean으로 관리하는 객체는 매번 새로 생성되지 않고, 처음에 만들어둔 객체를 재사용하게 된다.
- 이런 식으로 ApplicationContext나 getBean()을 직접 사용할 일은 거의 없다.

### 빈(Bean)
- Spring IoC 컨테이너가 관리하는 객체

#### 등록하는 방법
방법1) Component Scanning : Spring IoC 컨테이너가 IoC 컨테이너를 만들고 그 안에 빈을 등록할때 사용하는 인터페이스들을 라이프 사이클 콜백이라고 부른다.
여러 라이프사이클 콜백 중 컴포넌트 어노테이션을 찾아서, 해당 클래스의 인스턴스를 Bean으로 등록하는 일을 한다.
- @ComponentScan : 어디부터 컴포넌트를 찾아볼 것인지 알려주는 역할. @SpringBootApplication 어노테이션이 붙어있는 클래스부터 시작해 하위 클래스를 모두 찾아보게 된다.
```java
@ComponentScan(
  excludeFilters = {@Filter(
  type = FilterType.CUSTOM,
  classes = {TypeExcludeFilter.class}
), @Filter(
  type = FilterType.CUSTOM,
  classes = {AutoConfigurationExcludeFilter.class}
)}
)
```
- @Component
    - @Repository, @Service, @Controller, @Configuration
    - Repository는 약간 특이 케이스인데, JPA의 기능에 의해 등록된다. 어노테이션이 없어도 Repository 인터페이스를 상속 받으면 그 구현체를 Bean으로 등록한다.

방법2) xml이나 자바 설정 파일에 직접 등록 
두가지 자바 설정 파일을 작성하는 방식이 더 많이 쓰이는 추세다.
@Configuration 어노테이션을 붙인 클래스를 만들고, 그 안에서 @Bean을 사용해 직접 Bean을 정의한다.

```java
@Configuration
public class SampleConfig {

    @Bean
    public SampleController sampleController() {
        return new SampleController;
    }
}
```
@Configuration 어노테이션이 컴포넌트 스캐닝할 때 읽히게 되고, 안에서 정의한 bean들이 IoC 컨테이너에 정의된다.



### 의존성 주입 (Dependency Injection)
- @Autowired : 생성자, setter, 필드에 붙일 수 있다. Spring 5 이후로는, 매개변수가 하나뿐인 생성자라면 어노테이션을 생략해도 DI가 자동으로 적용된다.
Spring에서 권장하는 위치는 생성자. 필수적으로 생성해야 하는 레퍼런스 없이는 해당 클래스의 인스턴스 생성 자체가 불가능.
순환 참조/상호 참조하는 의존성 문제가 발생한다면 필드나 setter에 붙이면 됨. 물론 가급적 이런 일이 발생하지 않게 하는 것이 좋다.
필드에 붙일 경우 final과는 함께 사용할 수 없다.

