---
title: 【Java基础】Java网络编程
type: categories
copyright: false
date: 2019-04-12 22:13:28
tags: Java基础
categories: Java
title_en: java_net_program
---


>【学习参考资料】：[菜鸟教程-Java教程](http://www.runoob.com/java/java-tutorial.html)

## 1，Java网络编程
1）**概述**

`网络编程`：编写运行在多个设备（计算机）的程序，这些设备都通过网络连接起来。
`java.net`包中`J2EE`的`API`包含有类和接口，他们提供低层次的通信细节。主要有：
 > - `TCP`：传输控制协议，保障了两个应用程序之间的可靠通信，通常用于互联网协议，称为`TCP/IP`；
> - `UDP`：用户数据报协议，一个无连接的协议，提供了应用程序之间要发送的数据的数据包。
      
2）Socket编程

`套接字`使用`TCP`提供了两台计算机之间的通信机制。 客户端程序创建一个`套接字`，并尝试连接服务器的`套接字`。

当连接建立时，服务器会创建一个 `Socket` 对象。客户端和服务器现在可以通过对 `Socket` 对象的写入和读取来进行通信。
 
 `java.net.Socket` 类代表一个套接字，并且 `java.net.ServerSocket` 类为服务器程序提供了一种来监听客户端，并与他们建立连接的机制。
 
以下步骤在两台计算机之间使用`套接字`建立`TCP`连接时会出现：
> - 服务器实例化一个 `ServerSocket` 对象，表示通过服务器上的端口通信。
> - 服务器调用 `ServerSocket` 类的 `accept()` 方法，该方法将一直等待，直到客户端连接到服务器上给定的端口。
> - 服务器正在等待时，一个客户端实例化一个 `Socket` 对象，指定服务器名称和端口号来请求连接。
> - `Socket` 类的构造函数试图将客户端连接到指定的服务器和端口号。如果通信被建立，则在客户端创建一个 `Socket` 对象能够与服务器进行通信。
> - 在服务器端，`accept()` 方法返回服务器上一个新的 `socket` 引用，该 `socket` 连接到客户端的 `socket`。

3）**`ServerSocket`类的方法**：服务器应用程序通过使用 `java.net.ServerSocket` 类以获取一个端口,并且侦听客户端请求。

4）**`Socket`类的方法**：`java.net.Socket` 类代表客户端和服务器都用来互相沟通的套接字。客户端要获取一个 `Socket` 对象通过实例化 ，而 服务器获得一个 `Socket` 对象则通过 `accept()` 方法的返回值。

5）**`InetAddress`类的方法**：表示互联网协议（`IP`）地址。

6）**`demo`实例**

- *GreetingClient.java*
```java
import java.io.*;
import java.net.Socket;

/**
 * Socket编程--客户端实例
 * @author zhangbc
 * @date 2019/3/7 14:26
 */
public class GreetingClient {
    public static void main(String[] args) {
        String serverName = args[0];
        int port = Integer.parseInt(args[1]);
        try {
            System.out.println("连接到主机：" + serverName + ", 端口号：" + port);
            Socket client = new Socket(serverName, port);
            System.out.println("远程主机地址：" + client.getRemoteSocketAddress());
            OutputStream outServer = client.getOutputStream();
            DataOutputStream outData = new DataOutputStream(outServer);

            outData.writeUTF("Hello from " + client.getLocalSocketAddress());
            InputStream inFromServer = client.getInputStream();
            DataInputStream inData = new DataInputStream(inFromServer);
            System.out.println("服务器响应：" + inData.readUTF());
            client.close();
        } catch (IOException ex) {
            ex.printStackTrace();
        }
    }
}
```

- *GreetingServer.java*
```java
import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.IOException;
import java.net.ServerSocket;
import java.net.Socket;
import java.net.SocketTimeoutException;
import java.lang.Thread;

/**
 * @author zhangbc
 * @date 2019/3/7 15:09
 */
public class GreetingServer extends Thread {
    private ServerSocket serverSocket;

    public static void main(String[] args) {
        int port = Integer.parseInt(args[0]);
        try {
            Thread thread = new GreetingServer(port);
            thread.run();
        } catch (IOException ex) {
            ex.printStackTrace();
        }
    }

    public GreetingServer(int port) throws IOException {
        serverSocket = new ServerSocket(port);
        serverSocket.setSoTimeout(10000);
    }

    public void run() {
        while (true) {
            try {
                System.out.println("等待远程连接，端口号为："
                        + serverSocket.getLocalPort() + "...");
                Socket server = serverSocket.accept();
                DataInputStream inData = new DataInputStream(server.getInputStream());
                System.out.println(inData.readUTF());

                DataOutputStream outData = new DataOutputStream(server.getOutputStream());
                outData.writeUTF("谢谢连接我：" + server.getLocalSocketAddress()
                        + "\nGoodbye!");
                server.close();
            } catch (SocketTimeoutException es) {
                System.out.println("Socket timed out!");
                break;
            } catch (IOException ex) {
                ex.printStackTrace();
                break;
            }
        }
    }
}
```

## 2，Java发送邮件
- *SendEmail.java*
```java
package com.runoob;

import javax.activation.DataHandler;
import javax.activation.DataSource;
import javax.activation.FileDataSource;
import javax.mail.*;
import javax.mail.internet.*;
import javax.mail.Message.RecipientType;
import java.util.Properties;

/**
 * 发邮件(纯文本，HTML文本，附件)
 * @author zhangbocheng
 * @version v1.0
 * @date 2019/3/7 20:40
 */
public class SendEmail {
    public static void main(String[] args) throws NullPointerException {
        // 收件人邮箱
        String to = "xxxxxxxxxxxxxxxxx@qq.com";
        // 发件人邮箱
        final String from = "xxxxxxxxxxxxxxxxx@163.com";
        final String pwd = "xxxxxxxx";
        // 指定发邮件的主机
        String host = "smtp.163.com";
        // 获取系统属性
        Properties properties = System.getProperties();

        // 设置邮件服务器
        properties.setProperty("mail.host", host);
        properties.put("mail.smtp.auth", "true");

        // 获取默认Session对象
        Session session = Session.getDefaultInstance(properties, new Authenticator() {
            @Override
            protected PasswordAuthentication getPasswordAuthentication() {
                return new PasswordAuthentication(from, pwd);
            }
        });

        try {
            // 创建默认的MimeMessage对象
            MimeMessage message = new MimeMessage(session);
            // Set From：头部头字段
            message.setFrom(new InternetAddress(from));
            // Set To：头部头字段
            message.addRecipient(RecipientType.TO,
                    new InternetAddress(to));
            // Set Subject：头部头字段
            message.setSubject("This is the Subject Line!");
            // 设置消息体
            message.setText("This is test text.");
            // 发送HTML消息，可以插入html标签
            message.setContent("<h1>This is actual message</h1>",
                    "text/html;charset=utf-8");
            // 创建消息部分
            BodyPart messageBodyPart = new MimeBodyPart();
            // 消息
            messageBodyPart.setText("This is message body.");
            // 创建多重消息
            Multipart multipart = new MimeMultipart();
            // 设置文本消息
            multipart.addBodyPart(messageBodyPart);

            // 附件部分
            messageBodyPart = new MimeBodyPart();
            String fileName = "/home/projects/java_pro/java_instances_demo/src/main/java/com/runoob/SendEmail.java";
            DataSource source = new FileDataSource(fileName);
            messageBodyPart.setDataHandler(new DataHandler(source));
            messageBodyPart.setFileName(fileName);
            multipart.addBodyPart(messageBodyPart);

            // 发送完整部分
            message.setContent(multipart);
            // 发送消息
            Transport.send(message);
            System.out.println("Sent message successfully.");
        } catch (MessagingException mex) {
            mex.printStackTrace();
        }
    }
}
```

## 3，Java Applet基础

1）**`Applet`基础**

`Applet`是一种`Java`程序，一般运行在支持`Java`的`Web`浏览器内，是一个全功能的Java应用程序。
> - `Java` 中 `Applet` 类继承了 `java.applet.Applet` 类。
> - `Applet` 类没有定义 `main()`，所以一个 `Applet` 程序不会调用 `main()` 方法。
> - `Applet` 被设计为嵌入在一个 `HTML` 页面。
> - 当用户浏览包含 `Applet` 的 `HTML` 页面，`Applet` 的代码就被下载到用户的机器上。
> - 要查看一个 `Applet` 需要 `JVM`。 `JVM` 可以是 `Web` 浏览器的一个插件，或一个独立的运行时环境。
> - 用户机器上的 `JVM` 创建一个 `Applet` 类的实例，并调用 `Applet` 生命周期过程中的各种方法。
> - `Applet` 有 `Web` 浏览器强制执行的严格的安全规则，`Applet` 的安全机制被称为沙箱安全。
> - `Applet` 需要的其他类可以用 `Java` 归档（`JAR`）文件的形式下载下来。
    
2）**`Applet`的生命周期**

`Applet` 类中的四个方法给我们提供了一个框架：
> - `init`:  提供所需的任何初始化。在 `Applet` 标记内的 `param` 标签被处理后调用该方法。
> - `start`: 浏览器调用 `init` 方法后，该方法被自动调用。每当用户从其他页面返回到包含 `Applet` 的页面时，则调用该方法。
> - `stop`: 当用户从包含 `Applet` 的页面移除的时候，该方法自动被调用。
> - `destroy`: 此方法仅当浏览器正常关闭时调用。
> - `paint`: 该方法在 `start()` 方法之后立即被调用，或者在 `Applet` 需要重绘在浏览器的时候调用。`paint()` 方法实际上继承于 `java.awt`。

3）**`Applet`类**

每一个 `Applet` 都是 `java.applet.Applet` 类的子类，基础的 `Applet` 类提供了供衍生类调用的方法,以此来得到浏览器上下文的信息和服务。

4）**`Applet`的调用**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Hello World, Applet</title>
</head>
<body>
<hr>
<applet code="HelloWorldApplet.class" width="320" height="120">
</applet>
<hr>
</body>
</html>
```
