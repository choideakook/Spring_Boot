# 요청 Mapping Handler Adapter 구조

## ✏️ Spring MVC 의 구조

[🔗 Spring MVC 구조의 이해](https://github.com/choideakook/TIL/tree/main/Spring/8%20Spring%20MVC%20핵심기술/6%20Spring%20MVC%20구조%20이해)

- 요청 Mapping Handler Adapter 는 핸들러 어댑터에 위치하고 있다.
    - 요청 매핑 핸들러 어댑터의 정확한 로직은 `@RequestMapping` 을 처리하는 핸들러 어댑터인 `RequestMappingHandlerAdapter` 에 위치한다.

<img width="534" alt="s8611" src="https://user-images.githubusercontent.com/115536240/219523670-4acb2e73-c905-419c-a08d-3319db96d8c9.png">

<br>

## ✏️ RequestMappingHandlerAdapter 의 동작 방식

![s891.png](%E1%84%8B%E1%85%AD%E1%84%8E%E1%85%A5%E1%86%BC%20Mapping%20Handler%20Adapter%20%E1%84%80%E1%85%AE%E1%84%8C%E1%85%A9%20463af083c9534efca71fa7a49939773c/s891.png)

### 📍 ArgumentResolver

- `RequestMappingHandlerAdapter` 의 핵심 기능이다.
- HttpServlet request / response , Model , @RequestParam , @ModelAttribute , @RequestBody , HttpEntity 같이 HTTP 메시지를 처리하는 부분까지 매우 큰 부분을 유연하게 담당하고 있다.
- Controller (핸들러 어댑터) 가 필요로 하는 다양한 Param 을 생성해,
값이 모두 준비되면 Controller 에게 반환해준다.
    - 값을 반환받은 Controller 는 값을 Param 으로하는 핸들러를 호출한다.
- ⚠️ 참고로 Spring 은 30 개 이상의 ArgumentResolver 를 제공하고 있다.

<br>

### 📍 ReturnValueHandler

- `RequestMappingHandlerAdapter` 의 반환값 핵심 기능이다.
- 응답 값을 반환하고 처리하는 기능을 담당한다.

<br>

### 📍 HTTP Message Converter

- ArgumentResolver 와 ReturnValueHandler 에서 작동된다.

![s8922.png](%E1%84%8B%E1%85%AD%E1%84%8E%E1%85%A5%E1%86%BC%20Mapping%20Handler%20Adapter%20%E1%84%80%E1%85%AE%E1%84%8C%E1%85%A9%20463af083c9534efca71fa7a49939773c/s8922.png)

- ***요청의 경우***
    - 각각의 ArgumentResolver 가 처리할 수 있는 data 인지 확인하기 위해 Message Converter 가 하나하나 getRead() 로 체크하고,
    처리가 가능하다면 필요한 객체를 생성한다.
- ***응답의 경우***
    - 각각의 ReturnValueHandler 는 Message Converter 를 호출해 getWrite() 로 체크후, 응답 결과를 만든다.

<br>

### 📍 확장성

Spring 은 이 기능을 모두 interface 로 제공하기 때문에 필요하다면 언제든지 직접 확장 해보는 것도 가능하다.

- HandlerMethodArgumentResolver
- HandlerMethodReturnValueHandler
- HttpMessageConverter