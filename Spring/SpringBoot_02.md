# 알짜배기 지식
Client request를 서블릿이 받는다. 스프링이 받는게 아니다.

스프링은 이들을 가지고 내포하고 있는 것이지 이들의 기능을 수행할 수 없다. 그러므로 스프링 안에 내포되어 있는 톰캣이 받는다.

- Web Server: 

정적인 자워은 WAS를 거치지 않고 바로 Web선에서 응답해주고, 동적인 자원은 WAS에 요청을 전달하여 WAS에서 받은 결과를 클라이언트에게 응답한다.(appache)

- WAS: 

인터넷 상에서 HTTP를 통해 상요자나 컴ㅁ퓨터나 장치에 애플리케이션을 수행해주는 미들웨어 (소프트웨어 엔진)이다.(tomcat)

동적 서버 콘텐츠를 수행하는 것으로 일반적이 ㄴ웹 서버와 구별 되며, 주로 데이터베이스 서보와 같이 수행된다.

- DOM:

Document Object Model  
HTML 내의 원하는 위치에 접근하기 위한 하나의 방식. 자바스크립트 사용. 

html 문서의 태그들을 자바스크립트가 이용할 수 있는 객체로 만드는 것.  
브라우저가 HTML 웹 페이지를 인식하는 방식을 계층화시켜 트리구조로 만든 객체모델.

- 자바스크립트를 많이 사용하면 브라우저는 서버에 비해 성능은 별로 좋지 않아서 안좋다.  
이를 효율적으로 처리하도록 해주는게 React View

- 요즘 SPA Single Page Application이라고 한페이지만 존재하고 데이터만 받는 페이지가 유행이다. 

- Json
자바스크립트 객체 문법으로 구조화된 데이터를 표현하기 위한 문자 기반의 표준 포맷.  
문자열 형태로 존재한다.  
데이터에 접근하기 위해서 네이티브 json '객체'로 변환될 필요가 있다. => '파싱'  
네트워크를 통해 전달할 수 있게 객체를 문자열로 반환하는 과정 => '문자열화'  
스프링부트 MVC에서 @RequesBody를 사용한 메소드에서 객체로 호출을 하면 json방식으로 응답을 한다.    
데이터 구조와 실제 데이터를 다른 언어 및 플랫폼에서 해석 가능한 형식으로 전송할 수 있어야 합니다. JSON은 이를 가능케 하는 데이터 교환 포맷.  
인간이 읽을 수 잇는 문서로 이루어져 있다.   
JSON은 일반적으로 서버에서 클라이언트로 데이터를 보낼 때 사용하는 양식으로, '제이슨'으로 읽는다. 클라이언트가 사용하는 언어에 관계 없이 통일된 데이터를 주고받을 수 있도록, 일정한 패턴을 지닌 문자열을 생성해 내보내면 클라이언트는 그를 해석해 데이터를 자기만의 방식으로 온전히 저장, 표시할 수 있게 된다.

- 톰캣: WAS 컨테이너(Servlet 컨테이너), 동적 데이터 처리, 컨테이너 안에 jsp, servlet

-Spring 기본동작:
1. 핸들러 맵핑(Controller를 찾는다.)
2. 핸들러 어댑터: 핸들러 맵핑을 통해서 찾은 핸들러를 실행하기 위함. 여러 컨트롤러(핸들러) 들이 있을 수 있다. 그 중에서 개발자에게 맞는 핸들러를 정하기 위함.  
애노테이션 기반의 핸들러가 0순위  
3.  몰라잉,,, 조금 더 깊이 공부를 하고 다시 작성할 예정,,,

# 프레임워크:  
내가 작성한 코드를 대신 제어하고 실행하면 프레임워크.

# 라이브러리:
내가 작성한 코드가 직접 제어의 흐름을 담당하면 라이브러리.

# Spring은 프레임워크이다. 그 기능을 수행하기 위해  IoC, DI의 기능이 있다.  
Controller, Service 같은 객체들의 동작을 개발자가 직접 구현하기는 하지만,  
해당 객체들이 어느 시점에 호출될지는 신경쓰지 않는다.  
Spring이 요구하는 대로 객체를 생성하면, Spring이 해당 객체들을 가져다가 생성하고, 메서드를 호출하고, 소멸시킨다.  
이게 바로 프레임워크이고 프로그램의 제어권이 개발자에서 프레임워크로 역전된 것이다.

# IoC:  
제어의 역전. 코드의 제어를 내가 아닌 외부(컨테이너)에 제어를 맡기겠다.  
구체적인 구현을 분리시킬 수 있다.  
객체간 의존성이 낮아진다.  

# DI:  
의존관계를 제어한다.  
종속성을 줄일 수 있다.
재사용성이 증가한다.  
다른 구현이 필요한 경우, 코드를 변경할 필요없이 해당 구현을 사용하도록 구성할 수 있다.  
객체의 생성과 소멸 등 클래스간 의존관계를 스프링 컨테이너가 제어해준다.
방법1. 생성자 주입, 방법2. setter 주입.

# 싱글톤 패턴  
어플리케이션이 시작될 때 최초 한번만 메모리에 할당하고 그 메모리에 인스턴스를 만들어 사용.  
private로 되어 있어서 get(조회)만 가능하고, 변화가 불가능하다.  
클라이언트가 여러번 호출을 해도 최초 생성된 메모리의 값만 응답한다.  
스프링은 기본이 싱글톤 Scope으로 되어있다.

# @Configuration
수동으로 Bean을 등록할 때 메소드 상단에 사용된다.  
기능:  
1. 스프링컨테이너에서 빈을 관리할 수 있게한다. 
2. Config 클래스를 CGLIB를 사용하여 바이트코드를 조작하여 Config클래스를 상속 받는 임이의 (proxy)클래스를 복사하여 그 임의의 클래스가 빈으로 등록되며, 싱글톤이 되도록 보장해준다.

- @Configuration를 사용하지 않고, Bean만 사용한다면:  
Bean이 스프링컨테이너에 등록되기는 하지만 싱글톤이 보장되지 않는다.