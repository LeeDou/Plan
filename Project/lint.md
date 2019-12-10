# GIT 仓库提交代码检查

提交代码前进行代码格式检查，可以在提交代码前检查代码格式，以及编码错误，提升代码质量。

要实现代码提交前的检查，可以考虑到git 提供的钩子函数 pre-commit，即在git commit 执行前的钩子函数，在pre-commit 中对代码进行检查并修改。  
npm 提供了强大的命令行，同样我们可以使用 npm 命令来实现代码仓库的检查及修改。  
goole 一下发现，原来还存在 husky 这个插件可以帮助实现代码检查及修改。

### pre-commit 





eslint+husky+prettier+lint-sta