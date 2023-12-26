## 애노테이션
### @Controller

:@Component를 구체화하여 만든 어노테이션

View를 리턴하는  @Controller 와 Json 데이터를 리턴하는  @RestController 가 있다.

:@ResponseBody가 있어야 데이터만 출력할 수 있다.

### @RestController

:view가 필요 없는 API만 지원시 사용(@Controller + @ResponseBody)

:ResponseBody가 없어도 데이터를 리턴한다.

:데이터만 출력가능하다. view 리턴 불가능.


## 매핑 
### @RequestMapping
:@RequestMapping(value, method = ... .Get/Post/등등)
:@RequestMapping(value = "/hi", method = RequestMethod.GET)  
=>hi라는 요청이 오면 Get메소드로 처리를 해주겠다.  
:특정 url로 요청이 올 때 어떤 메소드로 처리할지 매핑해주는 애노테이션  
메소드 위에 애노테이션 해놓으면 @RequestMapping("/member") /member뒤에
url이 와야한다. localhost:8080/memeber/add, localhost:8080/memeber/min   

### @GetMapping(value) (@PostMapping, @PutMapping,,,)  
:@RequestMapping을 상속 받는다.  
:@GetMapping는 @RequestMapping중에서도 Get방식으로 구체화된 애노테이션  
:특정 url이 오면 Get형식으로 처리.

### @ResponseBody  
:View가 아닌 데이터를 리턴해준다.  
:@RestController로 컨트롤러가 설정되어 있으면 @ResponseBody를 안넣어도 데이터만 전송한다.

### Get
서버에서 데이터를 조회. Read  
url에 파라미터가 노출되기 때문에 중요한 정보를 다루면 안된다. 보안에 취약.  
캐시가능  
히스토리에 남는다.

### Post
서버에 테이터를 생성할 때 주로 사용. Create  
http body에 데이터를 포함해서 전달, 파라미터 직접적 노출하지 않는다.  
길이 제한이 없다.  
캐시불가능  
히스토리에 남지 않는다.  
서버의 값이나 상태를 바꾸기 위해 활용

GET 방식은 URL에 폼 데이터를 추가하여 서버로 전달하는 방식입니다.

GET 방식의 HTTP 요청은 브라우저에 의해 캐시되어(cached) 저장됩니다.

또한, GET 방식은 보통 쿼리 문자열(query string)에 포함되어 전송되므로, 길이의 제한이 있습니다.

따라서 보안상 취약점이 존재하므로, 중요한 데이터는 POST 방식을 사용하여 요청하는 것이 좋습니다.

 

POST 방식은 폼 데이터를 별도로 첨부하여 서버로 전달하는 방식입니다.

POST 방식의 HTTP 요청은 브라우저에 의해 캐시되지 않으므로, 브라우저 히스토리에도 남지 않습니다.

또한, POST 방식의 HTTP 요청에 의한 데이터는 쿼리 문자열과는 별도로 전송됩니다.

따라서 데이터의 길이에 대한 제한도 없으며, GET 방식보다 보안성이 높습니다.