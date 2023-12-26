# Today
@Controller, @RestController  
@RequestMapping, @GetMapping  
@ResponesBody  
Thymeleaf  

## Spring 프레임워크  
IOC  
: 제어의 반전, 프레임워크가 객체의 생명 주기를 모두 프레임워크에 위임할 수 있다. 외부라이브러리가 프로그래머가 작성한 코드를 호출하고, 흐름을 제어한다.  
개발자가 작성한 객체나 메소드의 제어를 개발자가 아니라 외부에 위임하는 설게 원칙이다.

DI  
: 의존성 주입, 프로그램 구성요소간의 의존 구조를 외부 파일에서 정의해준다. 설정 파일을 분리하기 때문에 유지보수가 쉽고, 각 구성요소가간 결합성이 낮아지게 된다.  (@Autowried)
IOC의 원칙을 실현하기 위한 여러 디자인 패턴 중 하나.

AOP  
: 관점지향 프로그래밍: 관점에 따라 기능을 분리. 

POJO
: Plan Old Java Object: 기존의 자바 코드를 이용할 수 있다.

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


## 동적 웹페이지 만들기  
```java
@GetMapping("/hi")
    public String hi(String name, Model model) {
        model.addAttribute("name", name);//전달할 뷰에 데이터 전달
        return "hi";
    }
```

localhost:8888/hi?name=tjdqls  
Model을 이용해 view에 데이터를 전달한다.  
model.addAttribute(key, value)를 hi.html에 전달한다.

```java
<h1>Hi~~~!!!!!!<span th:text="${name}"></span></h1>
```  
th = thymeleaf 타임리프: 백엔드 서버에 html을 동적으로 렌더링하는 용도로 사용.

th:text="${name}"에 key값을 받아서 리턴한다.  
그러면 value값이 출력이 될 것이다. 

## shop파일 해석
```java
@Controller
@RequestMapping("/member")
public class MemberController {

    @GetMapping("/add") //정적 웹
    public String addForm() {
        return "memberForm";
    }

    @PostMapping("/add") //동적 웹 위에서 입력한 이름이 항상 달라지니까.
    public String add(@RequestParam String name, Model model) {
        model.addAttribute("aa", name);
        return "memberView";
    }
}```
<memberForm.html>
html
<head>
    <meta charset="UTF-8">
    <title>회원가입</title>
</head>
<body>
<h1>회원가입 폼</h1>
<form action="add" method = "post">
    이름: <input type="text" name="name" placeholder="이름을 입력하세요." />
    <input type="submit" value="회원가입" />
</form>
</body>

<form>: 입력된 데이터를 한번에 서버로 전송한다.
form action: 폼을 전송할 서버쪽 스크립트 파일을 지정.
form method: 폼 데이터가 서버로 제출될 때 사용되는 http 메소드를 명시한다.(Get 또는 Post)
<input>: form 태그 안에서 사용가능.

http add에 http 메소드는 post 방식으로 제출한다.(보안 좋아)  
이름이 입력하고 회원가입을 누르면 메소드의 이름은 같지만 PostMapping("/add")로 이동하게 된다.  

<memberView.html>
<html lang="en" xmlns: th = "http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>회원정보보기</title>
</head>
<body>
<h1>회원정보보기</h1>
이름: <span th:text="${aa}"></span>
</body>
</html>  

Thymeleaf를 사용하여 동적이 웹으로 만들어 줘야한다.  
GetMapping("/add")에서 입력한 이름을 받아야 하기 때문에 항상 그 값이 달라지기 때문이다.  
public String add(@RequestParam String name, Model model) {
        model.addAttribute("aa", name);

윤성빈이라는 이름으로 회원가입을 했으면 윤성빈이 출력된다.







