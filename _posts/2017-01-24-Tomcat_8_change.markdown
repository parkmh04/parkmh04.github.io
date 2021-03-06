---
title:  "Tomcat 8 변경점"
categories: [Web/Was]
tags: [Web/Was]
---

대표적으로 Tomcat이 사용하던 Database Connecting Pool Libray가 변경.  
기존에 사용하던 Apache dbcp Library를 기본으로 사용하고 있었음.(DBCP 1.X)    
기존의 것이 구리기 때문에 Apache Tomcat DBCP Library를 사용하는 것으로 변경하였고.  이에따라 설정의 키들이 변경됨.  
 
![DBCP Library 설명](https://parkmh04.github.io//images/tomcat8_change.gif)   
    
< 기타 변경점 >  
 -  JAVA 7 이상을 사용해야함.  
 -  서블릿 3.1 적용(3.0부터 비동기식 입출력 방식 지원)  
 -  JSP 2.3 적용   
 -  EL 3.0 적용 (JSP가 아닌 JAVA에서도 EL을 사용할 수 있도록 ELProcessor API 제공)  
 -  웹 소켓 1.0 적용  
 -  NIO 컨넥터가 기본적용 됨  
     : 기존에는 BIO가 Default, 성능은 APR > NIO > BIO, 안정성은 BIO < NIO < APR,   
       따라서 NIO로 Tunning하는 경우가 많은데, 굳이 건드릴 필요가 없게됨.  
   
 **Ref** :  
 [아파치 톰캣 마이그레이션 가이드](https://tomcat.apache.org/migration-8.html){: target="_blank" }  
 [톰캣 8.0 변경점](http://start.goodtime.co.kr/2014/02/%ED%86%B0%EC%BA%A3-8-%EC%86%8C%EA%B0%9C/){: target="_blank" }  


