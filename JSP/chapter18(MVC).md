# MVC 패턴

### MVC 패턴이란?

- MVC 는 Model, View, Controller의 약자입니다.

  - 하나의 애플리케이션, 프로젝트를 구성할 때 그 구성요소를 세가지의 역할로 구분한 패턴입니다.

  - 패턴의 핵심은 소프트웨어의 비즈니스 로직과 화면을 구분하는데 중점을 두고있습니다.

    - 이러한 "관심사 분리" 는 더 나은 업무의 분리와 향상된 관리를 제공합니다.

    - **즉 관심사의 분리에 의해 각각의 책임이 명확해지고 유지보수가 쉬워집니다.**

![image](https://github.com/user-attachments/assets/069e950f-368b-4574-bef7-1d15be8ab3e6)

### **MVC 패턴의 동작과정**

- **모델(Model)** 
  - 애플리케이션의 핵심적인 **비지니스 로직을 담당**합니다. 
  - 데이터 베이스와 직접적으로 연결되어 데이터를 처리하고, 사용자가 요청하는 데이터 연산을 수행합니다.
- **뷰(View)**
  - 사용자에게 **보여지는 부분을 담당**합니다. 
  - 사용자가 보는 화면을 구성하고, 사용자로부터 입력을 받습니다.
- **컨트롤러(Controller)**
  - 사용자의 **요청을 처리하고, 요청에 따라 모델과 뷰를 업데이트**합니다. 
  - 사용자의 요청을 모델로 전달하여 데이터를 처리하고 그 결과를 뷰로 전달하여 사용자에게 보여줍니다.



### Servlet Container

- Servlet Container란 특정 기능(회원가입 등)의 처리를 담당하는 Web Component(Java에서는 Servlet)들의 라이프사이클을 관리한다

- Request(웹 요청)가 왔을 때, 이 Servlet Container가 어떤 Servlet(Web Component)에게 처리를 위임할지 결정하는 Routing 역할까지 수행

  

### DispatcherServlet의 사용이유

- 초기의 Spring MVC는 모든 서블릿을 URL 매핑을 위해, web.xml이라는 파일에 전부 등록해주어야 했습니다.
  - Servlet을 계속 추가하는 방식을 이용하면 애플리케이션 입장에선 각각의 요청마다 웹 요청/응답을 위해 Request/Response Object를 매번 다뤄야 하는 방식이기 때문에 한계가 있습니다.
- Spring에서는  'DispatcherServlet'이라는 서블릿 클래스가 존재합니다.
- DispatcherServlet이 해당 어플리케이션으로 들어오는 모든 요청을 핸들링해주고, 공통 작업을 처리하면서 URL 매핑 과정이 유연해지는 장점을 얻을 수있습니다.

- 이 오브젝트 덕분에 우리는 일일이 Request 정보를 분석하고 바인딩하는 코드를 작성하지 않아도 됩니다.

![image-20241208145854882](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20241208145854882.png)



## Dispatcher Servlet이란?

- Dispatch + Servlet
- HTTP 요청을 중앙에서 처리하는 **프론트 컨트롤러** 역할
- Dispatcher Servlet은 클라이언트로부터 오는 모든 요청을 가로채서 **공통으로 관리합니**다.

- 이후 적절한 **핸들러에게 요청을 위임**하고, 해당 핸들러의 **실행 결과를 응답 형태로 반환**합니다.

![image](https://github.com/user-attachments/assets/e43961f1-487d-4772-8226-d61e6272de75)


### 핸들러 (Handler)

- 일반적으로 컨트롤러 자체를 의미합니다. 즉, 요청을 처리할 수 있는 객체(컨트롤러 클래스의 인스턴스)가 핸들러로 매핑됩니다.

<br>

**클라이언트 요청 수신**

- 사용자가 웹 브라우저를 통해 특정 URL로 요청을 보내면, 이 요청은 서블릿 컨테이너(예: Tomcat)에 의해 DispatcherServlet으로 전달됩니다.

**핸들러 매핑 진행**

- DispatcherServlet은 `HandlerMapping`을 사용하여 들어온 요청 URL과 일치하는 컨트롤러(핸들러)를 찾습니다.
- Spring은 여러 종류의 `HandlerMapping` 구현체(예: `RequestMappingHandlerMapping`)를 통해 요청과 적합한 핸들러를 매핑합니다.

**핸들러 어댑터를 통한 핸들러 실행**

- DispatcherServlet은 `HandlerAdapter`를 사용하여 찾은 핸들러(컨트롤러)를 실행합니다.
- 핸들러 어댑터는 핸들러가 어떤 타입이든 상관없이 일관된 방식으로 실행할 수 있게 해줍니다.
- 이 과정에서 컨트롤러의 메소드가 호출되고, 비즈니스 로직이 수행됩니다.

**모델 및 뷰 생성**

- 컨트롤러는 요청 처리 결과를 `ModelAndView` 객체로 반환합니다. 이 객체에는 모델 데이터와 뷰 이름이 포함됩니다.
- 모델 데이터는 뷰에서 표시할 데이터(예: JSP, Thymeleaf 템플릿 등)를 담고 있습니다.

**뷰 리졸버를 통한 뷰 선택**

- DispatcherServlet은 `ViewResolver`를 사용하여 `ModelAndView`에서 지정한 뷰 이름을 실제 뷰 객체로 변환합니다.
- 예를 들어, 뷰 리졸버는 논리적인 뷰 이름을 기반으로 실제 JSP 파일의 경로를 찾아냅니다.

**뷰 렌더링**

- 선택된 뷰는 모델 데이터를 기반으로 최종 HTML 페이지를 생성합니다.
- 생성된 HTML은 클라이언트에게 응답으로 전송됩니다.

**응답 반환**

- DispatcherServlet은 완성된 뷰를 클라이언트에게 반환하여 브라우저에 표시되도록 합니다.



