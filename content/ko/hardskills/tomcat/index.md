---
title: 톰캣의 구조와 동작 메커니즘
summary: 아파치 톰캣이 무엇이고 어떻게 동작하며, 어떤 구조를 가지고 있는지에 대해서 알아봅니다.
date: 2024-02-28
author:
  - jaeheon
# Featured image
# Place an image named `featured.jpg/png` in this page's folder and customize its options here.
image:
  caption: 'Image credit: [**Unsplash**](https://unsplash.com)'
---

웹 프로그래밍을 공부하면서 톰캣을 사용하는데, 정작 톰캣에 대해서는 아무것도 모른다는 생각이 들었습니다. 책에서도 톰캣을 실행시켜서 작동시키는 부분에 대해서는 설명이 되어있었으나, 톰캣에 대해서 자세히 알려주는 부분은 없었습니다. 그리고 톰캣이 웹 애플리케이션 서버인지 아니면 서블릿 컨테이너인지에 대한 궁금증이 생겼습니다. 따라서 아파치 톰캣, 구조, 톰캣의 작동 원리에 관해서 자세히 알아보고자 합니다.


# 톰캣은 무엇인가
Apache Tomcat톰캣에 대해서 부는 이름은 가지각색입니다. 어느 곳에서는 웹 애플리케이션 서버라고 하며, 또 어떤 곳에서는 서블릿 컨테이너라고 부르기도 합니다. 우선 이에 대해서 알아보기 위해서는 웹 서버와 웹 애플리케이션 서버(WAS: Web Application Server)에 대한 이해가 필요합니다.

우선 웹 서버란 웹 브라우저 클라이언트로부터 HTTP 요청을 받아서 정적인 콘텐츠를 제공해 주는 서버를 칭합니다. 하지만 현실 세계에서는 이렇게 정적인 콘텐츠만을 제공해 주는 일은 극히 드물어 보입니다. 따라서 동적 콘텐츠를 제공하기 위해서 WAS(Web Application Server)를 이용합니다.

클라이언트가 HTTP 요청을 보내면 웹 서버는 이를 받아서 다시 WAS에 보냅니다. 그러면 WAS는 비즈니스 로직을 처리한 후 다시 웹 서버로 보내면 웹 서버는 받은 데이터를 클라이언트에게 제공해 주는 과정을 거칩니다.

WAS에 대해서 더욱 자세히 살펴보겠습니다. WAS란 J2EE 스펙을 구현한 서버로서 분산 트랜잭션, 보안, 스레드 처리 등의 기능을 처리하는 분산 환경에서 사용되는 미들웨어입니다. 즉 웹 서버 + 웹 컨테이너로 웹상에서 사용하는 컴포넌트를 올려 사용하는 서버입니다. 엄밀하게 이야기하자면 톰캣은 J2EE 스펙 중 EJB 기술이 적용되어 있지 않기에 웹 애플리케이션 서버(WAS)는 아니라고 할 수 있습니다. 아파치 톰캣은 비즈니스 로직을 처리할 때 서블릿 관리를 해준다는 점에서 서블릿 컨테이너에 가깝습니다.

참고로 웹 애플리케이션 서버는 Jave EE 전체를 지원하기 때문에 웹 애플리케이션 서버는 웹 컨테이너보다 많은 기술들을 다룰 수 있습니다.
즉, Apache Tomcat(아파치 톰캣) 은 기본적으로는 서블릿 컨테이너이지만 자체 웹 서버가 내장이 되어있습니다. 이 때문에 톰캣은 WAS 기능을 일부 가지고 있는 서블릿 컨테이너 라고 보는 것이 합당해 보입니다.


# 톰캣의 구조


```<?xml version='1.0' encoding='utf-8'?>
<Server port="8005" shutdown="SHUTDOWN">
   <Listener className="org.apache.catalina.core.AprLifecycleListener" SSLEngine="on" />
   <Listener className="org.apache.catalina.core.JasperListener" />
   <Listener className="org.apache.catalina.core.JreMemoryLeakPreventionListener" />
   <Listener className="org.apache.catalina.mbeans.GlobalResourcesLifecycleListener" />
   <Listener className="org.apache.catalina.core.ThreadLocalLeakPreventionListener" />
   <GlobalNamingResources>
     <Resource name="UserDatabase" auth="Container"
               type="org.apache.catalina.UserDatabase"
               description="User database that can be updated and saved"
               factory="org.apache.catalina.users.MemoryUserDatabaseFactory"
               pathname="conf/tomcat-users.xml" />
   </GlobalNamingResources>
   <Service name="Catalina">
     <Connector port="8080" protocol="HTTP/1.1"
                connectionTimeout="20000"
                redirectPort="8443" />
     <Connector port="8009" protocol="AJP/1.3" redirectPort="8443" />
     <Engine name="Catalina" defaultHost="localhost">
       <Realm className="org.apache.catalina.realm.LockOutRealm">
         <Realm className="org.apache.catalina.realm.UserDatabaseRealm"
                resourceName="UserDatabase"/>
       </Realm>
       <Host name="localhost"  appBase="webapps"
             unpackWARs="true" autoDeploy="true">
         <Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs"
                prefix="localhost_access_log." suffix=".txt"
                pattern="%h %l %u %t &quot;%r&quot; %s %b" />
       </Host>
     </Engine>
   </Service>
</Server>
```

# 서버의 태그 확인
다음은 서버에 관한 설정파일인 server.xml입니다. 
이를 통해 용어에 대해서 이해한 다음, 톰캣의 구조에 대해서 살펴보겠습니다.

컨텍스트(Context): 컨텍스트는 톰캣 내에서 단일 웹 애플리케이션을 의미합니다.

커넥터(Connector): 커넥터는 클라이언트와 통신을 담당하는데, 이는 HTTP 요청을 받아들이고 처리하는 역할을 합니다. 톰캣의 기본 커넥터는 HTTP/1.1 표준에 따라 클라이언트로부터의 요청을 처리합니다. 또한, 커넥터는 특정 포트(기본적으로 8080)에서 요청을 기다립니다. redirectPort 속성은 SSL(HTTPS) 연결이 필요한 경우, 요청을 안전한 포트(기본적으로 8443)로 리디렉션 합니다.
호스트(Host): 호스트는 Tomcat 서버에 대한 네트워크 이름(www.myname.com)의 연결을 의미합니다. 호스트는 임의의 수의 컨텍스트(응용프로그램)를 포함할 수 있습니다. Tomcat의 기본 구성에는 localhost라는 이름의 호스트가 포합됩니다

엔진(Engine): 엔진은 톰캣의 핵심 역할을 담당합니다. 서비스에는 여러 개의 커넥터가 있을 수 있으므로 엔진은 이러한 커넥터로부터 모든 요청을 수신하고 처리하여 클라이언트에 전송할 적절한 커넥터로 응답을 다시 전달합니다. 엔진에는 하나 이상의 호스트가 포함되어야 하며, 그중 하나가 기본 호스트로 지정되어 있어야 합니다. 기본 Tomcat 구성에는 localhost 호스트를 포함하는 Catalina 엔진이 포함됩니다

리스너(Listener): 리스너는 org.apache.catalina.LifecycleListener 인터페이스를 구현하여 특정 이벤트에 응답할 수 있는 Java 객체입니다.

렐름(Realm): 렐름 컴포넌트는 모든 컨테이너 구성 요소(엔진, 호스트, 컨텍스트) 안에 나타날 수 있습니다. 사용자, 비밀번호, 사용자 역할의 데이터베이스를 나타냅니다. 그 목적은 컨테이너 기반 인증을 지원하는 것입니다.

밸브(Valve): 밸브는 컨테이너(컨텍스트, 호스트 또는 엔진)에 삽입하면 애플리케이션에 도달하기 전에 들어오는 모든 HTTP 요청을 가로채는 인터셉터와 같은 요소입니다. 이를 통해 특정 애플리케이션, 가상 호스트에서 실행되는 애플리케이션 또는 엔진 내에서 실행되는 모든 애플리케이션으로 향하는 요청을 사전 처리할 수 있습니다.


# 톰캣 디렉터리 구조
톰캣 설치 폴더bin: 톰캣 실행과 종료에 관한 스크립트 파일이 위치합니다.
conf: server.xml 와 같은 서버 전체 설정에 관한 설정 파일이 위치합니다. 이 중 server.xml은 서버 설정과 관련된 접근/접속에 관한 내용들이 담겨있습니다. 설정되어 있는 포트와 충돌 나는 일을 방지하기 위해 필요에 따라 포트를 변경할 수 있습니다. 더해서 webapps 폴더가 아닌 다른 경로로 변경하기 위해서는 < context docBase=""/ > 부분에 프로젝트 경로를 입력해 주면 됩니다. web.xml은 톰캣의 환경설정 파일이며 서블릿, 필터, 인코딩을 설정할 수 있으며, 서버가 올라갈 때 가장 먼저 읽는 파일입니다.

lib: 톰캣과 다른 웹 서버를 연결해 주는 바이너리 모듈들과 톰캣 구동시 필요한 jar 라이브러리들이 위치합니다.

logs: 예외 발생 사항 등 톰캣 로그 파일들이 위치합니다.

temp: 톰캣이 실행되는 동안 임시파일들이 위치하는 임시 저장용 폴더입니다.

webapps: 웹 애플리케이션이 위치합니다.

work: JSP파일을 서블릿 상태로 변환한 java파일과 class 파일들이 저장되는 위치입니다.

## 웹 애플리케이션 컨테이너에 등록
컨테이너에 웹 애플리케이션을 등록하는 방법은 두 가지입니다.
- %CATALINA_HOME%webApps 디렉터리에 애플리케이션 저장
- server.xml 파일에 직접 웹 애플리케이션 등록

첫 번째 방법은 설치한 톰캣 디렉터리의 webapps 폴더에 작성한 웹 애플리케이션을 위치시킨 후 톰캣을 재실행시켜 톰캣이 자동으로 웹 애플리케이션을 인식한 후 실행시키는 방법입니다.
하지만 개발 환경에서는 수시로 애플리케이션을 실행하고 테스트할 필요가 있습니다. 따라서 웹 애플리케이션을 통째로 움직이지 말고 다른 디렉터리에 웹 애플리케이션을 생성 후 그 위치를 server.xml에 등록하여 실행시키는 방법이 두 번째 방법입니다.


# 마무리
지금까지 톰캣에 대해서 알아보았습니다. 웹 프로그래밍을 공부하면서 톰캣을 사용하는데, 톰캣에 대해서 하나도 모르고 있었다는 사실이 부끄럽기도 합니다. 이 글을 작성하기 위해 다양한 아티클들을 읽어보며 톰캣뿐만 아니라 다양한 개념에 대해서 자세히 알아갈 수 있는 즐거운 시간이었습니다.