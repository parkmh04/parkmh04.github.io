---
title:  "Nginx 설치"
categories: [Web/Was]
tags: [Web/Was]
---

### 1. 설치 작업 전 확인사항    

 - 시스템코드 확인 : 보통 5자리이하로 설정  
 - 엔진 설치여부 및 엔진관리 계정(nginxadm) 생성 확인  
 - 엔진 미설치시 openssl / GCC compiler 설치여부 확인    
> openssl과 openssl-devel (openssl lib로 openssl 관련 헤더 파일정보가 있어 compile시 필요)  
> GCC는 apache 바이너리 설치파일 compile시 필요    

```java
[root@SERVER01]# rpm -qa | grep openssl  

// Bash 실행
-bash-3.2$ rpm -qa | grep gcc
```    

 - openssl, GCC Complier 없는 경우 설치    
 
```java
 [root@SERVER01] yum install gcc gc gcc-c++ make apr-util openssl openssl-devel zlib zlib-devel unzip perl
```
 - application 관리 계정 생성 확인  
 - 사용 포트 선정 (가장 최근 설치된 도메인의 Port Set 확인 )    
 
```java
 [root@SERVER01] netstat -an | grep 포트
 [root@SERVER01] service iptables status // ip table 방화벽 확인
```
---

### 2. 설치 작업    
 - 로그 및 설치 파일 경로 생성(nginxadm 으로 진행)
 - nginx, CRONOLOG, ZLIB, PCRE, NGINX 다운로드
 - apache 컴파일 옵션 및 엔진 설치    

```java
cd nginx-1.10.2
[root@SERVER01] ./configure --prefix=/engn001/nginxadm/nginx/nginx-1.10.2 --user=nginxadm --group=nginxadm --with-pcre=/engn001/nginxadm/nginx/installer/pcre-8.39 --with-zlib=/engn001/nginxadm/nginx/installer/zlib-1.2.8 --with-http_ssl_module

[root@SERVER01] make && make install
```    
 - Nginx 인스턴스 집합 디렉토리 생성    
 - 인스턴스 생성    
 - 인스턴스 conf 디렉토리 생성    
 - 로그 디렉토리 링크 연결    
 - 기동/정지 스크립트 작성    

```java
// 하위 인스턴스의 Conf 설정을 물고 Nginx 기동
[nginxadm@SERVER01] /engn001/nginxadm/nginx/nginx-1.10.2/sbin/nginx -c /engn001/nginxadm/nginx/nginx-1.10.2/servers/test_01/conf/nginx.conf

// 하위 인스턴스의 Conf 설정을 물고 Nginx 정지
[nginxadm@SERVER01] /engn001/nginxadm/nginx/nginx-1.10.2/sbin/nginx -s stop
```

- nginx.conf 수정 항목  
- location 정보 수정  
- Listen Port 수정  
- ssl 설정  

---

Ref :  
[Apache 2.4 간단 구축 - 스승님!!](https://soonhyukyoon.github.io/2016/05/12/WebServer-Apache_HTTPD/){: target="_blank" }    
[Nginx 설정](http://sarc.io/index.php/nginx/61-nginx-nginx-conf){: target="_blank" }    