---
title:  "Tomcat  VS Nginx"
categories: [Web/Was]
tags: [Web/Was]
---

**1. 접속자가 많을 수록 Nginx > Apache ? **
    - Nginx는 Worker Process(Single Thread)를 이용, 각 요청이 올 때마다 Worker Process의 내부 Listner가 요청을 받아읽고, 쓰는 작업을 반복한다.  각 커넥션마다 프로세스와 Thread를 복제하지 않고, Worker Process 내부적으로 처리하기 때문에, 많은 요청이 올 경우에도 CPU/메모리 사용량이 급증하지 않는다.(Event Driven 방식)   
    - Apache는 요청(Connection) 하나당 프로세스와 쓰레드 기반으로 동작한다. 따라서 접속량이 증가할 수록 CPU와 메모리의 사용량이 증가하여 성능저하를 발생 시킬 수 있다.  아파치는 이러한 문제점을 해결 하기 위해 2.4.x 버전 이후로 event MPM 방식을 채용하여 Nginx의 Event Driven 방식을 흉내내고 있으나, Nginx에 비해서는 부족한 편이라고 평가한다.  
 
      **Ref : **  
	  [Nginx와 Apache 성능 비교](ttp://blog.naver.com/PostView.nhn?blogId=tmondev&logNo=220737182315&redirect=Dlog&widgetTypeCall=true)  
 
**2. Session Persistence**  
    - Nginx 의 경우 Session Persistence 기능을 Nginx Plus 버전을 통해서 제공하나, 상용이다.  
      따라서 무료로 배포되는 Nginx를 사용시에는 Redis를 활용하거나, DB session 등 별도의 방법을 이용하여 관리해야한다.  
 
    - Apache의 경우, Tomcat AJP connetor(Mod_jk)에서 Session Clustering 기능을 사용할 수 있다.  
 
      **Ref : **
	  [톰캣 Session Cluster 사용법](http://sarc.io/index.php/tomcat/111-tomcat-session-cluster-1)  
 
**3. Log Rotate**  
    - Nginx의 경우 linux OS에서 제공하는 LogRotate를 사용할수도 있으며, CronLog를 사용하기도 한다.  
 
    - LogRotate - Linux에서 제공하는 로그 로테이션 기능  
      [LogRotate 사용법](http://culturescrap.tistory.com/entry/logrotate-%EC%82%AC%EC%9A%A9%EB%B2%95%EB%A1%9C%EA%B7%B8-%EC%84%B8%EB%8C%80%EA%B4%80%EB%A6%AC)  
      [Nginx LogRotate 적용법](http://www.galgulee.com/nginx-log-rotate-%EC%8B%9C%ED%82%A4%EA%B8%B0-logrotate-%EC%82%AC%EC%9A%A9/)  
 
    - CronLog - 일/월/년 단위 로그 로테이션  
      [CronLog 설정](http://egloos.zum.com/lukasy/v/2448406)
 
     - Apache의 경우 RotateLog나 Cronlog를 사용한다.  
       [Apache RoateLog 사용법](https://httpd.apache.org/docs/trunk/ko/programs/rotatelogs.html)
 
     - Apache, Nginx 모두  Cronlog를 이용하는 것이 보관의 편의성 측면에서 좀더 낫다. 



