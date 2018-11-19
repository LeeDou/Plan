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




https://segmentfault.com/a/1190000006190814