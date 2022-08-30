# IoC (Inversion of Control)
## 제어 역전, IoC(Inversion of Control)
- 의존성에 대한 제어권이 뒤바뀜
- 메소드나 객체의 호출 작업을 개발자가 결정하는 것이 아니라 외부에서 결정되는 것
- 객체 간의 결합도를 줄임

</br>

### 일반적인 (의존성에 대한) 제어권 
: 자기자신
```java
class OwnerController {
    private OwnerRepository repository = new OwnerRepository();
}
```

### IoC
: "내가 사용할 의존성을 누군가 알아서 주겠지"하고 가정하고 레포지토리 코드를 사용한다.
- 내가 사용할 의존성의 타입(또는 인터페이스)만 맞으면 어떤 것이든 상관없다.
```java
class OwnerController {
    private OwnerRepository repo;

    public OwnerContorller(OwnerRepository repo) {
        this.repo = repo;
    }
}
```
```java
class OwnerControllerTest {
    @Test
    public void create() {
        OwnerRepository repo = new OwnerRepository();
        OwnerController controller = new OwnerController(repo);
    }
}
```

</br>

## IoC 컨테이너
ApplicationContext (BeanFactory)
- 컨테이너 내부에 만든 객체들(Bean)을 만들고 의존성을 관리해 준다.(엮어준다)

빈(bean) 설정
- 이름 또는 ID
- 타입
- 스코프

하지만 컨테이너를 직접 쓸 일은 많지 않다.

</br>

## Bean
- spring IoC 컨테이너가 관리하는 객체
- 의존성 관리 용이
- 빈(bean)으로 등록된 객체는 기본적으로 스코프가 '싱글톤'으로 정해짐

### 빈(bean) 등록 방법
- Component Scanning
   - @Component
        - @Repository
        - @Service
        - @Controller
- 직접 등록
    ```java
    @Configuration
    public class {

        @Bean
        public String jonnygim() {
            return "jonnygim";
        }

    }
    ```
</br>

### 사용 방법
- @Autowired 또는 @Inject
- ApplicationContext 에서 getBean()으로 직접 꺼내기

</br></br>

# DI (Dependency Injection)
## 의존성 주입, DI(Dependency Injection)

- 제어의 역행이 일어날 때 스프링이 내부에 있는 객체들 간의 관계를 관리할 때 사용하는 기법
- 일종의 IoC 라고 볼 수 있음
- 필요한 의존성을 어떻게 받아올 것인가
- 빈(bean)끼리만 가능 (IoC 컨테이너 안에 들어있는 객체들 끼리만)

### 이점
- Unit Test 가 용이해짐
- 코드의 재활용성이 높아짐
- 객체 간의 의존성을 줄임
- 객체 간의 결합도를 낮추면서 유연한 코드 작성 가능

</br>

## @Autowired / @Inject 사용 위치
- 생성자 (권장)
    - 빈(bean)이 되는 클래스에 생성자가 하나만 있고, 그 매개변수 타입이 빈으로 등록되어 있다면 빈으로 등록(@Autowired 가 없어도)
    ```java
    public OwnerController(OwnerRepository clinicService) {
        this.owners = clinicService;
    }
    ```
- 필드
    ```java
    @Autowired
    private OwnerRepository owners;
    ```
- Setter
    ```java
    @Autowired
    public void setOwners(OwnerRepository owners) {
        this.owners = owners;
    }
    ```

<br>

### 생성자 injection 권장 이유
- 필수적으로 사용해야 하는 레퍼런스 없이는 인스턴스를 만들지 못하게 강제할 수 있음


