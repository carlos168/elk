一.下载及安装

首先去官网下载mac对应版本的Mysql,尾缀为.dmg的程序包

下载地址：https://dev.mysql.com/downloads/mysql/

下载完毕后，一步步傻瓜式安装即可，安装完后mysql会弹出一个框框，告诉你安装成功以及root用户的初始密码，注意Mysql for Mac 的初始密码是随机生成的，最好记住

 

二.更改初始密码 

A.打开终端

1.进入mysql的bin目录下（默认安装的目录为 /usr/local/mysql/bin）

cd /usr/local/mysql/bin/ 

2.输入指令，会提示输入密码（输入mac账户的root密码）

sudo ./mysqld_safe --skip-grant-tables

3.输入密码后，会发现有一些提示，且Mysql会自动重启

B.另开一个终端

1、输入指令1：（进入到mysql的bin目录下） 
输入：cd /usr/local/mysql/bin/ 
2、输入指令2： 
输入：./mysql 
3、进入到mysql命令状态下：（mysql>是mysql命令状态提示） 
输入：mysql> FLUSH PRIVILEGES; 
4、设置密码root 
输入：mysql> SET PASSWORD FOR 'root'@'localhost' = PASSWORD('root');

至此，初始密码设置完毕

 

三.配置环境变量

若不设置环境变量，输入命令太繁琐，每次登陆Mysql的命令为：/usr/local/mysql/bin/ mysql -u root -p

windows系统直接在环境变量中添加mysql的路径就好，mac系统则需要在home目录下的.bash_profile文件中新建path路径指向mysql的路径

查看home目录：

输入命令 echo $HOME

mac默认的home目录为当前用户根目录 /Users/silverlaw

配置环境变量：

1.打开终端,输入：

open .bash_profile 

2.直接输入如下语句：

export PATH=${PATH}:/usr/local/mysql/bin

保存，关闭终端和TextEdit

重新打开终端，输入：mysql -u root -p，即可成功登陆
