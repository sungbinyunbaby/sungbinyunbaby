# JWT
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



# JWT 생성
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

## Security에 이 사용자가 인증이 된 사용자인 것을 알려줘야 한다. 필터에

아직 필터부분 못함 4교시