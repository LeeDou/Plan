# 真实的cookie

### cookie是什么？
定义： 又称为“小甜饼”。类型为“小型文本文件”（维基百科）

我们眼中的cookie，模糊概念——记录用户信息，存在于会话过程中。当然这是在我们刚开始接触所认为的cookie。

ok, 进入正题！

cookie是一个文本文件，存储在我们的电脑上。是以名/值对的形式存储：
`username=kantle`

所以总结来说说就是，cookie以名/值对的形式存储在客户端中。

### cookie的读取
既然存在客户端中，就当然可以用JavaScript这一运行在客户端的脚本语言读取。例：
`var x = document.cookie`  // 将可以取到所有的cookie， 形如： "cookie1=value1;cookie2=value2;cookie3=value3"

### cookie的创建
cookie既然已经可以读取，当然前提是得先创建，同样我们可以在客户端用JavaScript来创建cookie：
`document.cookie="username=kantle";`
我们知道cookie包含Expires/Max-Age、Path、Domain等属性，当然也是可以进行设置的，如
`document.cookie="username=kantle;max-age=3600*30; path=/"`
则可以得到：
![cookie](/img/cookie1.jpg)

### cookie的修改和删除
对cookie进行修改：
`document.cookie="username=lee; expires=Thu, 18 Dec 2019 12:00:00 GMT; path=/"`;

最后删除cookie，不对cookie赋值即可：
`document.cookie="username=; expires=Thu, 18 Dec 2019 12:00:00 GMT;`

### cookie的作用是什么
已经讲完了cookie在客户端的增删改查，那cookie存在的意义是什么，他有什么作用？

作为一个具有存储文件，cookie肯定是能帮助我们存储一些信息，具体哪些信息可能得在实际应用我，我们具体选择，不过cookie作为我们使用的最常见  
的功能就是保存用户信息，实现会话状态的保持。  

我们知道从客户端向服务端请求信息，完成信息传递就会断开连接，完成一次会话，完成请求后，客户端信息，及服务端信息是相互独立的， 
这大概就是我理解的HTTP协议是无状态的。  
但是，我们希望再次请求新的信息时，服务端对客户端用户可以进行验证，识别特定的用户，这就提到了session（真是千呼万唤始出来） 

由于HTTP协议是无状态协议，但是有时候服务端需要记录用户状态，识别用户，这是session就登场了。服务端要为特定的用户创建特定的session，标
识这个用户，这个session有一个唯一的标识（通常我们称之为sessionID，画重点），session是保存在服务端的，保存session的方式有很多，内存、 
数据库，文件都有，大型的网站还会有专门的session集群。  

讲完session的基本概念，现在进入正题，cookie为什么会这么火？  
因为cookie广泛用于实现session跟踪，第一次创建session（也就是用户登录时），服务端会在HTTP协议中告诉客户端，需要在客户端记录一个  
sessionID（其实是在HTTP返回头通过setCookie这个属性在客户端写入一个sessionID）。以后每次客户端发送请求都会把这个id高数服务端（通过  
HTTP请求头发送给服务端），这样服务端就知道客户端是谁。   

sessionID在cookie的记录就形如"sid=12345678", 在服务器中以一个形如{sid: 1223456}这样的对象进行保存，这样就保证了客户端和服务同时标识
唯一的用户，实现用户状态的保持。

### 举例（koa中来实践cookie）
例如通过koa来实现登录接口
上代码：
```js
const signin = function(ctx) {
  const sid = ctx.cookies.get('sid');
  ctx.cookies.set('sid', sid, {
    // domain: 'localhost',  // 写cookie所在的域名
    // path: '/index',       // 写cookie所在的路径
    // maxAge: 10 * 60 * 1000, // cookie有效时长
    expires: new Date('2021-02-15'),  // cookie失效时间
    httpOnly: false,  // 是否只用于http请求中获取
    overwrite: false  // 是否允许重写
  });
  ctx.response.body = '<h2>hello kanele! </h2>';
}
```
在客户端cookie中即可看到一个value值为kantle的cookie，同时expires被设置为2015-02-15
![cookie](/img/cookie2.jpg)

然后让我们来看一下请求： 
![cookie](/img/cookie3.jpg)
我么可以看到请求返回头的Set-cookie（`Set-Cookie:sid=kantle; path=/; expires=Mon, 15 Feb 2021 00:00:00 GMT`）属性，同样在请求
头中写入了Cookie(`Cookie:sid=kantle`),在以后的每一次请求中客户端都将会把sid这个值在HTTP请求头中传递给服务端。

ok, 这就是客户端cookie在session机制中的作用了。

### More

session机制还可以通过url 在url为不拼接sid=xxx(url重写技术)，以及通过form表单来实现。  

Session是在服务端保存的一个数据结构，用来跟踪用户的状态，这个数据可以保存在集群、数据库、文件中；

Cookie是客户端保存用户信息的一种机制，用来记录用户的一些信息，也是实现Session的一种方式。（郑重申明cookie不是一个值，也不是一个唯一标识！！！这句话是对产品讲的hhh）