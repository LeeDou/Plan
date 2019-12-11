# GIT 仓库提交代码检查

提交代码前进行代码格式检查，可以在提交代码前检查代码格式，以及编码错误，提升代码质量。

要实现代码提交前的检查，可以考虑到git 提供的钩子函数 pre-commit，即在git commit 执行前的钩子函数，在pre-commit 中对代码进行检查并修改。   
goole 一下发现，原来还存在 husky 这个插件可以帮助实现代码检查及修改。

### [pre-commit](https://pre-commit.com/) 
pre-commit能够防止不规范代码被commit
首先安装 pre-commit
```
 npm install pre-commit --save-dev
```
编辑 package.json 文件
```
"scripts": {
    "test": "echo \"Error: I SHOULD FAIL LOLOLOLOLOL \" && exit 1",
    "lint": "lint-staged",
  },
"pre-commit": [
  "lint",
]
```

通过执行 check 脚本，来实现对代码的检查修改。

### [husky](https://github.com/typicode/husky)
husky 也可以用来防止代码不规范提交的插件，但是相比 pre-commit 提供了更加强大的功能，husky 可以执行 pre-commit pre-push 等生命周期。husky 姑且可以认为是 git 钩子函数的一个代理集合。
安装 husky
> 在安装 husky 的时候，husky会根据 package.json里的配置，在.git/hooks 目录生成所有的 hook 脚本（如果你已经自定义了一个hook脚本，husky不会覆盖它）  

```
npm install husky --save-dev
```
编辑 package.json 
```
"scripts": {
    "test": "echo \"Error: I SHOULD FAIL LOLOLOLOLOL \" && exit 1",
  },
"husky": {
  // 在 hooks 中可以声明其他 git 钩子函数的操作， 
   "hooks": {
      "pre-commit": "lint-staged",
      "commit-msg": "echo $HUSKY_GIT_PARAMS"
    }
}
```

通过以上，无论是pre-commit 还是 husky ，我们都已经声明了在 pre-commit 这个钩子，但是在钩子中具体操作是 `lint-staged` ，那 lint-staged 又是什么？

### [lint-staged](https://github.com/okonet/lint-staged)
staged 是 git 的暂存区概念，lint-staged 的目的在于，将代码从暂存区(stage)提交到分支时执行。
安装 lint-staged 
```
npm install lint-staged --save-dev
```

修改 package.json
```
"src/**": [                     // 指定文件，而不是全局扫描
  "eslint --fix --ext .js",     // 执行eslint --fix操作，进行扫描，若发现工具可修复的问题进行fix
  "prettier --write",           // 执行prettier脚本，这是对代码进行格式化的，在下面具体来说
  "git add"                     // 上述两项任务完成后对代码重新add
]
```

可以看到，先执行 eslint 对代码扫描并修改，然后执行 prettier 脚本来对代码格式化。

### [eslint](https://eslint.org/docs/user-guide/getting-started)
安装 eslint 
```
npm install eslint --save-dev
```
eslint 初始化项目

```
npx eslint --init 会在文件中生成 .eslintrc.js 文件
```
在此文件 rule 中配置相关的检查规则
```
"rules":{
  // 禁止对象字面量中出现重复的 key
  "no-dupe-keys": "error",
  // 禁止出现重复的 case 标签
  "no-duplicate-case": "error",
  // 禁止出现空语句块,允许catch出现空语句
  "no-empty": ["error", {"allowEmptyCatch": true}],
  // 禁止对 catch 子句的参数重新赋值
  "no-ex-assign":"error",
  // 禁止不必要的布尔转换
  "no-extra-boolean-cast": "error",
  // 禁止不必要的分号
  "no-extra-semi": "error",
  // 强制所有控制语句使用一致的括号风格
  "curly": "error"
}
```

### [prettier](https://prettier.io/docs/en/install.html)
prettier 是一个格式化代码工具
安装 prettier 
```
npm install prettier --save-dev
```
在根目录下新建 .prettierrc 文件
```
{
  // 代码换行长度
  "printWidth": 200,
  // 代码缩进空格数
  "tabWidth": 2,
  // 使用制表符缩进而不是空格缩进
  "useTabs": true,
  // 代码结尾是否加分号
  "semi": false,
  // 是否使用单引号
  "singleQuote": true,
  // 对象大括号内两边是否加空格 { a:0 }
  "bracketSpacing": true,
  // 单个参数的箭头函数不加括号 x => x
  "arrowParens": "avoid"
}
```
> 注意在使用时需要删除注释

### 总结
到此我们就构建了一个具备语法检查，并可以自动格式化代码的项目。

```
// 依次执行，就可以看到相应的检查代码，并格式化代码效果
git add .
git commit -m "test"

```