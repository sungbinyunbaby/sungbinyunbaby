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