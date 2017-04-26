---
title:  "HTTP"
categories: [Basic Network]
tags: [Basic Network]
---

**1. HTTP(Hyper Text Transfer Protocol)**    
:  HTTP란 웹서버와 클라이언트간의 문서를 교환하기 위한 통신규약이다.(실제로 어떤종류의 데이터든지 전송이 가능하다.)
    - HTTP는 웹환경에서 사용하는 TCP/IP 기반의 응용프로토콜이다. 
	- HTTP는 연결상태를 유지하지 않는 통신규약이다. (연결상태를 유지 - FTP, Telnet)
	- HTTP는 Request - Response가 1:1로 호환된다.(Sync)
	- HyperText의 의미는 데이터 자체를 전송하기보다는 링크기반으로 데이터에 접근하겠다는 의미이다.
    - Client - Server 방식으로 동작한다.(요청과 응답)

![HTTP](https://parkmh04.github.io/images/http.PNG)

    
**2. Connectless Protocol**    
: HTTP는 클라이언트가 서버에 요청하고, 응답을 받으면 연결을 유지하지 않고 끊어버린다. 이러한 방식으로 인해 하기와 같은 특징이 있다.
    - 장점 : 불특정 다수를 대상으로 서비스가 가능하다. 계속해서 새로운 클라이언트와 연결하고 연결을 해지하기 때문에 더 많은 클라이언트의 요청을 처리할 수 있다.
	- 단점 : 요청한 클라이언트와의 이전 요청에 대한 결과 등과 같은 클라이언트의 이전상태를 알 수 없다. 로그인 세션 등의 정보를 유지할 수 없다.
	            따라서 Cookie를 사용하여 클라이언트의 이전상태를 확인한다.(DB를 이용하여 클라이언트 정보와 상태를 저장하기도 한다.)
                Cookie는 클라이언트와 서버의 상태 정보를 가지고 있기 때문에 HTTP의 단점을 해결하기 위해 사용한다.
    
**3. HTTP Method**    
:	Method는 클라이언트 요청에 대한 정보이다. 요청방식.  
    일반적으로 HTTP POST, HTTP GET을 이용하여 개발을 진행한다.  그렇게 해도 상관없기 때문이다.
	하지만 실제로 HTTP 규격에서는 HTTP의 Method를 아래와 같이 구분하고 있다.
    
	- GET(SELECT) : 데이터를 요청한다.
	- POST(INSERT) : 데이터를  삽입한다.
	- PUT(UPDATE) : 데이터를 업데이트한다.
	- DELETE(DELETE) : 데이터를 삭제한다.
	- HEAD(CHECK) : HTTP  Header 정보만 요청한다. 해당 자원이 존재하는지 혹은 서버에 문제가 없는지를 확인하기 위해서 사용한다.
    - OPTIONS : 웹서버가 지원하는 HTTP Mehtod 목록을 요청한다.
	- TRACE : 클라이언트의 요청을 그대로 반환한다. 
   
	![Ref : HTTP의 Method](https://www.tutorialspoint.com/http/http_methods.htm)    
  
	- RESTFUL 과와 HTTP Method
	  : 흔히 Restful API를 설계/개발한다고들 한다. Restful의 제약사항/목표는 여러가지 것들이 존재하지만, HTTP Method 관점에서 Restful하다는 것은
	    HTTP Method들을 원래의 목적에 맞게 사용하는 것을 의미한다.
  
		!회원 가입 URI : POST    /system/member
		!회원 수정 URI : PUT      /system/member
		!회원 조회 URI : GET      /system/member
		!회원 삭제 URI : DELETE /system/member

**5. HTTP Status Code**    
:   HTTP Response 의 상태코드.
	- 1xx - Informational - 정보교환
	- 2xx - Success - 성공
	- 200 - OK	- 요청이 성공적으로 전송됨
	- 3xx - Redirection - 방향 지정
	- 301 - Moved Permanently - 요청 페이지의 영구적인 위치 변화
	- 302 - Found	- 요청 페이지이 일시적인 위치 변화
	- 4xx - Client Error - 클라이언트 오류
	- 404 - Not Found - 요청받은 자원을 서버에서 찾을 수 없을때 나타나는 상태 
	- 405 - Method Not Allowed - 서버에서 사용자가 요청한 주소의 메소드를 지원하지 않을때 나타남
	- 5xx - server Error - 서버 오류

		
**4. HTTP 1.0  VS HTTP 1.1 VS HTTP/2(HTTP 2.0)**
   - HTTP 1.0
     : Client,  IP Adress가 1:1
	   매번 필요시 Connection을 Open / Close
	   하나의 Request/Response에 요청 신규 Connection 생성

   - HTTP 1.1(HTTP 1.0 호환)
     : Client,  IP Adress가 1:N(하나의 IP Adress로 여러개의 WebSite 연결 가능, HTTP Request Header에 Host를 사용해야함.)
	   필요시 Multi Connection을 Open / Close	 
	   여러개의 Request/Response들이 기존 Connection 활용 가능
	   OPTION HTTP Method 지원
	   캐싱 기능 제공
	   상태코드 추가

   - HTTP 2.0(HTTP 1.1 호환)
    : HTTP Header 데이터 압축(웹페이지 렌더링 시 요청 횟수를 줄여줌)
	  Server Push 추가(Client 요청 없이 Server에서 Resource 제공 가능)
	  
	  ![Ref : HTTP의 변화](http://sejoong.github.io/dev/2015/02/15/dev/) 
	