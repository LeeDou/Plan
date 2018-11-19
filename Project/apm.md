# Node开发前端脚手架

## 说在前面
从学习Vue使用vue-cli到实习公司使用orich等前端脚手架，在这过程中，深感脚手架的神奇，可以根据js命令生成自己所需的项目文件，自此就一直想自己开发一个前端脚手架......

## 准备
- npm 
- node
- es6
- [commander](https://github.com/tj/commander.js/)
- [co](https://github.com/tj/co)
- [co-prompt](https://github.com/tj/co-prompt)


vue-cli 的实现，是通过预先下载指定的模板，然后初始化项目时将模板copy到指定路径，而主流的脚手架实现方式另外一种就是通过从远程仓库拉取指定的模板。而本文将介绍的也是通过第二种，也就是通过拉取指定的模板初始化项目的方式。

首先来了解一下Commanderjs
## commander

顾名思义，我们可以认为他是一种指令语言。实际上它为我们写node提供了一种指令方式。

```
var program = require('commander')

program
    .version('0.0.1')
    .description('a test cli program')
    .option('-n, --name <name>', 'your name', 'GK')
    .option('-a, --age <age>', 'your age', '22')
    .option('-e, --enjoy [enjoy]')

program.parse(process.argv)
```
### 常用api
commandr.js 中命令行有两种可变性，一个叫做option，意为选项。一个叫做command，意为命令。

#### option  
用法： `.option('-n, --name <name>', 'your name', 'ok')`
- 第一各参数是选项定义，分为短定义和长定义。用|， , ， 连接。
  - 参数可以用<>或者[]修饰，前者为必须参数，后者为可选参数
- 第二个参数为选项描述
- 第三个参数为选项参数默认值

#### command
用法： `.comand('init <path>', 'description')`
- 可以接受三个参数，第一个为命令定义，第二个为命令描述，第三个位命令辅助修饰对象
- 第一个可以使用 <> 或者 []修饰命令参数
- 第二个可选。
  - 当没有第二个参数是，commander.js将返回command对象，若有第二个参数，将返回原型对象
  - 当带有第二个参数，并且没有显示调用action(fn)时，则将使用子命令模式
  - 所谓子命令模式即，./pm, ./pm-install, ./pm-search等，这些字命令跟主命令在不同的文件中。
  - 低三个参数一般不用，他可以设置是否显示的使用子命令模式


## co-prompt
co-prompt 实现命令行交互   
co-prompt 模块依赖于 co模块，所以使用co-prompt 需要安装co模块（co 是一个异步程序处理模块）   

### API

__普通文本提示__
- prompt(msg)
  - msg `<string>`提示信息具体内容
**密码文本提示，密码非明文显示**
- prompt.passward(msg, [mask])
  - msg `<string>` 提示信息的具体内容
  - mask `<string>` 输入密码时的显示替换字符，默认是"*"

**多行文本提示**
- prompt.multiline(msg)
  - msg `<string>` 提示用户信息的具体内容

**确认提示信息**
- prompt.confirm(msg)
  - msg `<string>` 提示用户信息的具体内容
  - 返回值 `<bool>` true|false
  > confirm() 方法只在用户输入[y|yes|ok|true] 这四个值时（不区分大小写），才返回true ，其它情况都是 false     

### 综合例子
```js
var co = require('co')
var prompt = require('prompt')

co(function* () {
    var username = yield prompt('username: ');
    var pwd = yield prompt.password('password: ');
    var desc = yield prompt.multiline('description: ');
    var ok = yield prompt.confirm('are you sure?(yes|no)');
    console.log('hello %s %s\n', username, pwd);
    console.log('you descrption as:\n' + desc);
    console.log('answer: %s\n', ok);
    process.exit();
})
```

## 现在开始从头构建一个脚手架程序

### 整体结构
- 初始化项目 init 从已添加的模板中选择，然后从远程仓库拉取
- 添加模板 add 将模板选项添加到template.json中
- 删除模板 delete 从template.json 删除模板选线
- 模板列表 list 模板选项列表
文件夹结构
```
├─bin
│      apm
│      commander.js
│      express
│      test
│      
├─command
│      add.js
│      delete.js
│      init.js
│      list.js
│      
├─node_modules
│  .gitignore
│  main.js
│  package-lock.json
│  package.json
│  README.md
│  templates.json  
```


https://segmentfault.com/a/1190000006190814