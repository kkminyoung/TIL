# AOC(Aspect Oriented Programming)

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
AOP가 어떻게 동작하는지 알아보기 위해, 실행 시간을 측정하는 어노테이션 @LogExecutionTime을 만들어보자.


#### 1. 인터페이스 생성
```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface LogExecutionTime {

}
```

#### 2. 어노테이션이 붙은 scope에서 실행할 코드 작성
```java
@Component    // bean으로 등록
@Aspect
public class LogAspect {

    Logger logger = LoggerFactory.getLogger(LogAspect.class);

    /**
     * @param joinPoint annotation이 실행되는 target
     */
    @Around("@annotation(LogExecutionTime)")
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
