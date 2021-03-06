---
title:  "Apache 설치"
categories: [Web/Was]
tags: [Web/Was]
---

### 1. 설치 작업 전 확인사항    

 - 시스템코드 확인 : 보통 5자리이하로 설정  
 - 엔진 설치여부 및 엔진관리 계정(apaadm) 생성 확인  
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
 - 사용 포트 선정 (가장 최근 설치된 도메인의 Port Set 확인)    
 
```java
 [root@SERVER01] netstat -an | grep 포트
```
---

### 2. 설치 작업    
 - apr, apr-util, pcre 다운로드 후 압축 풀어서 apache 컴파일일 경로에 옮기고, make && make install로 설치  
 - apache 컴파일 옵션 및 엔진 설치    

```java
// Apache 2.4 기준
[root@SERVER01] ./configure --prefix=/engn001/apaadm/apache24 --enable-modules=all --enable-mods-shared=most --enable-mpms-shared=all --enable-rewrite --enable-proxy --enable-so --enable-proxy-http --enable-proxy-connect --enable-cache --enable-mem-cache --enable-disk-cache --enable-deflate --enable-ssl --with-ssl=/usr/include/openssl (Apache 2.4) --with-included-apr --with-included-apr-util --enable-nonportable-atomics=yes (Apache 2.4에서는 event가 기본방식) --with-mpm=worker

[root@SERVER01] make && make install
```    

 - AJP compile 및 설치    
 - Apache 인스턴스 집합 디렉토리 생성    

> **Apache 홈 디렉토리 구조**  
- servers/ : 웹서버 하위 인스턴스를 설치하기 위한 디렉토리  
- bin/ : apache 인스턴스 기동 및 정지 등 실행에 관련된 쉘이 위치함  
- modules/ : *.so 등의 apache 에서 사용 가능한 모듈들의 파일이 위치  
- logs/ : apache에서 제공하는 기본 로그 디렉토리로 임의로 pid 등의 중요 로그를 위치시키는데 사용함  
- conf/ : apache에서 기본적으로 제공되는 template conf 파일이 위치하며 인스턴스별로 해당 디렉토리를 복사해서 사용함    

 - 인스턴스 생성    
 - 인스턴스 conf 디렉토리 생성    
 - 로그 디렉토리 생성    
 - 기동/정지 스크립트 작성    

```java
// 하위 인스턴스의 Conf 설정을 물고 Apache 기동
[apaadm@SERVER01] /engn001/apaadm/apache24/bin/apachectl -f /engn001/apaadm/apache22/servers/test_01/conf/httpd.conf -k start

// 하위 인스턴스의 Conf 설정을 물고 Apache 정지
[apaadm@SERVER01] /engn001/apaadm/apache22/bin/apachectl -f /engn001/apaadm/apache22/servers/test_01/conf/httpd.conf -k stop
```

 - http.conf 수정 항목
- Listen Port 수정
- 유저/그룹 지정(User nobody, Group nobody)
- 서버명 지정(localhost)
- DocumentRoot 지정 및 directory index 비활성화 조치
- welcome 페이지 설정
- 로그 형식 수정(년월일, 년월일시, 년월일시분 등)
- 2.4 mpm 사용할 경우 include 설정 주소 변경
- mod_jk 설정 추가
- 상태 모니터링 페이지 설정
- TRACE 메소드 무효화 (보안 이슈 대응)

---

Ref :  
[Apache 2.4 간단 구축 - 스승님!!](https://soonhyukyoon.github.io/2016/05/12/WebServer-Apache_HTTPD/){: target="_blank" }    