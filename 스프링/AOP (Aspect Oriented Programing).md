2023-10-19
### AOP
관점 지향 프로그래밍 : 공통된 관심사를 중심으로 코드를 몰아 넣는다고 생각하면 좋아요.

![](https://i.imgur.com/kH6HwuB.jpg)

핵심 관심 사항
BankingService, AccountService처럼 핵심이 되는 관심 사항
공통된 관심사
1. 간단한 메소드 성능 검사 -> 로깅
2. 트랜잭션 처리 -> JDBC를 쓸때 try catch 지옥... 어지러웠죠?
3. 예외 반환 등
모든 핵심 관심 사항에 로그를 추가하는 것은 각각을 따로 처리하는 것이 번거롭죠?
-> 그래서 한번에 처리하는 방법이 필요하게 되었어요.
### 2. Spring AOP
1. **Filter** : 웹 애플리케이션에서 HTTP 요청이나 응답을 조절할 때 쓰여요. 주로 WAS에서 HTTP 요청을 서블릿이나 컨트롤러로 보내기 전이나 후에 작업을 하기 위해 사용해요.

3. **Interceptor** : Spring에서 제공하는 기능으로, 요청을 처리하기 전이나 후, 또는 응답을 보내기 전에 특별한 작업을 하기 위해 쓰여요. 로깅이나 권한 확인, 트랜잭션 처리 같은 일에 주로 사용돼요.

5. **SpringAOP** : 특별한 범위나 순서를 우리마음대로 정하고 싶을 때 사용해요. 복잡한 프로그래밍에서 관심사를 분리해서 코드의 중복을 줄이고, 유지 보수를 용이하게 만들기 위한 목적도 있어요. 주로 로깅, 트랜잭션 관리, 보안 등의 기능을 횡단 관심사로 처리하는 데에 활용돼요.
	-> Filter와 Interceptor가 AOP의 개념을 사용하고 있기 때문에 비슷해 보일 수 밖에 없어요.

AOP의 구현 방식에는 **JDK Dynamic Proxy**와 **CGLIB**가 주로 쓰여요.
- **JDK Dynamic Proxy**
    - **장점**
        - 표준 Java API를 사용하기 때문에 별도의 의존성을 필요로하지 않아요.
        - 인터페이스 기반으로 동작하므로 명확한 계약을 제공해요.
    - **단점**
        - 프록시를 생성하려면 반드시 인터페이스가 필요해요. 따라서 인터페이스가 없는 클래스는 프록시로 만들 수 없어요.
        - 인터페이스를 변경할 때마다 관련 프록시도 수정해야 할 수도 있어요.
- **CGLIB** (Code Generation Library)
    - **장점**
        - 인터페이스 없이 클래스를 바탕으로 프록시를 생성할 수 있어요.
        - 성능 측면에서 JDK Dynamic Proxy보다 더 빠를 때가 있어요.
    - **단점**
        - 바이트 코드를 조작하는 방식이기 때문에 별도의 라이브러리 의존성이 필요하며, 이로 인한 문제가 발생할 수도 있어요.
        - 내부적인 동작 방식이 복잡해지기 때문에 디버깅이 어려울 수 있어요.

**CGLIB**이 **JDK Dynamic Proxy**보다 때때로 더 빠르게 동작하는 이유는 무엇일까요?
1. **바이트 코드 조작**: CGLIB는 바이트 코드를 조작해서 프록시 클래스를 만들어요. 이렇게 만든 프록시는 원본 클래스의 메소드를 호출할 때와 비슷한 속도로 동작해요.
2. **메소드 호출 오버헤드**: JDK Dynamic Proxy는 `invoke` 메소드를 통해 메소드 호출을 처리하게 돼요. 이 과정에서 약간의 오버헤드가 생길 수 있어요. 반면 CGLIB는 바이트 코드를 조작해서 메소드 호출을 직접 처리하기 때문에, 오버헤드가 적어져요.
3. **캐싱**: CGLIB는 만든 프록시 클래스를 캐싱해서 다시 사용해요. 그래서 여러 번 프록시 객체를 생성할 때 시간을 아낄 수 있어요.
4. **초기화 비용**: 처음 CGLIB로 프록시 클래스를 생성할 때는 조금 더 오래 걸릴 수 있어요. 그렇지만 이후 메소드 호출은 보통 빨라요. 반면 JDK Dynamic Proxy는 처음부터 빠를 수 있지만, 계속 사용하다 보면 CGLIB보다 느려질 수도 있어요.

하지만 이런 성능 차이는 어떤 상황이냐에 따라 달라져요. 따라서 어떤 것이 더 나은지 알아보려면 직접 테스트를 해봐야 해요.

### AOP 용어
1. **Target**: 실제 비즈니스 로직을 담당하는 부분이에요. AOP에서는 이 Target에 부가기능을 추가합니다.
2. **Advice**: 공통 기능을 어떻게, 어느 시점에 적용할지 정의한 모듈이에요. Before, After, Around, AfterReturning, AfterThrowing 등의 다양한 종류의 Advice가 있어요.
3. **JoinPoint**: 프로그램 실행 중에 Aspect의 코드가 적용될 수 있는 특정한 지점을 의미해요. 예를 들면, 메소드 호출이나 예외 발생 시점이 JoinPoint가 될 수 있죠.
4. **PointCut**: 실제로 Advice가 적용될 JoinPoint를 세밀하게 지정한 표현식이에요.
5. **Weaving**: Pointcut에 지정된 위치에 실제로 Advice를 적용하는 과정을 의미해요. 컴파일 시점, 클래스 로드 시점, 런타임 시점 등 다양한 시점에서 Weaving이 일어날 수 있어요.
6. **Aspect**: 공통 기능 모듈 자체를 의미하는데, Advice와 Pointcut을 합친 것이죠. 즉, 어떤 공통 기능을 어디에 적용할 것인지에 대한 정보를 담고 있어요.
### Pointcut 표현식
1. `execution(public * *(..))` - 모든 public 메소드 실행에 적용
2. `execution(* set*(..))` - 이름이 `set`으로 시작하는 모든 메소드 실행에 적용
3. `execution(* com.test.service.AccountService.*(..))` - `AccountService` 클래스의 모든 메소드 실행에 적용
4. `execution(* com.test.service.*.*(..))` - `service` 패키지의 모든 메소드 실행에 적용
5. `execution(* com.test.service..*.*(..))` - `service` 패키지와 그 하위 패키지의 모든 메소드 실행에 적용
6. `within(com.test.service.*)` - `service` 패키지 내의 모든 연결점(JoinPoint)에 적용
7. `within(com.test.service..*)` - `service` 패키지 및 그 하위 패키지의 모든 연결점에 적용
8. `this(com.test.service.AccountService)` - `AccountService` 인터페이스를 구현하는 대상 객체의 연결점에 적용
9. `target(com.test.service.AccountService)` - `AccountService` 인터페이스를 구현하는 대상 객체의 연결점에 적용
10. `args(java.io.Serializable)` - 인수로 `Serializable` 인터페이스를 구현하는 객체를 받는 연결점에 적용
```
@Before("execution(* com.example.service.*.*(java.lang.String, int))")
```

### Pointcut 조합
두 개 이상의 Pointcut 표현식을 조합할 수 있어요. 주로 `&&` (and), `||` (or), `!` (not) 연산자를 사용해서 조합해요.
예시:
- `execution(* com.example.service.*.*(..)) && args(String)`: `com.example.service` 패키지의 모든 클래스의 메소드 중, String 타입의 인자를 받는 메소드에만 조언을 적용하겠다는 의미에요.