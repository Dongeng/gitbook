# linux

> 类UNIX操作系统
> 
> 开放源代码

# 内核版本
> linux核心版本

# 发行版本
> CentOs、ubuntu

# 应用领域
> 服务器
> 
> 嵌入式

# 与windows相比
> 严格区分大小写
> 
> linux中所有内容一文件形式保存，包括硬件
> 
> linux不靠扩展名区分文件类型
> 
> windows下的程序不能直接在linux中安装运行


# 字符界面
> 优势：
> 
> 1.占用系统资源少
> 
> 2.减少出错，被攻击的可能性
> 


# 磁盘分区
> 使用分区编辑器在磁盘上划分几个逻辑区域

# 分区规则
> 主分区：最多4个
> 
> 扩展分区：
> 
> 1.最多1个
> 
> 2.主分区+扩展分区最多4个
> 
> 3.不能写入数据，只能包含逻辑分区

> 逻辑分区


# 文件
> 文件权限：
> 
> -rw-r--r--
> 
> 第一位表示文件类型： -（文件） d（目录） l（软链接文件）
> 
> 第二到第四位表示所有者，第五到第七位表示所属组，第八到第十位表示其他用户
> 
> r:读  w:写 x:执行

# 建立目录
> `mkdir [参数] 路径文件名`

# 文件搜索
> 文件搜索命令locate  `locate 文件名` 在后台数据库中按文件名搜索，速度快  `updatedb`更新数据库
> 
> 命令搜索命令whereis和which `whereis 命令名` 搜索系统命令所在位置
> 
> find `find [搜素范围] [搜索条件]` `find /home -name ldc` 此命令应当避免大范围搜索，否则会非常消耗系统资源
> 
> 字符串搜索命令 grep `grep [参数] 字符串 文件名`

# 环境变量
> 查看环境变量 `echo $PATH`
> 
> 执行命令时，会找$PATH 找到去执行，找不到抛出"command not found"


# 切换用户
> 命令： `su username`

# sudo
> 管理员授权执行命令

# yum
> 

# 下载gcc
> 1.切换root
> 
> 2.在联网情况下， 输入`yum install gcc`
> 
> 3.查看是否安装成功：`gcc -v` 

# vi/Vim
> Vim = vi + IMproved
> 
> 安装 ： `yum -y install vim*`
> 
> 多级撤销
> 
> 语法高亮和自动补全
> 
> 支持多种插件
> 
> 通过网络协议（HTTP/SSH）编辑文件
> 
> 多文件编辑
> 
> Vim可以编辑压缩格式文件（zip,gzip等）

# Vimrc
> rc = run command
> 
> 使用
> 
> vim ~/.vimrc  e ~/.vimrc

# 安装mysql
> 1.linux默认有mariadb,先执行 `yum install mysql`
> 
> 2.删掉mysql `yum remove mysql` 检查 `rpm -qa | grep mysql` 还有就删掉 
> 
> 3.下载mysql包`wget http://repo.mysql.com/mysql57-community-release-el7-8.noarch.rpm`
> 
> 4.更新安装`rpm -Uvh mysql57-community-release-el7-8.noarch.rpm`
> 
> 5.安装mysql-server `yum install mysql-server`
> 
> 6.第一次安装，改mysql密码 重启`service mysqld restart`
> 
> 7.登录`mysql -u root`  改密码 `update user set password=password("新密码") where user="root";`
> 
> 8.改完密码后 `service mysqld restart` 
> 
> 9.登录`mysql -u root -p` 输入新密码