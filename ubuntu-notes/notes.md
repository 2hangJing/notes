<!--
 * @Author: monai
 * @Date: 2020-02-27 14:42:53
 * @LastEditors: monai
 * @LastEditTime: 2023-06-17 19:37:27
 -->
# ubuntu 笔记
在学 docker 中 linux 部分指令以及部分软件安装、文件拷贝等操作记录

## 指令 ##

**1. apt-get**  
软件安装指令 <https://b9532026.wordpress.com/category/linux/>  

**2. ps -ef|grep xxx**  
查询一个软件的进程

**3. chmod 777 xx.sh**  
给 xx.sh 读、写、运行三项权限。

**4. ps -e | grep node**  
显示所有 node 运行的进程。

**5. npm run server 1>success.log 2>error.log &**  
让指令 `npm run server` 后台执行，并将正确输出到当前目录下 success.log 文件中，错误输出到当前目录下 error.log 文件中。

**6. sudo -i**  
切换到 root 账户。

**7. touch xx.xx**  
创建名字为 xx.xx 的文件。

**8.1 tail -n 50 ./xx.log**
静态（当前文件状态）查看 xx.log 文件后 50行。

**8.2 tail -f ./xx.log**
动态（当前文件改变查看中也会动态改变）查看 xx.log。

**9 service --status-all**
当前所有服务运行状态。（可用次方法查看服务名）
    1. [ + ] 代表启动运行状态
    2. [ - ] 代表关闭停止状态


## 软件 ##
#### **1. nginx**  

下载：`apt-get -y install nginx`  
配置文件目录：`/etc/nginx/nginx.conf`  
启动：`service nginx start`  
关闭：`service nginx stop`  
重启：`service nginx restart`  
重启：`service nginx reload` *不重启重新载入最新配置文件内容*

#### **2. nvm**

下载：`curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.2/install.sh | bash`  
下载完成后配置环境变量  
```
export NVM_DIR="$HOME/.nvm" \  
&& [ -s "$NVM_DIR/nvm.sh" ] \  
&& \. "$NVM_DIR/nvm.sh" \  
&& [ -s "$NVM_DIR/bash_completion" ] \  
&& \. "$NVM_DIR/bash_completion"  
```
node 下载指令：
`nvm install vx.x.x` 示例：`nvm install v10.16.0`

#### **3. curl**  
下载：`apt-get -y install curl`

#### **4. node**

下一代 Debian 正式发行版的代号为 "bullseye" — 发布时间尚未确定  
1. Debian 10（"buster"） — 当前的稳定版（stable）  
2. Debian 9（"stretch"） — 旧的稳定版（oldstable）  
3. Debian 8（"jessie"） — 更旧的稳定版（oldoldstable）  
4. Debian 7（"wheezy"） — 被淘汰的稳定版  
5. Debian 6.0（"squeeze"） — 被淘汰的稳定版  
6. Debian GNU/Linux 5.0（"lenny"） — 被淘汰的稳定版  
7. Debian GNU/Linux 4.0（"etch"） — 被淘汰的稳定版  
8. Debian GNU/Linux 3.1（"sarge"） — 被淘汰的稳定版  
9. Debian GNU/Linux 3.0（"woody"） — 被淘汰的稳定版  
10. Debian GNU/Linux 2.2（"potato"） — 被淘汰的稳定版  
11. Debian GNU/Linux 2.1（"slink"） — 被淘汰的稳定版  
12. Debian GNU/Linux 2.0（"hamm"） — 被淘汰的稳定版  

#### **5. mysql**

**下载：** `apt-get -y install mysql-server mysql-client`

启动mysql：  
方式一：`/etc/init.d/mysql start`   
方式二：`service mysql start`

停止mysql：  
方式一：`/etc/init.d/mysql stop`  
方式二：`service mysql stop`  

重启mysql：  
方式一：`/etc/init.d/mysql restart`  
方式二：`service mysql restart`  

**部分指令：**  
1. `mysql -u root -p;` 进入mysql，root 账户
2. `show tables;` 查看表
3. `use databaseName;` 进入名为 databaseName 的数据库
4. `CREATE DATABASE 数据库名;` 创建数据库
5. `source /code/xxx.sql;` 导入/code/xxx.sql 文件
6. `show databases;` 查看数据库
7. `use mysql; select user,host,authentication_string from user;` 查看 mysql 所有用户表

**删除 ubuntu mysql**
1. `apt-get remove mysql-common`
2. `apt-get autoremove --purge mysql-server-5.0`
3. `dpkg -l |grep ^rc|awk '{print $2}' | xargs dpkg -P`  

**查看默认账号密码：`cat /etc/mysql/debian.cnf`**

**ubuntu 中修改 mysqld.cnf 让外网可以连接数据库**  
1. 先查看 3306 数据库默认端口是否打开 `netstat -anp`。
2. 修改 mysqld.cnf。 指令：**`vim /etc/mysql/mysql.conf.d/mysqld.cnf`**，找到 `bind-address= 127.0.0.1` 将 `127.0.0.1` 修改为 `0.0.0.0`。修改后重启 mysql。


##### **博客环境切换注意：**  
**mysql:**
1. `cat /etc/mysql/debian.cnf` 查看默认账号密码。
2. `mysql -u root -p;` 进入mysql，root 账户或者默认 user 账号（第一步中查看的账号，版本不同可能不一样，默认：`debian-sys-maint`）。
3. `use mysql;`
4. `set password for "root"@localhost=password("123456");` 设置 root 密码。
5. `ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '123456';` 第二步切换链接 mysql 验证方式
6. `update user set host = '%' where user = 'root';` 第三步将 root IP访问限制关闭，% 为全部IP均可以访问。
7. `CREATE DATABASE xxxx;` 第四步创建数据库。
8. `source /code/xxx.sql;` 第五步导入/code/xxx.sql 文件。

**File Zilla root用户连接配置 :**
1. `sudo passwd root` 先设置 root 密码。
2. `sudo -i` 切换到 root 用户。
3. `sudo vim /etc/ssh/sshd_config` 进入ssh 配置，准备修改配置文件。
4. `PermitRootLogin yes` 找到 PermitRootLogin，摁 i 进入编辑模式，将后面值修改为 yes，`esc` `:wq` 保存退出。
5. `sudo service ssh restart` 重启 ssh 服务，重启后用 File Zilla root 账户链接。

