---
title:  "Proxy  VS Reverse Proxy"
categories: [Web/Was]
tags: [Web/Was]
---

### 1. Proxy (Forward Proxy)  
- 클라이언트가 대상 서버로 요청시에 Proxy 서버로 대상서버의 주소를 전달해 Proxy 서버가 데이터를 가져오는 방식    
   ex ) http://proxy.com/target?url=target.com  
![Forward Proxy](https://parkmh04.github.io//images/proxy.gif)  

### 2. Reverse Proxy  
- 클라이언트는 대상서버의 주소를 알지 못하며, Proxy 서버로 요청시에 Proxy 서버가 대상서버로 요청하여 데이터를 가져오는 방식.   
DMZ를 활용하여 내부서비스와 외부서비스를 구분하여 제공하는 경우에 외부망에 존재하는 서버가 공격받아 데이터가 유출될 경우,  
내부망에 존재하는 서버를 보호하기 위해 외부망에 서버는 연동을 하기 위해 내부망의 서버는 실제로 서비스를 제공하기 위해 구축하는 방식.    
     ex ) http://proxy.com/ (Proxy가 target.com으로 요청)    
-Apache의 경우 Reverse Proxy를 사용할 경우 기존정책이 톰캣의 8080, 8009 포트에만 접근이 가능하므로 임의의 포트로 연결할 수 없게 된다.  

![Reverse Proxy](https://parkmh04.github.io//images/reverse_proxy.gif)    
     Ref : Proxy  VS Reverse Proxy    
            [FowardProxy VS Reverse Proxy](http://www.lesstif.com/pages/viewpage.action?pageId=21430345){: target="_blank" }  
            [FowardProxy와 Reverse Proxy](http://idess.tistory.com/6){: target="_blank" }  
            [Nginx ReverseProxy 구성](http://www.joinc.co.kr/w/man/12/proxy){: target="_blank" }  
   
   



