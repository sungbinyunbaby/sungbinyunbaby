## Test Code
PostMan도 test이다.

테스트 코드가 안좋은 상황도 있다.

만약 요구사항이 빠르게 급변한다면 오히려 테스트 코드는 장애물이 될 수 있다.

테스트 코드 - 문서화 - 요구사항

요구사항이 바뀌면 테스트 코드도 바뀌어야 한다.

테스트 검증

assert문 - 여러가지가 있다 잘 보고 import해야함.지

## ControllerTest 방법
1. 클래스에 @Extenwith(MockitoExtension.class) -  
Mockito는 자바에서 유닛 테스트를 수행할 때 목(Mock) 객체를 생성하고 사용하기 위한 라이브러리입니다. 
2. 
@Mock

private UserService userService;

: 특정 객체에 대한 가짜 구현을 생성하며,
이를 사용하여 해당 객체의 행동을 검증하거나 특정 상황을 시뮬레이션 할 수 있다.

3. 
@InjectMocks

private UserController userController;

: mock객체들을 주입받아 사용할 객체를 자동으로 생성해주는 기능을 제공한다.
mock객체를 생성하고 필드에 주입하는 작업을 @InjectMocks를 사용하여 자동으로 처리할 수 있다.
userController 객체가 자동으로 mock객체를(userService) 사용할 수 있게 된다.

즉, @InjectMocks에 테스트 하려는 클래스, @Mock에 의존관계 주입을 하는 클래스를 넣는다.

4. private MockMvc mockMvc;

: 테스트를 위한 클래스, 컨트롤러를 테스트 할 때 사용된다.
주로 MVC의 통합 테스트를 수행할 때 사용된다.

5. mockMvc = MockMvcBuilders
        .standaloneSetup(userController)
        .build();

: 이 테스트 클래스의 개별 클래스 전에 실행할 코드.
MockMvcBuilders를 사용하여 MockMvc 인스턴스를 생성하고 설정할 수 있습니다.
MockMvc를 설정하고 초기화할 때 활용.
가장 일반적인 사용 사례는 standaloneSetup 메서드를 사용하여 MockMvc를 설정하는 것입니다.
MockMvc 인스턴스를 설정하는 메서드 중 하나입니다.
이 메서드를 사용하면 특정 컨트롤러에 대한 테스트를 진행할 수 있습니다.
standaloneSetup(userController)은 userController를 테스트하기 위한 MockMvc를 설정합니다.
이렇게 설정된 MockMvc를 사용하여 컨트롤러의 동작을 테스트할 수 있습니다.
이 메서드는 스프링 애플리케이션 컨텍스트를 초기화하지 않고 단독으로 테스트하기 때문에 테스트 실행이 빠르고 경량화됩니다.
하지만 스프링의 다양한 빈이나 설정을 사용하는 통합 테스트를 할 때에는
webAppContextSetup이나 @WebMvcTest 등을 사용하여 웹 애플리케이션 컨텍스트를 초기화하는 방법을 고려해야 할 것입니다.

6. when(userService.create(any()))/
        .thenReturn(responseDto)
:
컨트롤러: return userService.create(any)
서비스: public responseDto create(any){ return responseDto }

: when() 메서드는 목 객체의 특정 메서드 호출에 대한 동작을 정의합니다.
이것은 주로 테스트 시에 실제 create 메서드를 호출하지 않고도 특정한 결과를 반환하게끔 만들어줍니다.
이렇게 함으로써 테스트에서는 실제 의존성이나 리소스에 의존하지 않고
모의(mock)된 결과를 사용하여 코드를 테스트할 수 있습니다.

7. ResultActions

: ResultActions는 Spring의 MockMvc를 사용하여
수행된 HTTP 요청에 대한 결과를 검증하기 위한 메서드 체이닝을 제공하는 인터페이스입니다.
ResultActions는 perform 메서드를 통해 시작되며,
실제 HTTP 요청을 시뮬레이션하고 그에 대한 결과를 반환합니다.
mockMvc.perform(get("/person/1"))
    .andExpect(status().isOk()) //HTTP status 상태 확인
    .andExpect(content().contentType(MediaType.APPLICATION_JSON))
	 //JSON형식이 맞는지
	특정 HTTP 응답이 JSON 형식의 콘텐츠를 가지고 있는지를 검증합니다.
	이를테면, 컨트롤러가 JSON 형식의 응답을 반환하도록 구현되었는지를 테스트할 때 사용됩니다.
	@RequsetBody인지 아닌지,,,?
    .andExpect(jsonPath("$.person.name").value("Jason")); 
	//HTTP Body 부분이 JSON이 "username" : "testUser"  이렇게 적을건데 이렇게 하면 실행이 되는지 확인.


ControllerTest 방법
1. 클래스에 @Extenwith(MockitoExtension.class)
2. @Mock private UserService userService;
3. @InjectMocks private UserController userController;
4. private MockMvc mockMvc;
5. mockMvc = MockMvcBuilders
        .standaloneSetup(userController)
        .build();
6. when(userService.create(any()))/
        .thenReturn(responseDto)
7. ResultActions
mockMvc.perform(get("/person/1"))
    .andExpect(status().isOk())
    .andExpect(content().contentType(MediaType.APPLICATION_JSON))
    .andExpect(jsonPath("$.person.name").value("Jason")); 


<ServiceTest>
모킹
데이터베이스에 저장을 하는 것이 아니다.
그래서 해당하는 객체 모두를 다 정의해줘야 한다.

```java
package com.example.contents;

import com.example.contents.dto.UserDto;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.junit.jupiter.MockitoExtension;
import org.springframework.http.MediaType;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.ResultActions;
import org.springframework.test.web.servlet.setup.MockMvcBuilders;

import static org.hamcrest.CoreMatchers.is;
import static org.hamcrest.CoreMatchers.notNullValue;
import static org.mockito.ArgumentMatchers.any;
import static org.mockito.Mockito.when;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.post;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.*;


@ExtendWith(MockitoExtension.class)
public class UserControllerTests {
    @Mock
    //특정 객체에 대한 가짜 구현을 생성하며,
    //이를 사용하여 해당 객체의 행동을 검증하거나 특정 상황을 시뮬레이션 할 수 있다.
//    @Mock 어노테이션을 사용하면 Mockito는 목 객체를 생성하고 해당 필드에 주입합니다.
//    이를 통해 테스트에서는 실제 객체 대신 가짜로 생성된 목 객체를 사용하여 특정 동작을 검증하거나 조작할 수 있습니다
    private UserService userService;//private final UserService userService

    @InjectMocks
    //mock객체들을 주입받아 사용할 객체를 자동으로 생성해주는 기능을 제공한다.
    //mock객체를 생성하고 필드에 주입하는 작업을 @InjectMocks를 사용하여 자동으로 처리할 수 있다.
    private UserController userController;
    //userController 객체가 자동으로 위의 mock객체를(userService) 사용할 수 있게 된다.

    //테스트를 위한 클래스, 컨트롤러를 테스트 할 때 사용된다.
    //주로 MVC의 통합 테스트를 수행할 때 사용된다.
    private MockMvc mockMvc;


    //이 테스트 클래스의 개별 클래스 전에 실행할 코드.
    //MockMvcBuilders를 사용하여 MockMvc 인스턴스를 생성하고 설정할 수 있습니다.
    //MockMvc를 설정하고 초기화할 때 활용.
    //가장 일반적인 사용 사례는 standaloneSetup 메서드를 사용하여 MockMvc를 설정하는 것입니다.
    //MockMvc 인스턴스를 설정하는 메서드 중 하나입니다.
    //이 메서드를 사용하면 특정 컨트롤러에 대한 테스트를 진행할 수 있습니다.
    //standaloneSetup(userController)은 userController를 테스트하기 위한 MockMvc를 설정합니다.
    //이렇게 설정된 MockMvc를 사용하여 컨트롤러의 동작을 테스트할 수 있습니다.
    /*
    standaloneSetup을 사용하면 해당 컨트롤러만을 대상으로 하는 간단한 테스트를 수행할 수 있습니다.
    이 메서드는 스프링 애플리케이션 컨텍스트를 초기화하지 않고 단독으로 테스트하기 때문에 테스트 실행이 빠르고 경량화됩니다.
    하지만 스프링의 다양한 빈이나 설정을 사용하는 통합 테스트를 할 때에는
    webAppContextSetup이나 @WebMvcTest 등을 사용하여 웹 애플리케이션 컨텍스트를 초기화하는 방법을 고려해야 할 것입니다.
     */
    @BeforeEach
    public void beforeEach() {
        mockMvc = MockMvcBuilders
                .standaloneSetup(userController)
                .build();
    }

    @Test
    @DisplayName("UserDto를 표현한 JSON 요청을 보내면 UserDto를 표현한 JSON을 응답")
    public void testCreate() throws Exception {
        // given
        // userService.createUser 정의
        String username = "edujeeho";
        UserDto requestDto = new UserDto(
                null, username, null, null, null, null
        );
        UserDto responseDto = new UserDto(
                1L, username, null, null, null, null
        );

        //when() 메서드는 목 객체의 특정 메서드 호출에 대한 동작을 정의합니다.
        /*
        이것은 주로 테스트 시에 실제 create 메서드를 호출하지 않고도 특정한 결과를 반환하게끔 만들어줍니다.
        이렇게 함으로써 테스트에서는 실제 의존성이나 리소스에 의존하지 않고
        모의(mock)된 결과를 사용하여 코드를 테스트할 수 있습니다.
         */
        when(userService.create(any()))//:create 메서드에 어떠한 매개변수가 들어오던지 상관없이 이 메서드에 대한 동작을 설정하고자 한다.
                .thenReturn(responseDto);//create 메서드가 호출될 때 responseDto를 반환하도록 설정합니다.
        //컨트롤러: return userService.create(any)
        //서비스: public responseDto create(any){ return responseDto }

        //when 시작
        // perform: HTTP 요청이 보내졌다고 가정
        //ResultActions는 Spring의 MockMvc를 사용하여
        //수행된 HTTP 요청에 대한 결과를 검증하기 위한 메서드 체이닝을 제공하는 인터페이스입니다.
//        ResultActions는 perform 메서드를 통해 시작되며,
//        실제 HTTP 요청을 시뮬레이션하고 그에 대한 결과를 반환합니다.
        ResultActions result = mockMvc.perform(
                // post 요청을 보낸다.
                post("/user") //PostMapping("/user")
                        // 이 요청의 Body는 requestDto를 JSON으로 변환한 것 @RequestBody RequestDto requestDto
                        .content(JsonUtil.toJson(requestDto))//content:내용물, 내용, 목차
                        // 이 요청의 Body는 JSON이라고 생각하고 이해할 것 ?????
                        .contentType(MediaType.APPLICATION_JSON)
                        //(content().contentType(MediaType.APPLICATION_JSON))
        );

        // then
        // 응답의 코드가 2xx
        // 내용이 JSON
        // username이 변화하지 않았다.
        // id가 null이 아니다.
        result.andExpectAll( //ResultActions의 기능.
                // 2xx번대(성공) 상태코드, .andExpect(status.isOk()) 응답상태코드 확인.
                status().is2xxSuccessful(),// HTTP의 status
                // JSON 응답을 주었다.
                content().contentType(MediaType.APPLICATION_JSON),// HTTP헤더의 Content-Type
                // JSON에 username이라는 값이 전달된 username과 동일하다.
                //JSON 응답을 검증하는데 사용되는 코드.
                jsonPath("$.username", is(username)),//"username" : "testUser"
                // id는 null이 아니다.
                jsonPath("$.id", notNullValue())
                /*
                {
                    "username" : "testUser",
                    "id" : notnull
                }
                 */
        );
    }
}

```
ServiceTest
```java
package com.example.contents;

import com.example.contents.dto.UserDto;
import com.example.contents.entity.User;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.junit.jupiter.MockitoExtension;

import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.mockito.Mockito.when;

//다른 것들은 잘 동작한다고 가정.
@ExtendWith(MockitoExtension.class)
public class UserServiceTest {
    @Mock
    //실제 userRepository가 아니라
    //UserRepository의 메서드를 다 가지고 있지만
    //동작은 다르게 하는 Mock 객체
    //동작하는 방식을 정의를 해줘야 한다.
    private UserRepository userRepository;

    @InjectMocks
    //실제 userRepository가 아닌
    //위에 만든 짝퉁 userRepository를 의존성으로 사용.
    private UserService userService;

    //userDto를 인자로 받아 User을 생성하고
    //그 결과를 UserDto로 반환.
    @Test
    @DisplayName("UserDto로 사용자 생성")
    public void testCreateUser() {
        //given
        //데이터베이스에 저장이 되지 않아서 다 수동으로 만들어 주는 것이다.
        //1. userRepository가 특정 user를 전달 받을 것을 가정한다.
        String username = "sungbin";
        //userRepository가 입력 받을 user
        User userIn = new User(username, null, null, null); //User newUser = new User(dto.getUsername(),,,)
        //2. userRepository가 반환할 user
        User userOut = new User(username, null, null, null); //newUser = repository.save(newUser);
        userOut.setId(1L);
        //3. userRepository.save(userIn)의 결과를 userOut으로 설정.
        when(userRepository.save(userIn)) //newUser = repository.save(newUser);
                .thenReturn(userOut);
        when(userRepository.existsByUsername(username))
                .thenReturn(false);//if (repository.existsByUsername(dto.getUsername()))

        //when - userDto를 전달한다.
        //public UserDto create(UserDto dto)
        UserDto userDto = new UserDto(null, username, null, null, null, null);
        UserDto result = userService.create(userDto);//return UserDto.fromEntity(newUser);

        //then - 돌아온 result를 검사한다.
        assertEquals(username, result.getUsername());
    }

    //readUserByUsername
}

```