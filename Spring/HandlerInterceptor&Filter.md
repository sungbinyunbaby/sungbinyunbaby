1. HandlerInterceptor(Interceptor)
    Spring Framwork의 일부분.
    요청을 가로챈다.
    DispatcherServlet이 해석을 해서 넘겨주기 전에 가로채고,
    controller와 model이 응답을 주고, 그 응답이 전달되기 전에 가로채는 역할을 한다.
    Handler에 도달하기 전에 가로챈다는 의미에서 HandlerInterceptor이라고 한다.

    Configuration을 이용해 어떤 경로에 대해서 동작을 시킬 것인지 등록을 해서 사용하는 것이 HandlerInterceptor.
    (Configuration이 경로를 확인하고 어떤 HandlerMapping 앞에 HandlerInterceptor를 붙일 수 있을까를 정할 수 있다.) 
    어떤 시점에 등록을 할지 어떤 시점에 등록을 안 할지 결정을 할 수 있다.
    =>비지니스 로직과 연관성이 높은 기능 구현.(어떤 행동을 할 것이다를 명확히 하는 곳.)


2. Filter
    springboot의 기능이 아니라 Servlet API의 기능 즉, 자가가 제공하는 자바 웹서버의 기능이다.
    모든 요청과 응답을 처리하는 과정에서 요청과 응답을 검사해주는 역할.

    어떤 경로 이런거 없이 모든것이 요청이고 모든것이 응답이다.
    스프링 앞에 위치해 있어서 (톰캣같이) 스프링의 기능을 사용하기 어렵다.
    실질적인 비지니스 로직과는 상관이 없는 부분들
    보안, 인코딩을 다루는 곳.
    인증 인과 ->세션 -> 쿠키에 어떤 내용이 들어가 있는지 확인을 하려면 Filter에서 일관적으로 처리를 하는게 좋다.
    누구나 다 영향을 받아야 하는 기능들을 구현을 할 때 Filter를 사용한다.

    >>
    둘의 핵심은 모든 요청과 응답 과정에서 컨트롤러에 도달하기 전에 
    요청과 응답의 일부분을 골라서 적용을 시켜줄 수 있다.
    100개의 RequestMapping이 있으면 어떤 요청이 오고 갔는지 log를 남겨주라고 한다면
    모든 RequestMapping에 log를 붙여줘야 한다,,
    하지만 그 대신에 Interceptor와 Filter를 통해 
    요청이 컨트롤러에 도달하기 전에 어떠한 데이터가 있었는지 등을 확인을 하는데
    사용을 할 수 있다.

    Interceptor에서는 body를 읽어오는게 힘들다. (권장ㄴ)

    AOP를 직접 다루지 않을때 (관점지향 프로그래밍에 필요한 기능들을 다루지 않고)
    스프링에 요청들이 오고 가는 과정에서 무슨 일들이 있었는지 중간에서 확인을 하고 싶다고 하면
    Interceptor와 Filter을 찾아볼 수 있다.

    RequestMapping이 붙은 메서드는 HandlerMethod로 취급이 되어서 DispatherServlet에 등록이 된다.


    불특정 다수에 요청에 의해서 한번에 동시에 적용을 하고 싶은게 있으면 (기능이 있으면)
    HandelrInterceptor, Filter를 고려해 볼 수 있다.


## HandlerInterceptor
1. 인터셉트를 구현하는 클래스를 만든다.  
이 클래스는 인터셉터 기능을 사용하기 위해(사용하고 싶을 경우) 원하는 인터셉터 기능을 구현하는 클래스이다.  
현재는 그냥 어떤 일이 일어나고 있는지 log로 기록하는 용도이다.  
원하는 경로에 넣기 위해서는 이 클래스를 의존하는 클래스가 따로 필요하다.  

@Component
implements HandlerInterceptor
구현을 하라고 알림이 뜨지 않는다. 기본 구현되어 있기 때문에?
    1. preHandle 메서드: HandlerMethod(RequestMapping) (요청이)메서드에 도달하기 전 실행.
    preHandle이 false를 반환하면 요청이 HandlerMethod로 전달 되지 않음.

    2. postHandler 메서드: HandlerMethod(RequestMapping)가 처리가 되고 응답이 보내지기 전 실행.
    실제로 사용자에게 응답을 보내기 전 실행이 된다.

    3. afterCompletion 메서드: 요청의 처리가 완전히 마무리 되었을 때 실행.
    요청 처리 과정에서 예외가 발생하면 인자로 전달 받는다.
    실제로 응답이 보내지고 난 다음 실행.

    ```java
            //어떤 헤더가 있었는지 log로 남기기
        @Component
        @Slf4j
        public class LoggingInterceptor implements HandlerInterceptor {

            @Override
            public boolean preHandle(//요청이 HandlerMethod(RequestMapping) 메서드에 도달하기 전 실행.
                    //요청
                    HttpServletRequest request,
                    //응답
                    HttpServletResponse response,
                    Object handler
            ) throws Exception {
                HandlerMethod handlerMethod = (HandlerMethod) handler;
                log.info("pre handling of {}", handlerMethod.getMethod().getName());
                //body를 읽는 것은 막는건 없지만,,,
                request.getReader().lines().forEach(log::info);

                Enumeration<String> headerNames = request.getHeaderNames();
                while (headerNames.hasMoreElements()) {
                    String headerName = headerNames.nextElement();
                    log.info("{}: {}", headerName, request.getHeader(headerName));
                }
                //preHandle이 false를 반환하면
                //요청이 HandlerMethod로 전달 되지 않음.
                return HandlerInterceptor.super.preHandle(request, response, handler);
            }//인터셉터를 만든거다,,,,



            @Override
            public void postHandle(//요청이 처리가 되고 (RequestMapping이 처리가 되고) 응답이 보내지기 전 실행.
                    //요청객체
                    HttpServletRequest request,
                    //응답객체
                    HttpServletResponse response,
                    //실제로 요청을 처리할 RequestMapping을 나타내는 메서드 객체.
                    Object handler,
                    ModelAndView modelAndView
            ) throws Exception {
                HandlerMethod handlerMethod = (HandlerMethod) handler;
                log.info("post handling of{}", handlerMethod.getMethod().getName());
                Collections<String> headerNames = response.getHeaderNames();
                for (String headerName : headerNames) {
                    log.info("{}: {}", headerName, response.getHeader(headerName));
                }
                log.info("=====end of post handling");
        //        HandlerInterceptor.super.postHandle(request, response, handler, modelAndView);
            }



            @Override
            public void afterCompletion(//요청의 처리가 완전히 마무리 되었을 때 실행.
                    HttpServletRequest request,
                    HttpServletResponse response,
                    Object handler,
                    Exception ex //요청 처리 과정에서 예외가 발생하면 인자로 전달 받는다.
            ) throws Exception {
                HandlerInterceptor.super.afterCompletion(request, response, handler, ex);
            }
        }
    ```

2. 구현한 인터셉트 클래스를 원하는 경로에 넣기 위한 클래스를 만든다.
    @Configuration
    @RequiredArgsConstructor
    public class InterceptorConfig implements WebMvcConfigurer
        private final LoggingInterceptor loggingInterceptor;
    //구현한 인터셉트 클래스를 워하는 경로에 설정할 수 있다.
    ```java
    @Configuration
    @RequiredArgsConstructor
    public class InterceptorConfig implements WebMvcConfigurer {
        private final LoggingInterceptor loggingInterceptor;

        @Override
        //인터셉터를 등록하기 위한 메서드
        public void addInterceptors(InterceptorRegistry registry) {
            registry
                    //어떤 인터셉터를
                    .addInterceptor(loggingInterceptor)
                    //어떤 경로에
                    .addPathPatterns("/tests");
        }
    }
    ```












