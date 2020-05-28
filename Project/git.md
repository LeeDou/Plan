git 常用命令

删除分支
`git branch -d <BranchName>             // 删除本地分支`
`git push origin --delete <BranchName>  // 删除远程分支`
`git fetch -p                           // 清理本地无效分支(远程已删除本地没删除的分支)`
`git remote -v                          // 查看远程仓库地址`
git branch -m old_branch new_branch     // 修改本地分支名称
git push origin :old_branch             // 删除远程分支


ignore 规则不生效
git rm -r --cached .
git add .
git commit -m 'update .gitignore'