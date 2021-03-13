# Spring MVC Request Life Cycle


### 0. 전체구조

![image](https://user-images.githubusercontent.com/61506233/111024158-7d3e8000-8420-11eb-9e62-3fde778bbec4.png)

### 1. Filter
- 웹 애플리케이션의 전역적인 로직을 담당하며, Dipatcher Servlet에 들어가기 전인 웹 애플리케이션 단에서 실행된다.


### 2. DispatcherServlet
- 들어오는 모든 Request를 우선적으로 받아 처리해주는 서블릿이다.

#### 과정
```
- HandlerMapping이 Request에 적절한 Controller 정보를 전달한다.
- HandlerMapping으로 부터 받은 Controller 정보를 통해 해당하는 Controller와 매핑시킨다.

* Dispatcher = 배치 담당자라는 뜻, 즉 Request를 어느 Controller와 매핑시킬 것인지 배치하는 역할을 수행한다.
```

### 3. HandlerMapping
- DispatcherServlet로 부터 검색을 요청받은 Controller를 찾아 정보를 반환한다.


### 4. HandlerAdapter
- Request가 Controller에 매핑되기 전에 앞단에서 부가적인 로직을 수행한다.
- 주로 세션, 쿠키, 권한 인증 로직에 많이 사용된다.


### 5. Controller
- Request와 매핑되는 곳이며, Request를 어떤 로직으로 처리할 것인지를 결정하고 그에 맞는 Service를 호출한다.
- Service의 메소드를 호출해야 하기 때문에, Spring Container로 부터 Service Bean을 주입받아야 한다.


### 6. Service
- 데이터 처리 및 가공을 위한 비즈니스 로직을 수행한다.
- Request에 대한 실질적인 로직을 수행하며, Repository를 통해 DB에 접근하고 데이터의 CRUD를 처리한다.

### 7. Repository
- DB에 접근하는 객체이며, DAO (Data Access Object) 라고 부른다.
- Service에서 DB에 접근할 수 있게 하여 데이터의 CRUD를 수행할 수 있게 해준다.


### 8. ViewResolver
- Controller에서 반환한 View의 이름을 DispatcherServlet으로 부터 넘겨받고 해당 View를 렌더링한다.
- ViewResolver가 렌더링한 View를 DispatcherServlet에게 전달하고, DispatcherServlet은 해당 View 화면을 Response한다.
