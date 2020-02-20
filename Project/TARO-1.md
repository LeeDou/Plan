# Taro 原理解析1

1.Taro 使用，到各个指令 cli  init 过程

2.详细介绍执行 build 后编译文件的过程

### 先说说 Taro 是什么
Taro 是一套遵循 React 语法规范的 多端开发 解决方案。

现如今市面上端的形态多种多样，Web、React-Native、微信小程序等各种端大行其道，当业务要求同时在不同的端都要求有所表现的时候，针对不同的端去编写多套代码的成本显然非常高，Taro 实现的就是只编写一套代码就能够适配到多端的功能。

使用 Taro，我们可以只书写一套代码，再通过 Taro 的编译工具，将源代码分别编译出可以在不同端（微信/百度/支付宝/字节跳动/QQ小程序、快应用、H5、React-Native 等）运行的代码。 

另外市场还存在的编译成其他代码的框架有 [mpvue](http://mpvue.com/)、[weui](https://weui.io/)、[wepy](https://tencent.github.io/wepy)、[chameleon](https://cmljs.org/doc/framework/framework.html) 等。

Tara 遵循 React 语法，集成的是 [Nerv](https://nervjs.github.io/docs/) 框架

Nerv是一款基于virtual dom技术的类React UI框架，它基于React标准，拥有和React一致的API与生命周期。得益于与React保持一致API的特性，Nerv可以无缝兼容到React的相关生态，例如[react-router](https://github.com/ReactTraining/react-router)，[react-redux](https://github.com/reactjs/react-redux)，以及大部门使用React开发的组件，所以对于旧的React项目，可以无痛地将React替换成Nerv。

### Taro 编译
在 Taro 中先按照 Nerv 语法编写代码，然后通过编译工具将对应 Nerv 语法的代码编译成对应小程序、RN 代码
![taro](/img/taro1.png)
输入一份源代码，针对不同的端设定好对应的转换规则，再一键转换出对应端的代码。

### Taro 文件结构
```
|-- tarojs
    |-- cli                      // taro 脚手架
        |-- bin                  // 指令
        |-- dist                 
        |-- scripts
        |-- src
        |-- templates
        |-- package.json         // 配置文件
        |-- index.js             // 入口文件
    |-- taroize                  // Map 资源
        |-- lib
        |-- index.js
    |-- transformer-wx           // 语法转换
        |-- lib
        |-- index.js
```

### Taro-cli 指令
init: Init a project with default templete，依据模版初始化项目
config: Taro config，获取 taro 版本信息
create: Create page for project，新建页面
build: Build a project，编译项目
update: Update packages of taro，更新依赖包
convert: Convert weapp to taro，将 weapp 转为 taro
info: Diagnostics Taro env info，环境信息
doctor: Diagnose taro project，检查项目信息

### 从 init 指令分析 taro 实现
  执行 taro init 初始化项目  
  init 指令文件为 /bin/taro-init  
  
```
  // taro-init  
  const Project = require('../dist/create/project').default


  const project = new Project({
    projectName,
    projectDir: process.cwd(),
    templateSource,
    clone,
    template,
    description,
    typescript,
    css
  })

  project.create()
```
  创建 project 实例  
  调用 /dist/create/project 中的 Project 类  
  Project 继承自 /dist/create/creator 的 Creator 类  
  执行 Creator 构造函数获取项目当前路径  
  执行 create 函数  

```
  // /dist/create/project.js  
  create() {
    // 先下载模板到 user 文件目录下
    this.fetchTemplates()
      // 下载好的模板列表
      .then((templateChoices) => this.ask(templateChoices))
      // 通过 answer 创建项目
      .then(answers => {
      const date = new Date();
      this.conf = Object.assign(this.conf, answers);
      this.conf.date = `${date.getFullYear()}-${(date.getMonth() + 1)}-${date.getDate()}`;
      // write 写项目
      this.write();
    })
      .catch(err => console.log(chalk_1.default.red('创建项目失败: ', err)));
  }

  // 交互函数，来获取项目名、项目描述、语法、样式、模板
  ask(templateChoices) {
    const prompts = [];
    const conf = this.conf;
    this.askProjectName(conf, prompts);
    this.askDescription(conf, prompts);
    this.askTypescript(conf, prompts);
    this.askCSS(conf, prompts);
    this.askTemplate(conf, prompts, templateChoices);
    return inquirer.prompt(prompts);
  }

  write(cb) {
    // SOURCE_DIR: 'src'
    this.conf.src = config_1.default.SOURCE_DIR;
    // 根据克隆的模板文件，复制到当前文件夹的创建项目目录下
    init_1.createApp(this, this.conf, cb)
      .catch(err => console.log(err));
  }  
```
taro init 指令会从 git 仓库中克隆下来已经创建好的模板项目，然后根据项目创建过程中的项目配置来将指定的模板复制到创建项目的文件夹下。

### 总结
Taro 能够将一套复合 Nerv(React) 规范的代码通过编译，生成能够在不同类型的小程序项目，本文主要讲述了 taro 创建一个新项目的源码实现，后续将持续更新 taro 编译相关原码实现。



  

    