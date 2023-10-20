2023-10-20
### Spring MVC 특징

![ojIvGml.png](https://i.imgur.com/ojIvGml.png)

스프링은 다양한 기능들을 제공하는데, 그 중에서 웹 개발에 중점을 둔 부분이 스프링 MVC에요. 이를 간단하게 정리하면 
1. 스프링은 DI와 AOP와 같은 기능 외에도 웹 개발을 위한 MVC 프레임워크를 제공하고 있어요.
2. Model2Architecture와 FrontController 패턴을 사용하여 웹 애플리케이션의 흐름과 구조를 잘 정리할 수 있게 도와줘요.
3. 스프링 MVC는 스프링의 핵심 기능 위에 구축되었기 때문에, 스프링이 제공하는 다양한 기능들 (예: 트랜잭션 처리, DI, AOP 등)을 쉽게 활용할 수 있어요.
이렇게 스프링 MVC는 스프링의 강력한 기능들을 웹 개발에 효과적으로 활용할 수 있게 해줘요.

### Spring MVC 용어
1. **DispatcherServlet (Front Controller)** :
    -  클라이언트로부터의 모든 요청을 첫 번째로 받아들이는 진입점이에요.
    -  해당 요청을 알맞은 컨트롤러에 전달하고, 컨트롤러로부터 받은 응답을 뷰에 넘겨서 최종적인 응답을 생성하게 도와줘요.
2. **HandlerMapping** :
    -  클라이언트의 요청 URL을 기반으로 어떤 컨트롤러가 해당 요청을 처리할지를 결정해줘요.
3. **Controller** (개발자가 직접 만들어요!):
    -  클라이언트의 요청을 실제로 처리하고, 필요한 경우 모델과 상호작용한 후 그 결과를 DispatcherServlet에게 반환해줘요.
4. **ModelAndView** :
    - 컨트롤러가 처리한 결과 데이터와 그 데이터를 표현할 뷰에 관한 정보를 함께 담고 있어요.
    - `Model`은 데이터를 나타내며, `View`는 어떤 뷰(예: JSP)를 사용하여 결과를 표현할지를 나타내는 부분이에요.
```
	@GetMapping("/hello")
	public ModelAndView hello(Locale locale, Model model) {
		ModelAndView mav = new ModelAndView();
		mav.addObject("msg", "test");
		mav.setViewName("index");
		return mav;
	}
```
1. **ViewResolver** :
    - 컨트롤러가 리턴한 뷰 이름을 기반으로 실제 뷰를 찾아주는 역할을 해요.
    - 예를 들면, 뷰 이름이 "home"이면 "home.jsp"와 같은 실제 뷰 파일을 찾아줘요.
    - Servlet에서 foward와 같은 것이라 생각하면 되요.
2. **View** :
    - 클라이언트에게 보여질 최종 결과물을 생성하는 역할이에요.
    - JSP, Thymeleaf, FreeMarker 등 다양한 뷰 기술을 사용할 수 있어요. `View`는 `Model`에 담긴 데이터를 사용하여 클라이언트에게 보여질 응답을 생성하게 돼요.

알아두면 좋은 내용!
``` 
Spring Web MVC에서의 `DispatcherServlet`과 같은 주요 컴포넌트들은 기본적으로 싱글턴(Singleton) 스코프로 Spring 컨테이너에서 관리되고 있어요. 이는 동시에 많은 사용자의 요청을 효율적으로 처리하기 위해서입니다!
```

### @Controller 의 반환 값 
1. **String**:
    - 대부분 뷰의 이름을 나타냅니다. `ViewResolver`가 이 이름을 기반으로 실제로 보여질 JSP나 템플릿을 찾아서 처리하게 됩니다.
2. **ModelAndView**:
    - 뷰와 모델을 함께 포함하는 객체입니다. 컨트롤러에서 처리한 데이터를 모델에 넣고, 해당 데이터와 함께 보여질 뷰를 지정할 수 있습니다.
3. **void**:
    - 컨트롤러의 메서드가 `void`를 반환한다면, 요청 URI를 기반으로 뷰를 선택합니다. 예를 들어, "/home" 요청에 대한 `void` 반환 메서드는 "home" 뷰를 응답합니다.
4. **ResponseEntity**:
    - 응답 데이터와 함께 HTTP 상태 코드를 직접 지정하고 싶을 때 사용합니다. RESTful 웹 서비스를 만들 때 특히 유용하죠.
5. **HttpHeaders**:
    - 응답 헤더만 반환할 때 사용합니다.
6. **Callable, DeferredResult**:
    - 비동기 요청 처리를 위한 반환 타입들입니다.
7. **@ResponseBody**:
    - 메서드 위에 `@ResponseBody`를 추가하면 반환 값은 뷰를 찾는 것이 아니라 응답 바디로 직접 쓰여집니다. 주로 JSON이나 XML 같은 데이터를 반환할 때 사용하죠.
8. **RedirectAttributes**:
    - 리다이렉트 시 데이터를 전달하기 위해 사용됩니다.
9. @ModelAttribute("info")

### 참고 : Spring의 라이프 사이클

스프링의 라이프 사이클은 어플리케이션의 시작부터 종료까지의 여러 단계로 구성되어 있어요. 주요 단계들은 아래와 같아요:

1. **Bean 정의**: XML, 자바 설정, 애노테이션 등을 통해 빈(Bean)을 정의해요.
2. **Bean 초기화**: ApplicationContext가 생성되며 빈들이 초기화되어요.
3. **Bean 사용**: 애플리케이션은 필요에 따라 빈을 사용해요.
4. **Bean 소멸**: ApplicationContext가 종료될 때 빈의 생명주기도 끝나게 되요.

이런 단계들을 거치며, 스프링은 개발자가 객체의 생명주기나 의존성 주입과 같은 작업에 집중하지 않고 핵심 비즈니스 로직에만 집중할 수 있게 도와줘요.