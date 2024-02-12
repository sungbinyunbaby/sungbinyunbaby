# JWT - JSON으로 표현된 정보를 안전하게 주고 받기 위한 토큰의 일종이다. 
Json Web Token

JSON으로 표현된 정보를 안전하게 주고 받기 위한 토큰의 일종이다.  
인증 정보가 담겨있는 토큰.  
header.payload.signature  
내용이 읽기 어려워서 사용하는게 아니다.  
내용은 읽기 편하나 내용을 바꾸기 어렵기 때문에 사용을 하는 것이다.  
제대로 만들어져 있는 JWT인지, 악성 사용자가 만든 억지로 만든 JWT인지 판단하는게 편하기 때문에 사용한다. => 위변조가 어렵다. 토큰 기반 인증 시스템에서 많이 사용한다. 

payload: 누구인지, 권한이 뭐가 있는지 등을 넣을 수 있다.  
defalut로 들어가는 것: sub-누구인지, iat-언제 발급이되었는지, exp-언제 만료가 되었는지. 그 외 넣고 싶은게 있으면 넣어주면 된다. 그러나 JWT는 읽는게 어려운 것이 아니기 때문에 너무 많은 정보는 넣지 않는게 좋다. 사용자가 누구인지 명확하게 판단할 수 있을 정도면 충부하다.

signature: 헤더와 페이로드를 보고 암호를 넣어준다. 전체 데이터를 암호화 한게 signature에 들어간다. 그것을 기준으로 위조 되었는지 확인을 하는 역할을 한다. => JWT의 위변조 판단을 위한 부분.

토큰 기반으로 동작하며, 클리이언트 측에서 사용자 정보를 토큰에 담아 서버로 전달하는 방식.  
클라이언트에 저장되며 서버에서는 토큰의 유효성만을 검증한다.  
클라이언트 측에서 토큰을 저장하고 관리하기 때문에 서버에 대한 부담이 줄어든다.  
주로 클라이언트 측에서 토큰을 저장하고 전송하는 데에 사용되지만, 이를 쿠키에 저장하여 관리하는 경우가 많다.  
JWT를 안전하게 쿠키에 저장하면, 클라이언트 측에서는 브라우저가 자동으로 쿠키를 관리하게 되어 로그인 상태 등을 유지하면서도 일부 보안 문제를 방어할 수 있다.
서명(signature)을 통한 검증을 통해 안전한 통신을 제공하지만, 토큰이 노출되면 정보를 볼 수 있다.  
토큰을 생성한 서버가 아닌 다른 서버에서도 토큰을 검증할 수 있어, 분산 환경에서 확장성이 좋다.  
=>토큰 소유가 곧 인증 -> 여러 서버에 걸쳐서 인증이 가능하다.  
쿠키는 요청을 보낸 클라이언트에 종속되지만, 토큰은 쉽게 첨부가 가능하다.(주로 Header에)  
로그인 상태라는 개념이 사라져서 로그아웃이 불가하다.(기본적으로는,,, 방법은 여러가지 있다.)  
auth0을 이용하면 아이디를 카카오, 네이버처럼 다른 사이트에서 이용할 수 있다.

Session:  
서버 측에 사용자 정보 저장. 서버에 사용자 정보를 저장하므로, 서버에서 세션 관리가 이루어진다.    
클라이언트는 세션 ID를 쿠키 등을 통해 전달하여 서버에 저장된 정보를 활용하는 방식.  
상태 유지를 위해 서버 측헤서 세션을 관리하므로, 클라이언트는 세션 ID만을 기억하면 된다.  
보안은 서버에서 세션을 안전하게 관리하고 전달하기 때문에 상대적으로 안전하다.  
상대적으로 서버 확장이 어렵다: 서버에 세션을 안전하게 관리하고 전달하기 때문에  
(여러 대의 서버 컴퓨터를 추가할 경우 각 서버마다 세선 정보가 저장되기 때문에)  
웹 어플리케이션에서 세션을 관리할 때 자주 사용되는 쿠키는 단일 도메인 및 서브 도메인에서만 작동하도록 설계되어 있다. 따라서 쿠키를 여러 도메인에서 관리하는것은 번거롭다.


Session과 JWT의 보안  
세션은 서버에 사용자 상태를 저장하므로, 클라이언트가 세션 ID를 훔쳐도 서버에 저장된 실제 데이터에는 접근할 수 없다.  
JWT는 토큰 내에 정보를 포함하고 있기 때문에 클라이언트가 토큰을 훔치면 해당 정보에 접근할 수 있다. 따라서 토큰에 중요한 정보를 담아야 할 경우, 토늨의 안전성을 고려해야 한다. 또한, 토큰을 서명하지 않았거나암호화하지 않으면 토큰의 무결성이 보장되지 않을 수 있다.  
JWT를 사용할 때는 xss(cross-site sripting)공격에 노출될 가능성이 있다. JWT는 클라이언트 측에서 디코딩이 가능한 구조를 가지고 있다. 그래서 XSS 공격이 성공하면 공격자는 토큰을 디코딩하여 내부의 정보를 읽을 수 있다. 특히 토큰을 쿠키에 저장하면 쿠키를 훔치는 xss 공격에 취약해질 수 있다.  
따라서 token에 중요 정보를 넣어 놓지 않아야한다.  
토큰에 넣는 데이터가 많아질 수록 토큰이 길어지는데 매 api호출 시마다 토큰 데이터를 서버에 전달하는데 그만큼 네트워크 대역폭 낭비가 심할 수 있다.  
한번 발급된 token은 수정, 폐기가 불가하다.  
Access token의 Expire time을 짧게 하고 Refresh token을 이용해서 중간중간 Access token을 재발급하게 해서 Access token이 탈취되더라도 해커가 이용할 수 있는 시간을 최소화한다.  
세션은 CSRF(cross-site request forgery) 공격에 취약할 수 있다. 이에 대비하여 추가적인 보호책이 필요한다. JWT는 Header에 CSRF 토큰을 추가하여 이를 방지할 수 있다.

XSS(Cross-Site Scipting): 공격자가 악의적인 스크립트를 웹 페이지에 삽입하여 사용자의 브라우저에서 실행되도록 하는 공격이다.  
주로 사용자가 입력을 적절하게 검증 또는 이스케이프하지 않은 경우 발생한다.  

CSRF(Cross-Site Request Forgery): 희생자의 권한을 사용하여 특정 웹 애플리케이션에서 비정상적인 요청을 보내도록 하는 공격이다.  
사용자의 세션 정보가 쿠키에 저장되어 있고, 해당 쿠키가 자동으로 요청에 포함되어 인증을 수행하는 경우 발생할 수 있다.



# JWT 생성 JwtTokenUtils class생성 - jwt를 만들고 관련기능을 만든다.
1. jwtTokenUtils 클래스를 만들어 토큰에 관련된 정보를 넣는다. JWT 자체와 관련된 기능을 만든다. Service 클래스라고 보면 된다.

     암호키 만들기-비밀번호를 암호화 하고, 파싱하고 검사한다.
    .yaml에 jwt: secret: 암호를 넣어준다.  
    이를 클래스에 적용한다.  
    @Value("${jwk.secret}")
    ```java
    private final Key signinqkey; //보안과 관련된 인터페이스를 활용한다.
    private final JwtParser jwtParser;//JWT를 해석하는 용도.

     //암호키 지정하는 메서드. signature 부분.
    public JwtTokenUtils(
            @Value("${jwk.secret}")
            String jwkSecret
    ) {
        log.info(jwkSecret);
        this.signinqkey  //JWT를 만드는 용도의 암호키
                = Keys.hmacShaKeyFor(jwkSecret.getBytes());
        this.jwtParser = Jwts //JWT를 해석하는 용도
                .parserBuilder()
                .setSigningKey(this.signinqkey)
                .build();
    }
    ```

2. JWT 발급준비와 발급하기.
    UserDetails를 받아서 jwt로 변환하는 메서드를 만든다. 
    Claims에 jwt에 담고 싶은 정보를 담는다.  
    payload에 정보를 추가하는것. sub, iat, signature

    ```java
    //2. UserDetails를 받아서 JWT로 변환하는 메서드, 토큰 만들기.
    public String generateToken(UserDetails userDetails) {
        //jwt에 담고 싶은 정보를 Claims로 만든다.

        //현재 호출 되었을 때 epoch time, payload부분.
        Instant now = Instant.now();
        Claims jwtClaims = Jwts.claims()
                //sub: 누구인지
                .setSubject(userDetails.getUsername())
                //iat: 언제 발급 되었는지
                .setIssuedAt(Date.from(now)) //메서드가 호출된 시간
                //exp: 언제 만료 되었는지
                .setExpiration(Date.from(now.plusSeconds(20L)));
                //일반적인 jwt 외의 정보를 포함하고 싶다면 Map.put 사용 가능
                //jwtClaims.put("test","claim");

        //최종적으로 jwt를 발급한다.
        return Jwts.builder()
                .setClaims(jwtClaims)
                .signWith(this.signinqkey)
                .compact();
    }
    ```
    
3. 토큰 컨트롤러를 만들어서 토큰을 최종적으로 저장하고 발급한다.
    ```java
    public class TokenController {
        //jwt를 발급하기 위한 Bean
        private final JwtTokenUtils jwtTokenUtils;
        //사용자가 정보를 회수하기 위한 Bean
        private final UserDetailsManager manager;
        //사용자가 제공한 아이디 비밀번호를 비교하기 위한 클래스
        //현재는 서비스를 불리하지 않고 여기서 해보겠다.
        private final PasswordEncoder passwordEncoder;

        @PostMapping("/issue")
        public JwtResponseDto IssueJwt(
                @RequestBody JwtRequestDto dto
        ) {
            //사용자가 제공한 username, password가 저장된 사용자인지 판단.
            if (!manager.userExists(dto.getUsername())) //비어있지 않아야 한다. 저장되어진 아이디어야 하기 때문에
                throw new ResponseStatusException(HttpStatus.UNAUTHORIZED);
            UserDetails userDetails // 로그인 과정(사용자 인증 과정)
                    = manager.loadUserByUsername(dto.getUsername());

            //비밀번호 대조. 날것의 비밀번호와 암호화된 비밀번호가 서로 매치되는지 비교.
            if (!passwordEncoder
                    .matches(dto.getPassword(), userDetails.getPassword()))//날것비번, 암호화된 비번 대조.
                throw new ResponseStatusException(HttpStatus.UNAUTHORIZED);

            //JWT 발급, jwtTokenUtils.generateToken에 loadUserByUsername을 넣고,
            //최종적으로 토큰에 저장하고 발급한다.
            String jwt = jwtTokenUtils.generateToken(userDetails);
            JwtResponseDto response = new JwtResponseDto();
            response.setToken(jwt);
            return response;
        }
    }
    ```

4. 발급한 토큰이 정당한지 확인한다.

    ```java
    //JwtTokenUtils

    //3. 정상적인 JWT인지 판단하는 메서드
    public boolean validate(String token) {
        try {
            //정상적이지 않은 JWT라면 예외가 발생한다.
            jwtParser.parseClaimsJws(token);
            return true;
        } catch (Exception e) {
            log.warn("invalid jwt");
        }
        return false;
    }

    //4. 실제 데이터를(payload) 반환하는 메서드
    public Claims parseClaims(String token) {
        return jwtParser
                .parseClaimsJws(token)
                .getBody();

    }
    ```

    ```java
    //TokenController

    @GetMapping("/validate")
    public Claims validateToken(
            @RequestParam String token
    ) {
        if (!jwtTokenUtils.validate(token)) //정단한 토큰인지 확인하기 위함.
            throw new ResponseStatusException(HttpStatus.UNAUTHORIZED);
        return jwtTokenUtils.parseClaims(token); //정당한 토큰인지 확인이 되었으면 body출력.
    }
    ```

# 필터를 작성한다. Security에 이 사용자가 인증이 된 사용자인 것을 알려줘야 한다.
시큐리티에서 인증을 할 때는 필터를 통해서 인증을 하고, 지원하지 않는 방식으로 인증을 진행을 하고 싶으면 직접 필터를 만들어서 등록을 시켜주면 원하는 방식대로 인증을 할 수 있다.

- 필터 사용법.  
    extends OncePerRequestFilter //한번에 요청에는 단 한번만 실행하도록  
    @Override protected void doFilterInternal ///필터구현 메서드.  
    인증과 관련된 필터는 빈으로 등록하지 않는다. 왜냐하면 WebSecurityConfig 클래스에 수동으로 등록을 해 줄 것이기 때문에 자동 등록을 하지 않는다. 만약 빈으로 등록을 하였다면 빈 등록이 중복이 되기 때문이다.  

1. Authorization 헤더를 회수  
    String authHeader
        = request.getHeader(HttpHeaders.AUTHORIZATION);  
     //많이 사용하다 보니 스프링에서 상수로 존재한다.  

2. Authorization 헤더가 존재하는지 + Bearer로 시작하는지  확인한다.  
    (성공과 실패와 관계없이,,,헤더가 없더라도(jwt) 멈추지 않고 진행한다. doFilter 밖으로 보내줘야 한다. jwt가 없는 사용자는 인증이 없는 사용자도 있지만, 로그인을 하는 과정에 있는 사용자도 있다. 그래서 jwt가 없다고 통과를 못시키면 로그인을 하지 못하게 된다. 일단 여기서는 헤더가 있는지, Bearer로 시작하는지 사용자와 그렇지 않은 사용자만 나누면 되는 것이다. doFilter()에서 나간 후 AuthorizationFilter 클래스에서 jwt가 없는 사용자를 골라 상황에 맞게 처리를 할 것이다. 막거나 통과시키거나,,,  
    만약 다른 방법을 하고 싶다면 정상이거나 jwt가 없을 때는 통과시키고, 비정상일 경우에는 막겠다. 그 후 어느 시점에 doFilter을 호출할 것인지 명확하게 잘 정한다.)  
    if (authHeader != null && authHeader.startsWith("Bearer ")) { //=>인증이 성공하던 성공하지 않던 무조건 다음 필터를 호출해줘야 한다.(5번)  
    String token = authHeader.split(" ")[1]; //
   
3. Token이 유효한 토큰인지  
    // 3. Token이 유효한 토큰인지 확인. jwtTokenUtils의 validate메서드를 활용  
    if (jwtTokenUtils.validate(token))
   
4. 유효하다면 해당 토큰을 바탕으로 사용자 정보를 SecurityContext에 등록 
    SecurityContext context = SecurityContextHolder.createEmptyContext(); //비어있는  context를 만들고

    4-1.사용자 정보 회수
    ```java
    String username = jwtTokenUtils
        .parseClaims(token) //반환하는 메서드
        .getSubject(); //누구인지,,,

    UserDetails userDetails = manager.loadUserByUsername(username);
    for (GrantedAuthority authority :userDetails.getAuthorities()) {
    log.info("authority: {}", authority.getAuthority());
    }
    ```
    
    4-2.인증 정보 생성
    ```java
    AbstractAuthenticationToken authentication =
        new UsernamePasswordAuthenticationToken(
                  CustomUserDetails.builder()
                          .username(username)
                          .build(),
    ```
    
    4-1.인증 정보 등록
    ```java
    // 인증 정보 등록
    context.setAuthentication(authentication);
    SecurityContextHolder.setContext(context);
    ```
    
    5. 다음 필터 호출  
    // 5. 다음 필터 호출  
    // doFilter를 호출하지 않으면 Controller까지 요청이 도달하지 못한다.  
    filterChain.doFilter(request, response);

    필터를 다 만들었으면 WebSecurityConfig로 가서  
    .addFilterBefore를 사용해서 특정 필터 앞에 나만의 필터를 넣는다.

    ```java
        Slf4j
    //@RequiredArgsConstructor
    //여기 사용자가 보낸 JWT가 정당한지 정당하지 않은지 판단을 해주면 된다.
    public class JwtTokenFilter extends OncePerRequestFilter {
        private final JwtTokenUtils jwtTokenUtils;
        // 사용자 정보를 찾기위한 UserDetailsService 또는 Manager
        private final UserDetailsManager manager;

        public JwtTokenFilter(
                JwtTokenUtils jwtTokenUtils,
                UserDetailsManager manager
        ) {
            this.jwtTokenUtils = jwtTokenUtils;
            this.manager = manager;
        }

        @Override
        protected void doFilterInternal(
                HttpServletRequest request,
                HttpServletResponse response,
                FilterChain filterChain
        ) throws ServletException, IOException {
            log.debug("try jwt filter");
            // 1. Authorization 헤더를 회수
            String authHeader
                    // = request.getHeader("Authorization");
                    = request.getHeader(HttpHeaders.AUTHORIZATION); //많이 사용하다 보니 스프링에서 상수로 존재한다.
                    //getHeader은 null을 반환할 수 있는 메서드 그래서 Authorization 헤더가 존재하는지 + Bearer로 시작하는지가 필요하다.
            // 2. Authorization 헤더가 존재하는지 + Bearer로 시작하는지
            if (authHeader != null && authHeader.startsWith("Bearer ")) { //=>인증이 성공하던 성공하지 않던 무조건 다음 필터를 호출해줘야 한다.(5번)
                String token = authHeader.split(" ")[1]; //
                // 3. Token이 유효한 토큰인지 확인. jwtTokenUtils의 validate메서드를 활용.
                if (jwtTokenUtils.validate(token)) {
                    // 4. 유효하다면 해당 토큰을 바탕으로 사용자 정보를 SecurityContext에 등록
                    SecurityContext context = SecurityContextHolder.createEmptyContext(); //비어있는  context를 만들고
                    // 사용자 정보 회수
                    String username = jwtTokenUtils
                            .parseClaims(token)
                            .getSubject();

                    UserDetails userDetails = manager.loadUserByUsername(username);
                    for (GrantedAuthority authority :userDetails.getAuthorities()) {
                        log.info("authority: {}", authority.getAuthority());
                    }

                    // 인증 정보 생성
                    AbstractAuthenticationToken authentication =
                            new UsernamePasswordAuthenticationToken(
    //                                CustomUserDetails.builder()
    //                                        .username(username)
    //                                        .build(),
                                    // manager에서 실제 사용자 정보 조회
                                    // manager.loadUserByUsername(username),
                                    userDetails,
                                    token,
                                    userDetails.getAuthorities()
                            );
                    // 인증 정보 등록
                    context.setAuthentication(authentication);
                    SecurityContextHolder.setContext(context);
                    log.info("set security context with jwt");
                }
                else {
                    log.warn("jwt validation failed");
                }
            }
            // 5. 다음 필터 호출
            // doFilter를 호출하지 않으면 Controller까지 요청이 도달하지 못한다.
            filterChain.doFilter(request, response);
        }
        }
    ```

    ```java
    .addFilterBefore( //특정 필터 앞에 나만의 필터를 넣는다.
                        new JwtTokenFilter( //JwtTokenFilter는 빈이 아니지만 빈객체를 받는 것은 가능하다.
                                jwtTokenUtils,
                                manager
                        ),
                        AuthorizationFilter.class
                )
    ```

# 만들어보기!
1. jwt를 사용하기 위한 환경설정
2. jwt 생성과 검증을 위한 클래스 생성(JwtTokenUtils) - 암호화, [jwt에 정보 담기, jwt 생성]
3. jwt를 발급 받을 엔트포인트 만들기(TokenController) - 
    UserDetailsManager로부터 UserDeatils를 받아오면 generateToken()을 호출하여 jwt를 실제로 만들어 준다.  
    (사용자가 username과 password를 제공하고, 정당한 정보이면 응답으로 사용자의 정보에 맞게 jwt를 만들어 토큰에 저장한다.)  
4. jwt 기능 추가 - 들어온 토큰이 정당성이 있는 토큰인지 확인, 정당하면 jwt 반환해주기
5. getmapping으로 토큰을 반환한다. controller에서 확인.
6. filter를 바탕으로 jwt가 전달이 되었을 때 인증이 된 사용자라는 것을 알려주자.

    ### jwt를 사용하기 위한 환경설정
    1. application-mysql.yaml
        ```java
        spring:
            datasource:
                driver-class-name: com.mysql.cj.jdbc.Driver
                url: jdbc:mysql://127.0.0.1:3306/memo
                username: projectMemo
                password: # mysql workbench 비밀번호 입력.

        jpa:
            database: mysql
            hibernate:
                ddl-auto: create
            show-sql: true
        
        //JWT를 만들기 위해서는 JWT의 위변조 상태를 감지하기 위한 암호키. 간단하게 문자열로 작성.
        jwt:
            secret: aaaabbbsdifqbvaesoioegwaaaabbbsdifqbvaesoioegwaaaabbbsdifqbvaes
        ```
        
    2. application.yaml
            ```java
        spring:
            profiles:
                active: mysql    
        ```

    3. build.gradle jwt 추가하기.
        ```java
        //jwt
        implementation 'io.jsonwebtoken:jjwt-api:0.11.5'
        runtimeOnly 'io.jsonwebtoken:jjwt-impl:0.11.5'
        runtimeOnly 'io.jsonwebtoken:jjwt-jackson:0.11.5'
        ```

    
    ### jwt 생성 - 암호화, jwt 정보담기, jwt 발급하기
    jwt를 생성하기 위한 프레임워크 사용방법이라고 보면 된다.

    JwtTokenUtils class 만들기
    
    @Component를 추가하여 빈 등록.

    1. 암호화
    @Value를 사용하여 yaml에 설정한 암호키를 받아온다.
    *주의사항 lombok의 Value가 아니라 spring의 Value를 선택 해야 한다.
    ```java
    //springframework의 Value 선택!!
    import org.springframework.beans.factory.annotation.Value;
    @Slf4j
    @Component
    public class JwtTokenUtils {
        //1. 암호키 지정
        public JwtTokenUtils(
                //이렇게 하면 암호키를 받아올 수 있다.
                @Value("${jwt.secret}")
                String jwkSecret
        ){

        }
    }
    ```
    2. jwt에 담고 싶은 정보를 정한다.
        1. UserDetails의 규약을 사용하여 jwt에 담고 싶은 정보를 정한다.(claims 사용)
        2. jwt에 담고 싶은 정보를 정했으니 그 정보와 암호를 이용하여 jwt를 발급한다.(bulider)
    ```java
    //jwt를 발급하는 메서드
    public String generateToken(UserDetails userDetails) {
        //UserDetails의 규약을 따른다.
        //jwt에 담고 싶은 정보를 Claims로 만든다.
        //항상 담아두는 정보 표준:
        //sub:누구인지, iat: 언제 발급 되었는지(발급일), exp: 언제 만료 예정인지(만료일)
        //Claims는 jjwt에 정의 된 map 인터페이스를 상속 받는 인터페이스.
        //jjwt에 담길 정보를 의미한다.
        //map 처럼 데이터를 추가하여 jwt에 포함시킬 데이터를 정의할 수 있다. 일반적인 jwt 외의 정보를 포함하고 싶다면 Map.put 사용가능.
        //현재 사용자를 식별할 수 있는 username과 발급일과 만료일을 나타내는 iat, exp를 설정한다.
        //jwts 사용: jwt를 생성하고 구문 분석하는 정적 유틸리티 클래스.

        Instant now = Instant.now();//현재 호출 되었을 때 시간 (epoch time)을 받아온다.
        Claims jwtClaims = Jwts.claims()
                .setSubject(userDetails.getUsername())
                .setIssuedAt(Date.from(now)) //언제 발급 되었는지
                .setExpiration(Date.from(now.plusSeconds(20L))); //언제 만료 예정인지
    //        jwtClaims.put("test", "claim"); //jwt 외의 정보를 포함하고 싶다면 이렇게 사용가능(claims는 map 인터페이스를 상속 받는 인터페이스여서 가능)

        //최종적으로 jwt 발급
        return Jwts.builder()
                .setClaims(jwtClaims)
                .signWith(signinqkey)
                .compact(); 
                //compact() 메소드가 실제 Token을 문자열로 만들어 반환해 준다.
                //Actually builds the JWT and serializes it to a compact, URL-safe string according to the JWT Compact Serialization
    }
    ```
    3. 사용자의 요청을 바탕으로 위에서 만든 jwt를 토큰에 저장시킨다.
        토큰을 만든다.(로그인을 하지 않은 사용자를 가정해 만든다.)
        1. 클라이언트의 username이 존재하는지 확인한다.
        2. 있다면 username의 튜블의 비밀번호와 인코딩된 비밀번호가 일치하는지 대조한다.
        3. 비밀번호가 같다면 username에 관련된 튜플을 jwt에 넣고 jwt를 발급받고 토큰에 넣는다.
        *주의사항 비밀번호 확인: PasswordEncoder.matches() 사용. 날것의 비밀번호와 암호화된 비밀번호가 서로 매치되는지 비교할 수 있다.(equals ㄴㄴ)  
        *주의사항 오버라이딩 재사용: UserDetails userDetails // 로그인 과정(사용자 인증 과정)
        = manager.loadUserByUsername(dto.getUsername());  
        loadUserByUsername는 이미 jpaUserDetailsManager에서 이미 오버라이딩하여 재사용을 했기 때문에 재사용한 메서드를 사용하게 된다.

        ```java
        @Data
        public class JwtResponseDto {
            private String token;
        }

        @Data
        public class JwtRequestDto {
            private String username;
            private String password;
        }
        ```

        ```java
        @Slf4j
        @RestController
        @RequestMapping("token")
        @RequiredArgsConstructor
        public class TokenController {
            //jwt를 발급하기 위한 Bean
            private final JwtTokenUtils jwtTokenUtils;
            //사용자가 정보를 회수하기 위한 Bean
            //UserDetailService도 된다.=>서로 의존적인 관계를 덜 만들기에 적합하다.
            //+오롯이 사용자의 정보를 판단하기 위한 서비스를 만드는 것이기 때문에 좀 더 적합하다고 생각
            private final UserDetailsManager manager;
            //사용자가 제공한 아이디 비밀번호를 비교하기 위한 클래스
            //현재는 서비스를 불리하지 않고 여기서 해보겠다.
            private final PasswordEncoder passwordEncoder;

            //1. 클라이언트의 username이 존재하는지 확인한다.
            //2. 있다면 username의 튜블의 비밀번호와 인코딩된 비밀번호가 일치하는지 대조한다.
            //3. 비밀번호가 같다면 username에 관련된 튜플을 jwt에 넣고 jwt를 발급받고 토큰에 넣는다.
            @PostMapping("/issue")
            public JwtResponseDto IssueJwt(
                    @RequestBody JwtRequestDto dto
            ) {
                //사용자가 제공한 username, password가 저장된 사용자인지 판단.
                if (!manager.userExists(dto.getUsername())) //비어있지 않아야 한다. 저장되어진 아이디어야 하기 때문에
                    throw new ResponseStatusException(HttpStatus.UNAUTHORIZED);
                UserDetails userDetails // 로그인 과정(사용자 인증 과정)
                        = manager.loadUserByUsername(dto.getUsername());

                //비밀번호 대조.
                //암호문을 만들 때에는 sort라고 랜덤한 임의의 값이 추가가 된다.
                //그래서 인코딩이 된 암호(userDeatils.getPassword())와
                //날것의 암호를 다시 인코딩한 값(passwordEncoder.encode(dto.getPassword())을 비교한다고 해서
                //같은 값이 나올거라는 보장을 할 수 없다.
        //        if (userDetails.getPassword().equals(passwordEncoder.encode(dto.getPassword())))
                //그래서 PasswordEncoder.matches() 사용. 날것의 비밀번호와 암호화된 비밀번호가 서로 매치되는지 비교할 수 있다.
                //boolean matches(CharSequence rawPassword, String encodedPassword);
                if (!passwordEncoder
                        .matches(dto.getPassword(), userDetails.getPassword()))//날것비번, 암호화된 비번 대조.
                    throw new ResponseStatusException(HttpStatus.UNAUTHORIZED);

                //JWT 발급, jwtTokenUtils.generateToken에 loadUserByUsername을 넣고,
                //최종적으로 토큰에 저장하고 발급한다.
                String jwt = jwtTokenUtils.generateToken(userDetails);
                JwtResponseDto response = new JwtResponseDto();
                response.setToken(jwt);
                return response;
            }
        }
        ```
    4. jwt 기능 추가 - 토큰이 정당한지 확인한다. 정당하면 jwt 반환한다.

        만들어진 토큰이 정당한지 확인한다.  
        JwtTokenUtils에 정당한지 확인하는 메서드, 실제 데이터를 반환하는 메서드를 만든다.  
        역직렬화 하여 확인(private final JwtParser jwtParser;)  
        확인 후 정당한 jwt이면 페이로드 반환.(jwtParser.getBody())

        ```java
        private final JwtParser jwtParser;

        //정상적인 jwt인지 판단하는 메서드
        public boolean validate(String token){
            //Parses the specified compact serialized JWS string based
            //on the builder's current configuration state and returns the resulting Claims JWS instance.
            //This is a convenience method that is usable if you are confident that the compact string argument reflects a Claims JWS.
            //A Claims JWS is a JWT with a Claims body that has been cryptographically signed.
            //If the compact string presented does not reflect a Claims JWS, an UnsupportedJwtException will be thrown.
            //정상적이지 않은 jwt라면 예외(상황에 따라 다양한 예외)가 발생한다.
            try{
                jwtParser.parseClaimsJws(token); //sign을 사용하고 있으니 jwt가 아닌 jws를 사용할 것.
                return true;
            }catch (Exception e){
                log.warn("invalid jwt " + e.getMessage());
            }
            return false;
        }

        //jwt가 정상적이면 실제 데이터(payload)를 반환하는 메서드
        public Claims parseClaims(String token) {
            return jwtParser
                    .parseClaimsJws(token)
                    .getBody(); //Returns the JWT body, either a String or a Claims instance.
        }
        ```

    5. getmapping으로 토큰을 반환한다. controller에서 확인.
        ```java
        @GetMapping("/validate")
            public Claims validateToken(
                    @RequestParam String token
            ) {
                if (!jwtTokenUtils.validate(token)) //정단한 토큰인지 확인하기 위함. 역직렬화 하면서 검사.
                    throw new ResponseStatusException(HttpStatus.UNAUTHORIZED); //false가 반환되면 예외처리.
                return jwtTokenUtils.parseClaims(token); //정당한 토큰인지 확인이 되었으면 실제 데이터 body 반환.
            }
        ```

    6. Filter을 사용해서 jwt가 전달이 되었을 때 인증이 된 사용자라는 것을 알려주자.
       핕터 인터페이스 중 하나인 OncePterRequestFilter의 diFilterInternal()를 오버라이딩하여 사용. 요청당 한번만 호출된다.  
       Spring Security에서 인증을 할 때는 Filter를 통해서 인증을 진행하고,  
       지원하지 않는 방식으로 인증을 하고 싶을 경우 직접 filter을 만들어서 진행을 시켜주면 원하는 방식으로 인증을 할 수 있다.  
       여기에서 사용자가 첨부한 jwt를 찾아서 해당하는 jwt가 정상인지 아닌지 판단하고 정상일 때 포함되어 있는 정보를 바탕으로 이 사용자는 인증되었다는 것을 알려준다.

       Springboot에서 Filter 객체를 빈으로 등록하며 알아서 빈으로서 등록을 해준다.  
       하지만 인증관련 Filter은 빈으로 등록하지 않는다.  
       왜냐하면 webSecurityConfig 클래스에서 수동으로 등록을 해줘야 한다.  
       근데 빈으로 등록된 객체를 등록을 하면 스프링 컨테이너에서 자체적으로 등록하고 security에서 한번 더 등록을 해서 필터가 2번 등록될 수 있어서 빈으로 등록하지 않는다.

       
        

                    

