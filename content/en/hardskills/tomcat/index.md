---
title: 🐱 How Tomcat is structured and works
summary: Learn what Apache Tomcat is, how it works, and how it's structured.
date: 2024-02-28
author:
  - jaeheon
# Featured image
# Place an image named `featured.jpg/png` in this page's folder and customize its options here.
image:
  caption: 'Image credit: [**Unsplash**](https://unsplash.com)'
---


I've been using Tomcat while studying web programming, but I realized that I don't know anything about Tomcat. Even in the book, it was explained how to run Tomcat and make it work, but it didn't tell me anything about Tomcat in detail. And I was curious about whether Tomcat is a web application server or a servlet container. So, I would like to learn more about Apache Tomcat, its structure, and how it works.


# What is Tomcat
Apache TomcatThere are many different names for Tomcat. Some call it a web application server, others call it a servlet container. To understand this, we need to understand web servers and web application servers (WAS).

First, a web server is a server that receives HTTP requests from web browser clients and serves static content. In the real world, however, it's extremely rare that we want to serve only static content, so we use a Web Application Server (WAS) to serve dynamic content.

When a client sends an HTTP request, the web server receives it and sends it back to the WAS, which processes the business logic and sends it back to the web server, which then serves the data to the client.

Let's take a closer look at a WAS. A WAS is a server that implements the J2EE specification and is middleware used in a distributed environment to handle functions such as distributed transactions, security, and thread handling. In other words, it is a web server + web container that puts components on top of the web. Strictly speaking, Tomcat is not a web application server (WAS) because it does not utilize EJB technology as per the J2EE specification. Apache Tomcat is more like a servlet container in that it provides servlet management for handling business logic.

It's worth noting that a web application server can handle more technologies than a web container because it supports the entirety of Jave EE.
In other words, Apache Tomcat is basically a servlet container, but with its own web server inside. Because of this, it seems reasonable to think of Tomcat as a servlet container with some WAS functionality.


# Structure of Tomcat

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
# Check the tags on the server
Here is the configuration file for the server, server.xml. 
Let's take a look at it to understand the terminology, and then we'll look at the structure of Tomcat.
<br>

Context: A context refers to a single web application within Tomcat.
<br>

Connector: Connectors are responsible for communicating with clients, which means they accept and process HTTP requests. Tomcat's default connector handles requests from clients according to the HTTP/1.1 standard. Additionally, the connector listens for requests on a specific port (8080 by default). The redirectPort property redirects requests to a secure port (8443 by default) if an SSL (HTTPS) connection is required.
<br>

Host: A host refers to the connection of a network name (www.myname.com) to the Tomcat server. A host can contain an arbitrary number of contexts (applications). Tomcat's default configuration includes a host named localhost
<br>

Engine: The engine is the heart of Tomcat. Because a service can have multiple connectors, the engine receives all requests from these connectors, processes them, and forwards the responses back to the appropriate connector to send to the client. The engine must contain one or more hosts, one of which must be designated as the default host. The default Tomcat configuration includes a Catalina engine with a localhost host
<br>

Listener: A listener is a Java object that can respond to specific events by implementing the org.apache.catalina.LifecycleListener interface.
<br>

Realm: Realm components can appear inside any container component (engine, host, context). It represents a database of users, passwords, and user roles. Its purpose is to support container-based authentication.
<br>

Valve: A valve is an interceptor-like element that, when inserted into a container (context, host, or engine), intercepts all incoming HTTP requests before they reach your application. This allows you to pre-process requests destined for a specific application, an application running on a virtual host, or all applications running within an engine.
<br>


# Tomcat directory structure
bin: Contains script files for running and shutting down Tomcat.
conf: Contains configuration files for server-wide settings, such as server.xml. Of these, server.xml contains access/connections related to server settings. You can change the ports as needed to avoid conflicts with the set ports. In addition, if you want to change it to a path other than the webapps folder, you can enter the path to your project in the < context docBase=“”/> part. web.xml is Tomcat's configuration file and allows you to set servlets, filters, encodings, and is the first file read when the server goes up.

lib: This is where the binary modules that connect Tomcat to other web servers are located, as well as the jar libraries needed to run Tomcat.

logs: Contains Tomcat log files, including exception throws.

temp: This is where temporary files are stored while Tomcat is running.

webapps: This is where your web applications are located.

work: This is where the java and class files that convert JSP files to servlet state are stored.
<br><br>
## Registering a web application in a container
There are two ways to register a web application in a container.
- Save the application in the %CATALINA_HOME%webApps directory
- Registering the web application directly in the server.xml file

The first method is to place the web application you created in the webapps folder of your installed Tomcat directory and then rerun Tomcat so that Tomcat automatically recognizes the web application and runs it.
However, in a development environment, you need to run and test the application from time to time. Therefore, the second method is to create the web application in another directory instead of moving the entire web application, and then register the location in server.xml to run it.
<br>

# Wrapping up
In this article, we've learned about Tomcat. I use Tomcat while studying web programming, and I'm ashamed to admit that I didn't know anything about Tomcat. I had a great time reading various articles to learn more about Tomcat and its various concepts.
