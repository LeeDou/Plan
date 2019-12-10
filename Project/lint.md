# GIT 仓库提交代码检查

提交代码前进行代码格式检查，可以在提交代码前检查代码格式，以及编码错误，提升代码质量。

要实现代码提交前的检查，可以考虑到git 提供的钩子函数 pre-commit，即在git commit 执行前的钩子函数，在pre-commit 中对代码进行检查并修改。   
goole 一下发现，原来还存在 husky 这个插件可以帮助实现代码检查及修改。

### pre-commit 
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

### husky
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

### lint-staged


eslint+husky+prettier+lint-staged




eslint+husky+prettier+lint-sta