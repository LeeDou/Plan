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
newapm
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
│  templates.json
└─ README.md
```

### 初始化项目
新建项目后执行
`npm init`
然后继续执行
```js
npm i chalk co co-prompt commander --save
```
执行后package.json 如下：
```js
"dependencies": {
  "chalk": "^2.4.1",
  "co": "^4.5.0",
  "co-prompt": "^1.0.0",
  "commander": "^2.9.0"
}
```

在根目录下新建\bin文件夹，在里面建一个无后缀名的apm文件，这个bin\apm就是整个脚手架入口。
初始化代码
```js
#!/usr/bin/env node

process.env.NODE_PATH = __dirname + '../node_modules/'

const program = require('commander')

// 定义当前脚本
program
  .version(require('../package').version)

// 定义使用方法
program
  .usage('<command>')

```
这样就完成了apm脚手架的一个基本结构接下来就一一实现其他四个功能
### 具体实现
- init 
apm文件修改如吓
```js
program
  .command('init')
  .description('Generate a new project')
  .alias('i')
  .action(() => {
      require('../command/init')()
  })
```
在根目录下新建command文件夹，文件夹下新建init.js 
```js
'use strict'
const exec = require('child_process').exec
const co = require('co')
const prompt = require('co-prompt')
const config = require('../templates')
const chalk = require('chalk')

module.exports = () => {
 	co(function *() {
  	let tplName = yield prompt('Template name: ')
  	let projectName = yield prompt('Project name: ')
  	let gitUrl
  	let branch

	if (!config.tpl[tplName]) {
    	console.log(chalk.red('\n × Template does not exit!'))
    	process.exit()
    }
		gitUrl = config.tpl[tplName].url
		branch = config.tpl[tplName].branch

    // 将模板克隆到指定的项目下面
    let cmdStr = `git clone -b ${branch} ${gitUrl} ${projectName}`

	  console.log(chalk.white('\n Start generating...'))

	  exec(cmdStr, (error, stdout, stderr) => {
      if (error) {
        console.log(error)
        process.exit()
      }
      console.log(chalk.green('\n √ Generation completed!'))
      console.log(`\n cd ${projectName} && npm install \n`)
      process.exit()
	  })
  })
}
```
- add 添加模板
apm 文件修改如下
```js
program
	.command('add')
	.description('Add a new template')
    .alias('a')
    .action(() => {
        require('../command/add')()
    })
```
command文件夹下新建add.js 文件
```js
'use strict'
const co = require('co')
const prompt = require('co-prompt')
const config = require('../templates')
const chalk = require('chalk')
const fs = require('fs')

module.exports = () => {
 co(function *() {

   // 分步接收用户输入的参数
   let tplName = yield prompt('Template name: ')
   let gitUrl = yield prompt('Git https link: ')
   let branch = yield prompt('Branch: ')
    
   // 避免重复添加
   if (!config.tpl[tplName]) {
     config.tpl[tplName] = {}
     config.tpl[tplName]['url'] = gitUrl.replace(/[\u0000-\u0019]/g, '') // 过滤unicode字符
     config.tpl[tplName]['branch'] = branch
   } else {
     console.log(chalk.red('Template has already existed!'))
     process.exit()
   }
   
   // 把模板信息写入templates.json
   fs.writeFile(__dirname + '/../templates.json', JSON.stringify(config), 'utf-8', (err) => {
     if (err) console.log(err)
     console.log(chalk.green('New template added!\n'))
     console.log(chalk.grey('The last template list is: \n'))
     console.log(config)
     console.log('\n')
     process.exit()
    })
 })
}
```

同理我们可以添加list 和 delete功能
最终apm 文件如下：
```js
program
	.usage('<command>')

program
	.command('add')
	.description('Add a new template')
  .alias('a')
  .action(() => {
    require('../command/add')()
  })

program
	.command('list')
	.description('List all the templates')
	.alias('l')
	.action(() => {
		require('../command/list')()
	})

program
	.command('init')
	.description('Generate a new project')
  .alias('i')
  .action(() => {
    require('../command/init')()
  })

program
	.command('delete')
	.description('Delete a template')
	.alias('d')
	.action(() => {
		require('../command/delete')()
	})
```

list.js 入下
```js
'use strict'
const config = require('../templates')

module.exports = () => {
     console.log(config.tpl)
     process.exit()
}
```
delete.js 如下
```js
'use strict'
const co = require('co')
const prompt = require('co-prompt')
const config = require('../templates')
const chalk = require('chalk')
const fs = require('fs')

module.exports = () => {
 	co(function *() {
  	let tplName = yield prompt('Template name: ')

  	if (config.tpl[tplName]) {
      config.tpl[tplName] = undefined
    } else {
      console.log(chalk.red('Template does not exist!'))
      process.exit()
    }

		fs.writeFile(__dirname + '/../templates.json', JSON.stringify(config), 'utf-8', (err) => {
			if (err) console.log(err)
			console.log(chalk.green('Template deleted!'))
      console.log(chalk.grey('The last template list is: \n'))
      console.log(config)
      console.log('\n')
			process.exit()
		})
  })
}
```

### 结语
这样就完成了一个简单的脚手架功能，当然，一个成熟的脚手架会包含更多的功能，未来也将持续的学习，不断来更新。   

当然本文中没有提及构建一个前端项目部分，如果想了解vue项目的构建的话可以看
