---
title: websocket初学(2019-8-11)
---

##  一、WebSocket是什么

WebSocket是HTML5新增的协议，用于在浏览器和服务器之间建立双向通信的通道，并且不会受限，服务器可以在任何时候向浏览器发送消息

传统的HTTP协议是没有办法实现WebSocket的功能的，因为HTTP协议是一个请求响应协议。浏览器向服务器发送请求，服务器响应请求，再发送数据给浏览器。

所以基于传统的HTTP，无法在浏览器中实现在线聊天，在线炒股，在线多人游戏。但是有两种解决方案：轮询和Comet。轮询需要浏览器固定时间间隔向服务器发送请求，这样做的实时性不够并且频繁的请求会给服务器带来极大压力。Comet是一种长轮询机制，浏览器向服务器发送请求，如果服务器没有新的数据发送，就不用立即回复，直到新的数据到来或者请求超时再回复，这样暂时解决了实时性的问题，但是以多线程模式运行的服务器会让大部分线程的大部分时间都处于挂起状态，极大的浪费了服务器的资源。除此一个HTTP连接在长时间没有数据传输的情况下，链路上的任何一个网关都可能关闭这个连接，网关是不可控的，所以Comet连接需要定期发一些ping数据表示“正常工作”

以上两种机制都不能从根本解决浏览器能够实时接收数据的问题，所以出现了WebSocket标准，让浏览器和服务器之间可以建立无限制的全双工通信，任何一方都可以主动发消息给对方

#### WebSocket协议

**1. WebSocket是如何实现全双工的？** 实际上HTTP是建立在TCP上的，TCP本身就实现了全双工，只是HTTP的请求-应答机制限制了全双工通信。WebSocket连接建立之后只是简单规定，双方通信不用HTTP协议，直接发送数据就好。

**2. 如何创建WebSocket？** 首先连接需要由浏览器发起，因为请求协议是一个标准的HTTP协议。格式如下：
```
GET ws://localhost:3000/ws/chat HTTP/1.1
Host: localhost
Upgrade: websocket //这个连接将要被转换为WebSocket连接
Connection: Upgrade //这个连接将要被转换为WebSocket连接
Origin: http://localhost:3000
Sec-WebSocket-Key: client-random-string //标识这个连接，并非用于加密数据
Sec-WebSocket-Version: 13 //指定了WebSocket的协议版本
```
如果服务器接收请求，就会返回如下响应：
```
HTTP/1.1 101 Switching Protocols 
//101表示本次连接的HTTP协议即将被更改，更改后的协议就是Upgrade: websocket指定的WebSocket协议
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: server-random-string
```
WebSocket连接建立成功之后，浏览器和服务器就可以主动发送消息给对方。消息有两种，一种是文本，另一种是二进制数据。通常是JSON格式的文本。

**3. 如何实现安全的WebSocket连接？**
和HTTPS类似，首先浏览器用`wss://xxx`创建WebSocket连接时，会先通过HTTPS创建安全的连接，然后该HTTPS连接升级为安全的WebSocket连接，底层通信仍然走的是安全的SSL/TLS协议。

#### 服务器
由于WebSocket是一个协议，服务器具体怎么实现，取决于所用编程语言和框架本身。Node.js本身支持的协议包括TCP协议和HTTP协议，要支持WebSocket协议，需要对Node.js提供的HTTPServer做额外的开发。已经有若干基于Node.js的稳定可靠的WebSocket实现，我们直接用npm安装使用即可

## 二、使用ws
参考项目[基于WebSocket实现的简易在线聊天室](https://github.com/QM36/simple-chat-room)