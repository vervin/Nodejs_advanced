> Note:

> 当然在项目中如果有使用express框架，用express.static一行代码就可以达到目的了：

> app.use(express.static('public'))
> 这里我们要实现的正是express.static背后所做工作的一部分，建议同步阅读该模块源码。


不急着写下第一行代码，而是先梳理一下就基本功能而言有哪些步骤。

1.在本地根据指定端口启动一个http server，等待着来自客户端的请求
2.当请求抵达时，根据请求的url，以设置的静态文件目录为base，映射得到文件位置
3.检查文件是否存在
4.如果文件不存在，返回404状态码，发送not found页面到客户端
5.如果文件存在：
打开文件待读取
设置response header
发送文件到客户端
6.等待来自客户端的下一个请求

//http://www.cnblogs.com/SheilaSun/p/7271883.html
//https://www.cnblogs.com/SheilaSun/p/7294706.html

https://github.com/sqzhuyi/node-eagle

> createReadStream 与 readFile

要增添对目录访问的支持，我们重新整理下响应的步骤：

1.请求抵达时，首先判断url是否有尾部斜杠
2.如果有尾部斜杠，认为用户请求的是目录
如果目录存在
  如果目录下存在默认页（如index.html),发送默认页
  如果不存在默认页，发送目录下内容列表
如果目录不存在，返回404
2.如果没有尾部斜杠，认为用户请求的是文件
如果文件存在，发送文件
如果文件不存在，判断同名的目录是否存在
  如果存在该目录，返回301，并在原url上添加上/作为要转到的location
  如果不存在该目录，返回404


  https://github.com/sheila1227/nodejs-static-webserver