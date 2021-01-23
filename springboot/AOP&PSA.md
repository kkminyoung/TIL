# AOP(Aspect Oriented Programming)

- 흩어진 코드를 한 곳으로 모아!
- 여러 메서드에서 공통적으로 해야하는 일의 코드가 중복된다면, 따로 모아서 재활용하는 것.


ex) 흩어진 코드
```java
class A {

  void method a(){ 
    AAAA 
    오늘은 7월 4일 미국 독립 기념일이래요.
    BBBB 
  }

  void method b(){ 
    AAAA 
    저는 아침에 운동을 다녀와서 밥먹고 빨래를 했습니다.
    BBBB 
  }

}

class B { 
  void method c(){ 
    AAAA 
    점심은 이거 찍느라 못먹었는데 저녁엔 제육볶음을 먹고 싶네요. 
    BBBB
	}
}
```

ex) 모아진코드
```java
class A{
  void method a(){
    오늘은 7월 4일 미국 독립 기념일이래요.
  }
  void method b(){
    저는 아침에 운동을 다녀와서 밥먹고 빨래를 했습니다.
  }
}
class B{
  void method c(){
    점심은 이거 찍느라 못먹었는데 저녁엔 제육볶음을 먹고 싶네요. 
  }
}

class AAABBB{
  void method aaabbb(JoinPoint point){
    AAAA
    point.execute()
    bbbb
  }
}
```

- @Transctional이 AOP 기반으로 만들어진 어노테이션이다.
- AOP는 코드가 없는데도 코드가 있는 것처럼 해준다.

```java
@GetMapping("/owners/{ownerId}/edit")
public String initUpdateOwnerForm(@PathVariable("ownerId") int ownerId, Model model) {
        Owner owner = this.owners.findById(ownerId);
        model.addAttribute(owner);
        return VIEWS_OWNER_CREATE_OR_UPDATE_FORM;
}

```

AOP를 이용해 개발자가 작성하지 않았어도 특정한 코드가 삽입되는 듯한 효과를 낼 수 있다.

```java
@GetMapping("/owners/{ownerId}/edit")
public String initUpdateOwnerForm(@PathVariable("ownerId") int ownerId, Model model) {
        // 개발자가 작성하지 않은 코드가 삽입
        AAA...
        BBB...

        Owner owner = this.owners.findById(ownerId);
        model.addAttribute(owner);
        return VIEWS_OWNER_CREATE_OR_UPDATE_FORM;
}
```

### 구현 방법
기존 코드를 건들지 않고 새 기능을 추가하는 방법 세가지

1. 컴파일
2. 바이트코드 조작 : 클래스 로더가 클래스를 읽어오는 단계에서(.java를 .class로) 조작하는 것
3. 프록시 패턴 : 스프링 AOP가 사용하는 방법



### Example) 
실행 시간을 측정하는 어노테이션 @LogExecutionTime을 만들어보자!


#### 1. 인터페이스 생성
```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME) // 애노테이션 사용 유지 기간
public @interface LogExecutionTime {

}
```

```java
public class OwnerController{
	@LogExecutionTime
  @GetMapping("/find"){
    return "find"
  }
}
```

#### 2. 어노테이션이 붙은 scope에서 실행할 코드 작성
```java
@Component    // bean으로 등록
@Aspect
public class LogAspect {

    Logger logger = LoggerFactory.getLogger(LogAspect.class);


    // @param joinPoint annotation이 실행되는 target

    @Around("@annotation(LogExecutionTime)") //@Around : 해당하는 어노테이션 주변으로 이런 일을 해라
    public Object logExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable {
        StopWatch stopWatch = new StopWatch();
        stopWatch.start();

        Object proceed = joinPoint.proceed();

        stopWatch.stop();
        logger.info(stopWatch.prettyPrint());

        return proceed;
    }
}
```
- 어노테이션은 그저 마커이다. 어딘가에서 트랜잭션을 처리해주고, 우리는 어노테이션을 붙이는 것만으로도 처리를 할 수 있게된다.

# PSA(Portable Service Abstraction)
Service Abstraction으로 제공되는 기술을 다른 기술 스택으로 간편하게 바꿀 수 있는 확장성을 갖고 있는 것

나의 코드 : 확장성이 좋은 코드 or 기술에 특화되어 있는 코드

기존 코드를 거의 변경하지 않아도 사용 기술을 간단하게 바꿀 수 있다.

스프링은 서블릿을 사용하는 프로그램인데 서블릿을 사용하지 않고있다. 그 대신 @GetMapping이나 @PostMapping을 통해 특정 url로 요청이 들어왔을 때, 해당 블록이 요청을 처리하도록 구현 되어있다. 이렇게 추상화 계층을 사용해 어떤 기술을 내부에 숨기고 개발자에게 편의성을 제공하는 것을 Service Abstraction이라고 한다.

Spring Web MVC, Spring Transaction, Spring Cache 등이 모두 Portable Service Abstraction에 해당된다.
스프링은 MVC라는 추상화 기법을 사용. Spring Web MVC를 사용하면 서블릿을 low level로 직접 구현할 필요가 없어진다.
- View : templates
- Model : Repository
- Controller : Controller

스프링은 원래 Tomcat 기반으로 돌아가는데, dependency에서 web을 webflux로 바꾸고 다시 실행해보면 Netty 기반으로 돌아간다. 스프링의 PSA 덕분에 코드를 거의 바꾸지 않고도 톰캣이 아닌 완전히 다른 기술로 실행이 가능하다는 의미이다.

- 스프링 트랜잭션 (Atomic한 작업을 트랜잭션이라 부름)
    - commit, rollback을 명시적으로 호출하지 않아도, 어노테이션만 붙이면 트랙잭션 처리가 이루어짐
