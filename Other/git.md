### git pull/push 需要输入名称和密码

因为clone的时候用的https 不是用ssh 所以导致push时需要输密码
解决办法：
git bash进入你的项目目录，输入：

git config --global credential.helper store

通过记住账号和密码