# npm环境变量配置

背景： 随着前端npm的应用，我们会在全局安装各种插件，由于全局安装就会占用C盘空间，及缓存。通过配置npm 安装全局模块的路径，就可以解决以上问题。

## 环境变量中系统变量和用户变量的区别

环境变量分为系统环境变量和用户环境变量。环境变量是指系统环境变量，对所有用户起作用，而用户环境变量只对当前用户起作用。     
因为我们登录时会有不同用户区分，所以Windows就有了用户变量和系统变量区分。系统变量是万能的，每个用户都可以使用期配置，而用户变量只有在其配置     
的账户登录后使用。一般来说，我们电脑只有一个主用户，所以在其中任何一个配置即可，但当你用户变量和系统变量中同时配置时，若用户变量更新了配置，   
系统变量也要做相应的改变，故一般情况下都建议配置系统环境变量。     


另外还可以通过原型和实例来理解系统变量和用户变量，实例可以解释为： 假如你和你同学会到一间教室上课，你会将空调温度调到20度，而你同学会调节     
到16度，将你和你同学温度配置成不同的一套数据，当你们上课时温度调节到相应的设置。但系统变量就则不会变化，一直都是设置的初始值，不会按不同      
的用户登录而发生变化。    

## 步骤

### 新建[node_global] 和[node_cache]文件夹
找到node安装路径，例如我的安装路径为：
` D:\Program\node`
在此此文件夹下新建 node_global 和node_cache 文件夹

### 修改默认的全局目录
1. 打开cmd窗口，输入
```js
npm config set prefix "D:\Program\node\node_global"
npm config set cache "D:\Program\node\node_cache"
```
2. 直接修改C:/Users/[username]/.npmrc 文件的cache值和prefix 值
```js
prefix=D:\Program\node\node_global
cache=D:\Program\node\node_cache
```

### 配置置环境变量
》 “此电脑”-->“右键”-->“属性”-->“高级系统设置”-->“高级”-->“环境变量” 如下图
![环境变量](/img/nodepath.jpg)
1. 方法一：
进入环境变量对话框，在用户变量下修改[PATH] 添加
D:\Program\node\node_global\;
添加[NODE_PATH]  
D:\Program\node\node_global\node_modules

2. 方法二：
进入环境变量对话框，新建系统变量[NODE_PATH] 值为 [D:\Program\node\node_global\node_modules] 
修改系统变量[PATH]增加[D:\Program\node\node_global]
即：
path=D:\Program\node\node_global
NODE_PATH=D:\Program\node\node_global\node_modules

// C:\User\[用户名]\AppData\Roaming\npm --> [D:\Program\node\node_global]  这条指令改变的是执行路径

### 最后检测npm环境变量配置是否成功
1. 检测npm全局路径是否配置成功
`npm install webpack -g`
可以看到：
![安装路径](/img/npmpath.jpg)
已经将webpack安装在了D:\Program\node\node_global目录下

2. 检测node全局路径是否配置正确，正确的话cmd会输出相关信息
执行
`Ctrl+r`
输入cmd，进入node环境
![rquire](/img/npmwebpack.jpg)







### [Linux node/npm 环境变量配置 ](https://blog.csdn.net/jianleking/article/details/79130667)