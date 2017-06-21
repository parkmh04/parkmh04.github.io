---
title:  "Tomcat 설치"
categories: -[Web/Was]
tags: [Web/Was]
---

### 1. WAS 설치 전 고려사항    
- Active User <= 500 : 경량 WAS(Tomcat, Jetty, Resin)  
- Active User >= 500 : 중량 WAS(Wildfly, Weblogic, Websphere, JEUS)    

Ative User가 500명이 넘어도 Tomcat을 사용할 수 있다. 절대적인 기준은 아님.    

Tomcat을 설치하기 전 고려해야 할 사항은 Connector를 AJP로 할 것인지, HTTP로 할것인지부터,
프로토콜을 BIO(Default), NIO(Java Non-Blocking I/O), APR(Apache Portable Runtime)로 할 것인지 결정해야함.(NIO가 적절한 타협점이 될까)    

> **AJP** Connector  
- Apache와 Tomcat 연동시 사용하는 Connector 방식    

> **HTTP** Connector  
- Tomcat 혼자서 구동되거나, Nginx를 사용시에 사용하는 Connector 방식    

> **속도**  
- BIO < NIO < APR    

> **안정성**  
- APR < NIO < BIO    

APR을 사용할 경우, 별도 설치가 필요하다.(apr, apr-util)
```java
// APR
[root@SERVER01]# ./configure --prefix=/usr/local/apr
[root@SERVER01]# make && make install

// APR UTIL
[root@SERVER01]# ./configure --prefix=/usr/local/aprutil --with-apr=/usr/local/apr/
[root@SERVER01]# make && make install

// Tomcat Native Lib(다운은 알아서)
[root@SERVER01]# cd ~/tomcat-native-1.1.33-src/jni/native
[root@SERVER01]# ./configure  --prefix=/svc/tomcat/tomcat7/nativelib --with-apr=/usr/local/apr/bin/apr-1-config --with-java-home=$JAVA_HOME
[root@SERVER01]# make && make install
```    

---

### 2. 설치 작업    
 - Tomcat 다운 받아 대상 폴더에 압축해제  
 - Servers 폴더 생성 후 하위 인스턴스 생성(bin, lib 폴더를 제외하고 복사)
 - server.xml 수정(maxThread는 WebServer Connetion 설정보다 동일하거나 더 커야함. acceptCount = 10이하. connectionTimeout = Web보다는 길게, DB보다는 짧게)
 - Container Context Root 설정, appBase, docBase 설정, Db Connection 설정, Hot Deploy 설정 등을 한다.
 - logging.properties를 수정하여 불필요한 로그 비활성화
 - 기동/정지 스크립트 작성    

```java
// 기동 전에 로그 경로, 임시파일 제거 등을 수행하고 톰캣 기동
export SERVER_NAME="${APP_NAME}_10_${APP_PORT}"

# Catalina Base Directory
export CATALINA_HOME="/svc/tomcat/tomcat7"
export CATALINA_BASE="${CATALINA_HOME}/instances/${SERVER_NAME}"

# Catalina Log Directory
export CATALINA_LOG="${CATALINA_BASE}/logs"

# Template file clean
rm -rf ${CATALINA_BASE}/tmp/*
rm -rf ${CATALINA_BASE}/webapps/*
rm -rf ${CATALINA_BASE}/work/*

# Old log file clean
if [ 0 -eq `ls ${CATALINA_LOG} | grep ^catalina\.*\.log | wc -l` ]
   then echo "Previous server log does not exist"
   else
        find ${CATALINA_LOG}/ -name "catalina.*.log" -exec mv {} {}.${DATE} \;
        mv ${CATALINA_LOG}/catalina.*.* ${CATALINA_LOG}/backup/
fi
mv ${CATALINA_LOG}/gclog/gc_${SERVER_NAME}.log ${CATALINA_LOG}/gclog/backup/gc_${SERVER_NAME}.log.${DATE}

$CATALINA_HOME/bin/startup.sh

// 톰캣 다운
SERVER_NAME="${APP_NAME}_10_${APP_PORT}"

CATALINA_HOME="/svc/tomcat/tomcat7"
CATALINA_BASE="${CATALINA_HOME}/instances/${SERVER_NAME}"

# Catalina Base Directory
#export CATALINA_HOME="${CATALINA_HOME}"
#export CATALINA_BASE="${CATALINA_BASE}"

#$CATALINA_HOME/bin/shutdown.sh -force
ps -ef | grep java | grep ${SERVER_NAME} | grep -v 'grep'| awk {'print "kill -9 " $2'} | sh -x
```

---

Ref :  
[조대협의 블로그, 아파치 톰캣 튜닝가이드](http://bcho.tistory.com/788){: target="_blank" }    
[Tomcat Connector 설정](http://linuxism.tistory.com/520){: target="_blank" }    
[아파치 APR 설정법](https://kenu.github.io/tomcat70/docs/apr.html){: target="_blank" }    
[아파치 톰캣 설정법](https://tomcat.apache.org/tomcat-7.0-doc/config/http.html){: target="_blank" }    