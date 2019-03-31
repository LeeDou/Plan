# 如何新建一个npm包

前言：作为前端同学，我们经常会用`npm i` 这条命令，我们清楚的知道执行这条指令我们会将指定的包从npm上拉去下来，那么怎样来写一个npm包，并将其上传到npm中，在自己需要的时候直接使用呢？

### 第一步
去npm的官方网站注册一个账号。

### 第二步
新建一个文件，这个文件将包含你将上传的公共方法，使用npm init 初始化package.json文件。必须要有这个文件才能上传，npm可以上传任何一个包含package.json的包，package.json的main(入口)设置为index.js（名字自取）然后在本目录下建一个index.js文件，name表示你这个包的名字只能小写,version代表版本，每次更新都要修改这里的版本再npm publish。

### 第三步
在本地使用命令行连接npm  使用下面的命令，然后按照提示走
`npm login 或者npm adduser`

### 第四步
上传npm包
执行`npm publish`就能将我们刚才新建的项目作为一个npm包上传到npm官网。

例如我的pachage.json 文件为：
```js
{
  "name": "cofn",
  "version": "1.0.0",
  "description": "common function",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "leedou",
  "license": "ISC"
}
```
包名为cofn, 此时可以在npm官网中搜索cofn,将会有如下结果：
![cofn](/img/npmcofn.jpg)
若包名在npm官网中已经存在，将会上传失败显示如下错误
![errpr1](/img/npmpk.jpg)
错误展示为我不允许上传此包，并且询问我是否为此包的作者登陆npm，显然不是，所以上传失败。

### 第五步
更新npm包
- 撤销上传
如果想删除已经上传了的包，可以直接在当前文件下执行以下命令：
`npm unpublish --force'
若删除已经上传的包，就需要在24小时后才能再次上传，当然如果修改为其它名称作为另一个包上传不会受到任何影响。

- 更新包
每次更新npm包需要对包的版本号增大，可以手动更改package.json 文件，也可以通过npm命令进行修改。  
版本号由三位组成  a.b.c   
更新c处：npm version patch  
更新b处: npm version minor  
跟新a处: npm version major   
然后执行`npm publish`就可将更新后的包上传，若版本号没更新，将会报错。   

### 注意
若想自己上传的npm包能像平时我们使用其他包`import fs from 'fs'`一样使用，当然还有一些必要的规范，首先我们的入口文件在package.json必须指定，其次必须使用exports暴露你的方法，例如
```js
exports.cofn = () => {
    console.log('hello npm package');
}
```