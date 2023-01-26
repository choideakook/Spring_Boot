# HTTP method 의 활용

## ✏️ 클라이언트 — data —> 서버

### 📍 데이터의 전달 방식

1. 쿼리 파라미터를 통한 data 전송 방식
    - GET
    - 주로 정렬 필터에 사용됨 (검색어)
2. 메시지 바디를 통한 data 전송 방식
    - POST, PUT, PATCH
    - 회원 가입, 상품 주문, 리소스 등록, 리소스 변경

<br>

### 📍 클라에서 서버로 data 전송할 때 4가지 상황

1. 정적 데이터 조회
    1. 이미지, 정적 텍스트 문서
2. 동적 데이터 조회
    1. 예를 들어 검색, 게시판의 목록 에서 정렬 필터 (검색어)
3. HTML Form 을 통한 데이터 전송
    1. 회원 가입, 상품 주문, 데이터 변경
4. HTTP API 를 통한 데이터 전송
    1. 회원 가입, 상품 주문, 데이터 변경
    2. 서버 to 서버, 앱 클라이언트, 웹 클라이언트 (Ajax)
    
    🔗 HTTP API 설계 예시
    

<br>

## ✏️ 정적 데이터 조회 - GET

클라에서 단순한 url 경로만 message 에 담아서 서버로 보내면 서버도 단순하게 image resource 를 응답해 준다.

- 정적 data 기 때문에 별도 parameter 없이 resource 경로로 단순하게 조회 가능

```
클라이언트 -- GET --> 서버

- 요청 메시지 -
GET /static/.image.jpg HTTP/1.1
Host: localhost:8080

- 응답 메시지 -
HTTP/1.1 200 OK
Content-Type: image/jpeg
Content-Length: 34012

anwejfpoapnoeopoapejfpjpwe4339....
```

<br>

## ✏️ 동적 데이터 조회 - GET + query parameter

쿼리 파라미터를 사용해 동적 데이터를 조회한다.

- 주로 검색이나 게시판 목록을 정렬하는 기능에 사용
- 조회 조건을 줄여주는 필터, 조회 결과를 정렬하는 정렬 조건에 주로 사용
- 조회이기 때문에 마찬가지로 GET 을 쓰고 쿼리 파라미터를 사용해 data 전달

```
클라이언트 -- GET --> 서버 (쿼리 파라미터를 기반으로 정렬 필터해서 결과를 동적으로 생성)

- 요청 메시지 -
GET /serch?q=hello&hl=ko HTTP/1.1
Host: www.google.com
```

<br>

## ✏️ HTML Form 데이터 전송 - POST

![s5241.png](HTTP%20method%20%E1%84%8B%E1%85%B4%20%E1%84%92%E1%85%AA%E1%86%AF%E1%84%8B%E1%85%AD%E1%86%BC%205fcee9fc70e949c69004a03777893d7f/s5241.png)

- 웹 브라우저가 생성한 요청 HTTP 메시지
    - Content-Type - 서버에 요청할 메시지 바디의 컨텐츠 형식
        - application/x-www-form-urlencoded 가 디폴트 값이다.
        - post 이기 때문에 body 가 필요함
    - Content-Type 의 양식에 맞게 body 를 작성함
        - 클라가 입력한 username 과 age 가 쿼리형식과 유사한 형식으로 작성된다.
        - 정확히 말하면 Content-Type 에 맞는 형식으로 작성된다.

### 📍 GET 으로 HTML Form 데이터를 전송할 경우

- GET 은 body 가 없기 때문에 url 에 쿼리 파라미터 형식으로 data 를 전달하게 된다.

![s5242.png](HTTP%20method%20%E1%84%8B%E1%85%B4%20%E1%84%92%E1%85%AA%E1%86%AF%E1%84%8B%E1%85%AD%E1%86%BC%205fcee9fc70e949c69004a03777893d7f/s5242.png)

❗️ 억지로 사용할 수는 있지만 data 저장할 때 GET 을 사용하면 안된다.

- 올바른 query parameter 형식의 get 사용법

![s5242.png](HTTP%20method%20%E1%84%8B%E1%85%B4%20%E1%84%92%E1%85%AA%E1%86%AF%E1%84%8B%E1%85%AD%E1%86%BC%205fcee9fc70e949c69004a03777893d7f/s5242%201.png)

<br>

### 📍 multipart/form-data

단순 image 나 text 뿐 아니라 파일까지 주고 받아야 될 때 사용되는 content-type

- contente-type 은 multipart/form-data 이다.
- boundary=——XXX 는 클라이언트가 요청한 data 들을 구분선으로 구별해 서버가 알아보기 쉽게 만들어준다.

![s5244.png](HTTP%20method%20%E1%84%8B%E1%85%B4%20%E1%84%92%E1%85%AA%E1%86%AF%E1%84%8B%E1%85%AD%E1%86%BC%205fcee9fc70e949c69004a03777893d7f/s5244.png)

<br>

### 📍 정리

- HTML Form submit 시 POST 전송
    - 회원가입, 상품 주문, data 변경 …
- Content-Type: application/x-www-form-urlencoded
    - application/x-www-form-urlencoded 가 디폴트값이다.
    - form 의 내용을 메시지 바디를 통해서 전송
        - key = value, 쿼리 파라미터 형식
    - 전송 데이터를 url encoding 처리
        - abc김 → abc%EA%B9%80
- HTML Form 은 GET 전송도 가능
- Content-Type: multipart/form-data
    - 파일 업로드 같은 바이너리 데이터 전송시 사용
    - 다른 종류의 여러 파일과 폼의 내용 함께 전송 가능
- HTML Form 전송은 GET, POST 만 사용 가능하다.

<br>

## ✏️ HTTP API 데이터 전송

- 서버와 서버간의 백엔드 시스템 통신
- 앱 클라이언트
    - 안드로이드, 아이폰
- 웹 클라이언트
    - HTML 에서 Form 전송 대신 자바 스크립트를 통한 통신 (AJAX) 에 사용
    - React, VueJs 같은 웹 클라이언트와 API 통신
- POST, PUT, PATCH 는 메시지 바디를 통해 데이터 전송
- GET 은 query parameter 로 데이터 전달
- content-type: application/json 을 주로 사용
    - JSON 외에도 TEXT, XML, 등등.. 이 있다.
    - 사실상 JSON 이 표준값이다.

🔗 HTTP API 설계 예시