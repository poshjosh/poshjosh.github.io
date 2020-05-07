---
path: "./2019/11/12/Add-chat-functionality-to-a-maven-java-web-app.md"
date: "2019-11-12"
title: "How to Add Chat Functionality to a Maven Java Web App"
description: "Easy to understand tutorial: How to add chat functionality to a maven java web app"
lang: "en-us"
---

Today we learn how to add chat functionality to a java web application. We do
this by using a java library based on CometD chat libraries. This is a 2 part tutorial.

_*This  is Part 1 of a 2-part blog post*_

*Requirements.*
To be able to follow this tutorial, you should understand:
- The basics of building a java web application with Servlets.
- How to use [Java Server Pages (JSP)](https://docs.oracle.com/javaee/5/tutorial/doc/bnagy/">Java Server Pages (JSP))
- How to use [Maven](https://maven.apache.org/)
- How to use [Github](https://github.com)

### First we ask: Why add chat functionality? ###

Each visitor to a website could be engaged at the critical window when they
are about to make a decision. It could also be at the time algorithm decides
they are confused about something (e.g visiting the same link, back and forth 3 times).
At that point a chat window could appear with a customer care representative at the
other end to engage the potential custeomer. With that in mind, the following are
basically the advantages of chat:
- Reduce expenses.
- Increase sales.
- Improve customer service and loyalty.
- Discover customer pain points.
- Faster problem resolution.
- Customer convenience.
- Competitive advantages.
- Expand market reach.
### So what is CometD? ###

--------------------------------------------------------------------------------
![CometD logo](https://cometd.org/wp-content/uploads/2015/12/cometd-logo-fire-100x100.png")|
The CometD libraries allow your application to send messages in three forms:
Push-Subscribe, Peer-to-Peer and Remote-Call.
--------------------------------------------------------------------------------

- With Publish-Subscribe, a message sent by a publisher is broadcast to many subscribers.
This is the typical case for chat rooms, for example.
- With Peer-to-Peer, a message is sent by a sender to a specific receiver. This is the
typical case for private one-to-one chats, for example.
- With Remote-Call, a “message” is sent by a client to the server to perform a specific
action. This is the typical case for creating a new chat room, for example.

Learn more about the CometD messaging features in the
[CometD reference manual.](https://docs.cometd.org/current/reference/)

### Now how do we add CometD chat functionality to a java web application? ###

To add CometD chat functionality to a java web application, we will be using
the [CometD chat](https://github.com/poshjosh/cometdchat) library I wrote.
Clone the library from github. Then follow the next steps.

####Step 1####
Open your favorite java ide and create a maven web java application.

####Step 2####
Add the CometdD chat library you cloned from github as a dependency to your
maven java web application.

####Step 3####
Add the xml below to your web.xml

```xml
  <!-- Servlet to query cometd chat messages -->
  <servlet>
      <servlet-name>messages</servlet-name>
      <servlet-class>com.looseboxes.cometd.chat.servlets.Messages</servlet-class>
      <async-supported>true</async-supported>
  </servlet>
  <servlet-mapping>
      <servlet-name>messages</servlet-name>
      <url-pattern>/chat/messages</url-pattern>
  </servlet-mapping>
  <!-- CometdChat WebApp Config -->
  <context-param>
      <param-name>cometdChatAppName</param-name>
      <param-value>[PUT A NAME HERE]</param-value>
  </context-param>
  <filter>
      <filter-name>continuation</filter-name>
      <filter-class>org.eclipse.jetty.continuation.ContinuationFilter</filter-class>
      <async-supported>true</async-supported>
  </filter>
  <filter-mapping>
      <filter-name>continuation</filter-name>
      <url-pattern>/cometd/*</url-pattern>
  </filter-mapping>
  <!-- Cometd Servlet -->
  <servlet>
      <servlet-name>cometd</servlet-name>
      <servlet-class>com.looseboxes.cometd.chat.servlets.CometdWithMessageConsumer</servlet-class>
      <async-supported>true</async-supported>
      <init-param>
          <param-name>transports</param-name>
          <param-value>org.cometd.websocket.server.WebSocketTransport</param-value>
      </init-param>
      <init-param>
          <param-name>services</param-name>
          <param-value>com.looseboxes.cometd.chat.ChatService</param-value>
      </init-param>
      <load-on-startup>1</load-on-startup>
  </servlet>
  <servlet-mapping>
      <servlet-name>cometd</servlet-name>
      <url-pattern>/cometd/*</url-pattern>
  </servlet-mapping>
```

####Step4####
Add the code below to any ServletContextListener.

```java
public void contextInitialized(ServletContextEvent servletContextEvent) {
    final ServletContext servletContext = servletContextEvent.getServletContext();
    final CometdChat cometdChat = new CometdChat(servletContext);
}
```

####Step 5####
Add the code below to the head section of any <tt>jsp</tt> or <tt>jspf</tt> etc, page you want
chat to be available from.

```xml
<%@taglib uri="/META-INF/tlds/cometdchat" prefix="cometdchat"%>
<cometdchat:joinChat loginUserDisplayName="me" windowBackground="navy"/>
```
####Hurray! We made it####

Now when you open the page where you added the above taglib, a small chat window should open.

*_In Part 2 we will delve a bit deeper into whats happening behind the scenes_*

*References:*

- [CometD Messaging.](https://cometd.org/messaging/)
- [Top 10 Live Chat Benefits.](https://www.comm100.com/blog/live-chat-benefits/)
