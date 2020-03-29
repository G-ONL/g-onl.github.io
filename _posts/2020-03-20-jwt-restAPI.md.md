---
layout: post
title: "jwt와 RestApi"
comments: true
category: Spring
decription: >
  Jwt가 무엇인지, 어떻게 사용하는 것인지에 대해 알아봅시다.
---

## Jwt를 이용한 회원가입, 로그인, 로그아웃


로그인에 대한 전략이 다양하게 있지만 크게 많이 사용되는 것은 세션/쿠키를 이용한 방법과 OAuth, 그리고 이번에 사용한 Jwt(Json Web Token)을 이용한 방법이 아닐까 싶다.

이 세 가지에 대한 내용은 그림으로 보는 것이 말로 설명한 것보다 이해가 빠르다.


바로 Jwt를 적용한 Spring Boot 프로젝트를 확인해보자.

### 제일 먼저 Build.gradle 파일에 jjwt를 적어준다.

~~~
    compile 'io.jsonwebtoken:jjwt-api:0.11.1'
    runtime 'io.jsonwebtoken:jjwt-impl:0.11.1',
            'io.jsonwebtoken:jjwt-jackson:0.11.1' // or 'io.jsonwebtoken:jjwt-gson:0.11.1' for gson
~~~

### User Entity, Repository, Controller, Service를 작성해준다.


### JwtService를 작성 해준다.

~~~java
@Slf4j
@Service
public class JwtService {

  private static final String secretKey = "!122edasdji32irj23rjp2qwasdasdasdasd@!@@@!@#@$(&$#(&*(!#@&(*@&#(*@&$(*@&(*$&@(*$&(*#&$(*&(*&@(*&#(*@$&(*#&$@#@#^^&#^$#&$^#$#&@^#&*@#^@&#*@$@dposadjlkasjdlaj@!";
  private byte[] apiKeySecretBytes = DatatypeConverter.parseBase64Binary(secretKey);
  private SignatureAlgorithm signatureAlgorithm = SignatureAlgorithm.HS256;
  private final Key KEY = new SecretKeySpec(apiKeySecretBytes, signatureAlgorithm.getJcaName());


  public <T> String create(String key, T data, String subject) {
    String jwt = Jwts.builder()
        .setHeaderParam("typ", "JWT")
        .setHeaderParam("regDate", System.currentTimeMillis())
        //1000 * 60 * 60 * 24 = 1일
        //유효기간 1일
        .setExpiration(new Date(System.currentTimeMillis() + 1 * (1000 * 60 * 60 * 24)))
        .setSubject(subject)
        .claim(key, data)
        .signWith(KEY, this.signatureAlgorithm)
        .compact();

    return jwt;
  }

  public Boolean getExpToken(String jwt) {
    try {
      Date expiration = Jwts.parserBuilder().setSigningKey(KEY).build().parseClaimsJws(jwt)
          .getBody().getExpiration();
      Date now = new Date();

      if (expiration.after(now)) {
        return true;
      }
      return false;
    } catch (Exception e) {
      return false;
    }
  }

  public String getUserEmail(String jwt) {
    try {
      return String.valueOf(
          Jwts.parserBuilder().setSigningKey(KEY).build().parseClaimsJws(jwt).getBody()
              .get("userName"));
    } catch (Exception e) {
      return null;
    }
  }
}

~~~

### JwtInterceptor를 작성해준다.

~~~java
@RequiredArgsConstructor
@Component
public class JwtInterceptor implements HandlerInterceptor {

  private static final String HEADER_AUTH = "authorization";
  private final JwtService jwtService;


  @Override
  public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
      throws Exception {
    final String token = request.getHeader(HEADER_AUTH);
    if(token != null && jwtService.getExpToken(token)){
      request.setAttribute("userName", jwtService.getUserEmail(token));
      return true;
    }else{
      throw new Exception();
    }
  }
}
~~~

### WebConfig 파일을 작성 후에 JwtInterceptor를 등록해준다.

~~~java
@RequiredArgsConstructor
@Configuration
public class WebConfig implements WebMvcConfigurer {

  private static final String[] EXCLUDE_PATHS = {
      "/user/**",
      "/error/**"
  };

  private final JwtInterceptor jwtInterceptor;


  @Override
  public void addInterceptors(InterceptorRegistry registry) {
    registry.addInterceptor(jwtInterceptor).addPathPatterns("/**")
        .excludePathPatterns(EXCLUDE_PATHS);

  }
}
~~~


* 더 생각해 볼 사항

권한, 만료시에 연장을 어떤식으로? (갑자기 로그인 다시?) 
<!--stackedit_data:
eyJoaXN0b3J5IjpbOTMyOTg0NzQ2XX0=
-->