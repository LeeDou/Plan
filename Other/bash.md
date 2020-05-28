查看当前路径 pwd
nginx -t 查看执行的 nginx 的配置文件路径
which nginx 查看 nginx 安装路径

ps -ef | grep nginx 查看 nginx 进程

### mac

nginx 配置路径 /usr/local/etc/nginx/
nginx 安装路径 /usr/local/Cellar/nginx

command+option+c 复制文件路径

## ssh 连接报错
ssh localhost
//ssh: connect to host localhost port 22: Connection refused
sudo systemsetup -f -setremotelogin on
 
ssh localhost 

## 断开 ssh 连接
法1：Ctrl+D

法2：输入 logout

1.启动sshd服务：
sudo launchctl load -w /System/Library/LaunchDaemons/ssh.plist

2.停止sshd服务：
sudo launchctl unload -w /System/Library/LaunchDaemons/ssh.plist

3查看是否启动：
sudo launchctl list | grep ssh

如果看到下面的输出表示成功启动了：
－－－－－－－－－－－－－－
- 0 com.openssh.sshd
