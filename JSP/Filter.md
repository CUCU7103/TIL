# Filter

- Filter는 HTTP 요청과 응답을 변경할 수 있는 재사용 가능한 클래스이다. 

- Filter는 객체의 형태로 존재하며 클라이언트에서 오는 요청과 최종 자원사이에 위치하여 

  클라이언트로 부터 들어오는 요청이나 서블릿이 처리한 응답을 각각 전달하기전에 특정 작업을 수행하는 재사용 가능한 코드 입니다.

- 클라이언트와 자원사이에 한 개의 필터만 존재할 수 있는 것은 아니며 여러 개의 필터가 모여 필터 체인을 형성할 수 도 있습니다.



### 필터의 요청 흐름

![image](https://github.com/user-attachments/assets/257604f2-d367-46d6-8e96-766bb12aa0ca)


### 필터의 구현

- Filter 인터페이스

  

코드 예시

``` java
public class FirstFilter implements Filter {
	
    public init(FilterConfig filterConfig) throws ServletException{
        // 필터 초기화 작업
     }

    public void doFilter(ServletRequest request, ServletRespone response,FilterChain chain)
    throws IOException, ServletException{
        // 1. request 파라미터를 이용하여 요청의 필터작업을 수행함
        ....
        // 2. 체인의 다음 필터 처리
            ....
        chain.doFilter() // 처리하지 않으면 다음필터로 넘어가지 않음
            ....
        // 3. response를 이용하여 응답의 필터링 작업을 수행합니다.
         ...
    }
    
    public void destory(){
        // 주로 필터가 사용한 자원을 반납합니다.
    }
    
}
```

필터는 다음의 3가지 메서드로 구성된다.

- `init()` 
  - 필터 초기화 메서드, 서블릿 컨테이너가 생성될 때 호출된다.
- `doFilter()` 
  - 필터 기능을 수행합니다. chain을 이용해서 체인의 다음 필터로 처리를 전달할 수 있습니다.
  - doFilter() 의 세 번째 파라미터로 FilterChain 객체를 전달받는다 이는 클라이어트의 요청이 거쳐가게 되는 필터 체인을 의미합니다.
  - FilteChain을 사용해서 필터는 체인에 있는 다른 필터에 변경한 요청과 응답을 전달할 수 있습니다.
- `destroy()` 
  - 필터 종료 메서드, 서블릿 컨테이너가 종료될 때 호출된다.





### 필터 설정하기

- Filter를 설정하는 방법
  - web.xml 이용하기
  - @WebFilter 이용하기 



#### 필터의 요청 및 응답 클래스

- 필터가 필터로써의 제 역할을 하려면 클라이언트의 요청을 변경하고, 클라이언트로 가는 응답을 변경할 수 있어야 합니다.

- Wrapper 클래스를 통해 다음과 같은 역할을 할 수 있습니다.

  - 요청 정보를 변경하여 최종 자원인 서블릿/jsp/HTML 기타자원에 전달합니다.

  - 최종 자원으로부터의 응답을 변경하여 새로운 응답정보를 클라이언트에 보냅니다.

  

  - HttpServletRequestWrapper
    - HttpServletRequest 인터페이스를 구현하는 래퍼 클래스입니다
    - HTTP 프로토콜에 특화된 요청 처리 기능을 제공합니다
    - ServletRequestWrapper를 상속받아 HTTP 관련 추가 기능을 확장합니다

  

  - HttpServletResponseWrapper
    - HttpServletResponse 인터페이스를 구현하는 래퍼 클래스입니다
    - 서블릿의 HTTP 응답을 수정하거나 확장할 때 사용됩니다
    - ServletResponseWrapper를 상속받아 HTTP 관련 추가 기능을 제공합니다



### 필터의 응용기능들

- 사용자 인증 
- 캐싱 피터
- 자원 접근에 대한 로깅
- 응답 데이터 변환(HTML 변환, 응답헤더 변환 등)
- 공통기능 실행

































