# 环境变量配置

背景： 随着前端npm的应用，我们会在全局安装各种插件，由于全局安装就会占用C盘空间，及缓存。通过配置npm 安装全局模块的路径，就可以解决以上问题。

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
D:\node\node\node_global\;
这种方法不需要添加系统环境变量NODE_PATH,只需要编辑用户变量

2. 方法二：

















### [Linux node/npm 环境变量配置 ](https://blog.csdn.net/jianleking/article/details/79130667)