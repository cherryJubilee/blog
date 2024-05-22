---
draft: false
authors:
  - hyewon
date: 2024-01-01
categories:
  - Web
  - HTTP
---

# HTTP란?

> HTTP의 특징과, HTTP request/response 구조 그리고 더 안전해진 HTTPS 이해하기

<!-- more -->

## HTTP 정의

- HyperText Transfer Protocol
- 웹 브라우저와 웹 서버가 통신을 할 때 사용하는 통신규칙
- HTTP는 서버와 클라이언트 간에 HTTP라는 양식에 맞춰 데이터를 요청하고 응답하는 방식이다.
- Request는 클라이언트가 서버로 전달하는 메시지이고, Response는 Request에 대한 서버의 답변이다.

## HTTP 특징 (Connectionless / Stateless)

HTTP는 서버에 연결 후 요청에 대한 응답을 받으면 연결을 끊어버리는 Connectionless 특성을 가진다.
이로 인해 많은 사람들이 웹을 이용하더라도 실제 동시 접속을 최소화하여 더 많은 유저의 요청을 처리할 수 있다.  
하지만 연결을 끊었기 때문에, 클라이언트의 이전 상태(login 유무)를 알 수가 없다는 Stateless의 특성이 생기게 된다. 정보를 유지할 수 없는 Connectionless와 Stateless의 특성을 가진 HTTP의 단점을 해결하기 위해 cookie, session, jwt가 도입되었다.

## 1. HTTP request

HTTP request 메세지는 크게 3부분으로 구성된다

- start line
- header
- message body

![request](./woong1/request.png)

### 1-1. Start Line

말 그대로 HTTP request의 첫 라인으로 HTTP 메소드, Request target, HTTP version이 있다.

- **HTTP 메소드**: 요청의 의도를 담고 있는 GET, POST, PUT, DELETE 등이 있다.
- **Request target**: HTTP Request가 전송되는 목표 주소입니다.
- **HTTP version**: version에 따라 Request 메시지 구조나 데이터가 다를 수 있어서 version을 명시한다.

```
GET / search?q=hello&hi-ko HTTP/1.1
```

### 1-2. Header

해당 request에 대한 추가 정보(addtional information)를 담고 있는 부분이다.

```
Key:Value 형태
```

자주 사용되는 header 정보

- **Host** : 요청한 서버의 도메인명 또는 IP 주소
- **User-Agent**: 요청을 보내는 클라이언트의 웹브라우저에 대한 정보.
- **Referer** : 바로 직전에 머물렀던 웹 링크 주소
- **Accept**: 클라이언트가 받을 수 있는 응답(response) 타입.
- **Connection**: 해당 요청이 끝난후에 클라이언트와 서버가 계속해서 네트워크 컨넥션을 유지 할것인지 아니면 끊을것인지에 대해 지시하는 부분.
- **Content-Type**: 해당 요청이 보내는 메세지 body의 타입. 예를 들어, JSON을 보내면 application/json.
- **Content-Length**:메세지 body의 길이.
- **Authorization**: 인증 토큰 정보 등

### 1-3. Body

POST, PUT 요청과 같이 **요청과 함께 전달되는 데이터** 를 포함한다. 요청과 함께 전달되는 데이터가 없을 경우 Body가 없을 수도 있다. GET request들은 url에 정보를 담아서 body가 없는 경우가 많고, POST 요청일 경우, body에 데이터를 담아간다.

---

## 2. HTTP Response

Response도 request와 마찬가지로 크게 3부분으로 구성되어 있다.

- Status line
- Headers
- Body

![response](./woong1/response.png)

### 2-1. Status Line

Response의 상태를 간략하게 나타내주며 3부분으로 구성되어 있다.

- HTTP Version
- Status code: 응답 상태를 나타내는 코드이다. (예를 들어, 200, 404 등)
- Status text: 응답 상태를 간략하게 설명해주는 부분.예를 들어, "Not Found"

```
HTTP/1.1 404 Not Found
```

### 2-2. Headers

Request의 headers와 동일 하지만 response에서만 사용되는 header 값들이 있다. 예를 들어, User-Agent 대신에 Server 헤더가 사용된다.

- **Location** : 301, 302 상태코드일 때만 볼 수 있는 Header로 서버의 응답이 다른 곳에 있다고 알려주면서 해당 위치(URI)를 지정한다.
- **Status Code**: 응답 상태 코드 (200, 404, 500 등)
- **Server** : 웹 서버의 종류
- **Age** : max-age 시간내에서 얼마나 흘렀는지 초 단위로 알려주는 값
- **Referrer-policy** : 서버 referrer 정책을 알려주는 값 ex) origin, no-referrer, unsafe-url
- **WWW-Authenticate** : 사용자 인증이 필요한 자원을 요구할 시 , 서버가 제공하는 인증 방식
- **Proxy-Authenticate** : 요청한 서버가 프록시 서버인 경우 유저 인증을 위한 값  


### 2-3. Body

Response의 body와 일반적으로 동일하다. Request와 마찬가지로 모든 response가 body가 있지는 않다. 응답할 데이터가 없을 경우 body가 비어있게 된다.

---

## HTTP와 HTTPS 차이점

HTTP는 암호화가 되지 않은 평문 데이터를 전송하는 프로토콜이였기 때문에, HTTP로 비밀번호나 주민등록번호 등을 주고 받으면 제3자가 정보를 조회(스니핑)할 수 있었다. 이러한 문제를 해결하기 위해 HTTPS가 등장하게 되었다.

HTTPS를 사용하는 2가지 이유

1. 내가 사이트에 보내는 정보(id, pw)들을 제 3자가 보지 못하도록 암호화 한다.
   - 대칭키 / 비대칭키 (공부해보기)
   - 암호 가능한 키를 클라이언트한테 주기 + 해독 가능한 키는 google서버만 갖고 있고 (완전 비밀)
2. 접속한 사이트가 믿을만한 곳인지를 알려준다.
   - HTTPS를 사용하는 웹사이트는 신뢰할 수 있는 기관(인증 기관, CA)으로부터 SSL/TLS 인증서를 받게 되며 인증서를 가지고 있지 않은 브라우저는 경고를 표시를 해준다.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
