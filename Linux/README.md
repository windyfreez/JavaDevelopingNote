# 📚 Linux 运维部署指南
## 前言

### 学习说明
Linux 是后端开发绕不开的必修课：本地用 VMWare 装虚拟机学习，生产环境全是 Linux 服务器。面试和工作中最常见的场景就是——**在干净的 CentOS7 服务器上从零搭建 JDK、MySQL、nginx、Redis、RabbitMQ 环境，并把 Spring Boot 项目部署上线**。本笔记按照"虚拟机 → 系统安装 → 常用命令 → 环境安装 → 项目部署"的顺序整理，命令全部可直接复制执行。

### 实用要点
- 学习环境：VMWare Workstation + CentOS 7（与生产环境最接近的经典组合）；
- 部署思维：**能装起来是第一步，能开机自启、能排错、能扛住重启才是生产级**；
- 面试角度：Linux 高频考点集中在**常用命令、权限管理、进程/端口排查、部署流程**，重点在于熟练度和排错思路，不要求背冷门参数。

---

## 一、Linux 系统基础与常用命令

### 1. 目录结构速览
| 目录 | 作用 |
|---|---|
| / | 根目录，一切文件的起点 |
| /bin、/sbin | 系统命令（ls、cp、systemctl 等） |
| /etc | 系统/软件配置文件（如 /etc/profile、/etc/my.cnf） |
| /home | 普通用户的家目录（/home/itheima） |
| /root | root 用户的家目录 |
| /usr/local | 软件安装目录（源码编译默认装这里） |
| /var | 变化的数据：日志（/var/log）、数据库文件（/var/lib/mysql） |
| /tmp | 临时目录 |
| /opt | 第三方软件目录 |
| /proc | 虚拟文件系统，内存中的进程信息 |

### 2. 文件与目录操作
| 命令 | 作用 | 常用写法 |
|---|---|---|
| ls | 列出文件 | `ls -l` 详情、`ls -a` 含隐藏文件、`ll` 等价 `ls -l` |
| cd / pwd | 切换目录 / 查看当前目录 | `cd /usr/local`、`cd ~` 回用户家目录 |
| mkdir | 创建目录 | `mkdir -p /a/b/c` 递归创建 |
| touch | 创建空文件 / 更新时间戳 | `touch app.log` |
| cp | 复制 | `cp -r 源 目标` 复制目录 |
| mv | 移动/重命名 | `mv old.txt new.txt` |
| rm | 删除 | `rm -rf /path` 递归强制删除，**慎用！** |
| ln | 软链接 | `ln -s /usr/local/redis/bin/redis-cli /usr/bin/redis-cli` |
| find | 查找文件 | `find / -name "*.log"` |
| which / whereis | 定位命令位置 | `which java` |

> ⚠️ `rm -rf` 是生产环境最危险的命令，删除前务必确认路径，建议先 `ls` 看一眼再删。

### 3. 文件查看与内容搜索
| 命令 | 作用 | 常用写法 |
|---|---|---|
| cat | 查看全部内容 | `cat /etc/os-release` |
| head / tail | 查看头部/尾部 | `tail -f app.log` 实时跟踪日志（部署排错最常用） |
| less / more | 分页查看 | `less big.log`，q 退出 |
| grep | 按内容搜索 | `grep -n "error" app.log`、`grep -rn "keyword" /usr/local/` |
| wc | 统计行数/字数 | `wc -l app.log` |
| sort / uniq | 排序 / 去重 | `sort x.txt | uniq -c` 统计重复次数 |
| cut | 按列截取 | `cut -d':' -f1 /etc/passwd` 取出用户名 |

### 4. 权限与用户管理
```bash
# 查看权限：-rwxr-xr-x  =  类型 + 属主(u) 属组(g) 其他(o)
ls -l

# 修改权限：r=4 w=2 x=1，755 = 属主rwx、属组rx、其他rx
chmod 755 script.sh
chmod +x script.sh         # 给所有用户加执行权限

# 修改属主/属组
chown -R mysql:mysql /usr/local/mysql

# 用户管理
useradd itheima            # 创建用户
passwd itheima             # 设置密码
usermod -aG wheel itheima  # 加入sudo组（CentOS7）
groups itheima             # 查看用户所属组
su - itheima               # 切换用户
sudo 命令                   # 以root权限执行
```

### 5. 系统管理（进程、服务、端口、磁盘）
| 命令 | 作用 |
|---|---|
| `top` | 实时查看 CPU/内存占用（按 q 退出） |
| `free -h` | 查看内存使用 |
| `df -h` | 查看磁盘分区使用率 |
| `du -sh /usr/local` | 查看目录占用空间 |
| `ps -ef \| grep java` | 查看 java 相关进程（部署排查最常用） |
| `kill -9 进程号` | 强制杀进程 |
| `systemctl status/start/stop/restart 服务名` | 管理 systemd 服务 |
| `crontab -e` | 编辑定时任务（如每天凌晨清理日志） |
| `uname -a` / `cat /etc/os-release` | 查看内核 / 系统版本 |
| `uptime` / `history` | 运行时长 / 命令历史 |
| `date` | 查看/设置时间 |

### 6. 网络与防火墙
```bash
ip addr                  # 查看IP（CentOS7 网卡一般叫 ens33）
ping -c 3 www.baidu.com  # 测试外网连通性

# 查看端口占用（部署排错必会）
netstat -tlnp            # 查看所有监听端口及对应进程
ss -tlnp                 # 新版替代命令
lsof -i:8080             # 查看8080端口被谁占用（需安装lsof）

# 防火墙（CentOS7 默认 firewalld）
systemctl status firewalld
firewall-cmd --zone=public --add-port=8080/tcp --permanent   # 永久开放端口
firewall-cmd --reload                                        # 重载生效
systemctl stop firewalld && systemctl disable firewalld      # 学习环境可直接关闭

# 文件传输
scp app.jar root@192.168.1.100:/usr/local/app/   # 本地推送到服务器
wget http://xxx/xxx.tar.gz                        # 服务器直接下载
curl -I http://localhost:8080                     # 测试接口连通性
```

### 7. 压缩解压与软件包管理（yum）
```bash
# 压缩解压
tar -zxvf jdk-8u391-linux-x64.tar.gz    # 解压 .tar.gz
tar -zcvf backup.tar.gz /usr/local/app   # 打包压缩
unzip app.zip                            # 解压 zip（需 yum install -y unzip）

# yum 软件管理（基于 rpm，自动解决依赖）
yum install -y 软件名      # 安装
yum remove -y 软件名       # 卸载
yum search 关键字          # 搜索
yum list installed | grep mysql   # 查看已安装
yum repolist               # 查看软件源
```

### 8. vi/vim 编辑器速记
```text
vi 文件名          # 打开（没有则新建）
i                # 进入插入模式开始编辑
Esc              # 退出插入模式
:wq              # 保存并退出
:q!              # 不保存强制退出
/关键字           # 搜索，n 下一个
dd               # 删除当前行
yy + p           # 复制当前行并粘贴
```
> ⚠️ 改配置文件前建议先 `cp 文件 文件.bak` 备份，改坏了能恢复。

---

## 二、VMWare 虚拟机安装

### 1. VMWare Workstation 下载安装
1. 官网下载 VMWare Workstation Pro（个人使用现已免费，搜索"VMware Workstation Pro download"即可）；
2. 双击 exe 一路下一步安装，安装完成后打开主界面。

### 2. 新建虚拟机（以 CentOS7 为例）
1. 点击"**新建虚拟机**" → 选择"**典型（推荐）**"；
2. 选择"**稍后安装操作系统**"（或直接指定 CentOS 镜像 ISO）；
3. 客户机操作系统选 **Linux**，版本选 **CentOS 7 64 位**；
4. 设置虚拟机名称和位置（如 `D:\VM\CentOS7`）；
5. 磁盘容量建议 **40GB**，选择"将虚拟磁盘拆分成多个文件"；
6. 完成后在虚拟机设置中挂载镜像：CD/DVD → 使用 ISO 映像文件 → 选择 `CentOS-7-x86_64-Minimal-2009.iso`；
7. 点击"开启此虚拟机"，进入 CentOS 安装界面（安装步骤见第三章）。

### 3. 虚拟机网络模式三种选择
| 模式 | 原理 | 适用场景 |
|---|---|---|
| **桥接模式** | 虚拟机与宿主机在同一网段，相当于局域网内一台独立主机 | 需要局域网其他机器直接访问虚拟机（部署测试） |
| **NAT 模式（默认）** | 虚拟机通过宿主机共享上网，对外隐藏 | 虚拟机只需要上网，最常用 |
| **仅主机模式** | 只能与宿主机通信，不能上网 | 隔离环境测试 |

> ⚠️ 桥接模式下如果宿主机换网络（如公司换 WiFi），虚拟机 IP 会变，配置了固定 IP 的服务可能连不上。

### 4. 常见问题
- **虚拟机没网**：检查 Windows 服务里 VMware NAT Service / DHCP 服务是否启动；检查虚拟机网卡 `ONBOOT=yes`；
- **复制粘贴/拖拽失效**：`yum install -y open-vm-tools open-vm-tools-desktop` 安装 VMware 增强工具（相当于安装 Tools）；
- **屏幕分辨率小**：桌面版系统安装 `open-vm-tools-desktop`；
- **快照**：安装好系统、配置好环境后记得"**虚拟机 → 快照**"打一个快照，环境搞坏了秒级恢复。

---

## 三、CentOS7 安装与初始化配置

### 1. 镜像下载
- 阿里云镜像：`https://mirrors.aliyun.com/centos/7/isos/x86_64/`
- 推荐 **CentOS-7-x86_64-Minimal-2009.iso**（最小化安装、无图形界面，最接近服务器环境；新手可选 DVD 版带图形界面）。

### 2. 安装过程要点
1. 开机后选择 **Install CentOS 7**，语言选中文或 English；
2. **软件选择**：最小安装（Minimal Install），或带"开发工具"（Development Tools，自带 gcc 等编译工具）；
3. **安装位置**：选"自动配置分区"即可（想深入学习可手动 LVM 分区）；
4. **网络和主机名**：打开"以太网"开关（ON），设置主机名（如 `node01`）；
5. 设置 **root 密码**，创建普通用户；
6. 安装完成重启。

### 3. 安装后初始化配置（新服务器必做）
```bash
# 1. 配置网卡开机自启（最小化安装默认网卡是关闭的！）
vi /etc/sysconfig/network-scripts/ifcfg-ens33
# 把 ONBOOT=no 改为 ONBOOT=yes
# 如果要用静态IP，还需要：BOOTPROTO=static 并追加
#   IPADDR=192.168.1.100
#   NETMASK=255.255.255.0
#   GATEWAY=192.168.1.1
#   DNS1=114.114.114.114
systemctl restart network
ip addr   # 验证IP

# 2. 关闭防火墙（学习环境）或开放端口（生产推荐开放端口）
systemctl stop firewalld
systemctl disable firewalld   # 禁止开机启动

# 3. 关闭 SELinux（不关会导致很多服务启动异常/权限怪异）
setenforce 0                  # 临时关闭
vi /etc/selinux/config        # 永久关闭：SELINUX=disabled（需重启）

# 4. 换阿里云 yum 源（国内下载速度翻倍）
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.bak
wget -O /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo
yum clean all && yum makecache

# 5. 时间同步（服务器时间不准会导致日志错乱、证书校验失败）
yum install -y ntpdate
ntpdate ntp.aliyun.com

# 6. 主机名与常用工具
hostnamectl set-hostname node01
yum install -y vim net-tools lsof unzip wget   # net-tools 提供 ifconfig/netstat
```

---

## 四、Linux 安装 JDK

### 1. 方式一：yum 安装 OpenJDK（最快，适合快速测试）
```bash
yum install -y java-1.8.0-openjdk java-1.8.0-openjdk-devel
java -version   # 验证
```
> 优点：一条命令装完；缺点：版本不可控，生产环境一般用方式二手动安装指定版本 JDK。

### 2. 方式二：手动安装 Oracle JDK（生产推荐）
```bash
# 1. 上传 jdk-8u391-linux-x64.tar.gz 到服务器（scp/rz 均可）
mkdir -p /usr/local/java
tar -zxvf jdk-8u391-linux-x64.tar.gz -C /usr/local/java
ls /usr/local/java   # 得到 /usr/local/java/jdk1.8.0_391

# 2. 配置环境变量
vi /etc/profile
# 文件末尾追加：
export JAVA_HOME=/usr/local/java/jdk1.8.0_391
export PATH=$JAVA_HOME/bin:$PATH
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar

# 3. 使配置生效并验证
source /etc/profile
java -version
javac -version
echo $JAVA_HOME
```
> ⚠️ `vi /etc/profile` 改的是系统级环境变量，所有用户生效；改完必须 `source /etc/profile` 或重新登录。

---

## 五、Linux 安装 MySQL

### 1. 方式一：官方 yum 仓库安装 MySQL 8.0（推荐）
```bash
# 1. 卸载系统自带的 mariadb（与MySQL冲突）
yum remove -y mariadb-libs

# 2. 安装 MySQL 官方 yum 源
wget https://dev.mysql.com/get/mysql80-community-release-el7-3.noarch.rpm
rpm -ivh mysql80-community-release-el7-3.noarch.rpm

# 3. 安装 mysql 服务端
yum install -y mysql-community-server

# 4. 启动并设置开机自启
systemctl start mysqld
systemctl enable mysqld

# 5. 查看初始临时密码（MySQL8 安装后自动生成）
grep 'temporary password' /var/log/mysqld.log

# 6. 登录并修改密码
mysql -uroot -p   # 输入临时密码
ALTER USER 'root'@'localhost' IDENTIFIED BY 'Root@123456';
```
> 想装 5.7：`yum-config-manager --disable mysql80-community` 再 `--enable mysql57-community` 后安装即可（yum-config-manager 由 yum-utils 提供）。

### 2. 配置远程访问（本地 Navicat / IDEA 连接）
```sql
-- 进入mysql命令行后执行
CREATE USER 'root'@'%' IDENTIFIED BY 'Root@123456';
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```
```bash
# 防火墙开放3306端口（如果之前没关防火墙）
firewall-cmd --zone=public --add-port=3306/tcp --permanent
firewall-cmd --reload
```
> ⚠️ MySQL8 默认密码策略要求密码包含大小写字母+数字+特殊字符，简单密码会报 validate_password 错误；开发环境可执行 `SET GLOBAL validate_password.policy=LOW;` 降低策略。

### 3. 方式二：tar.gz 二进制包安装 MySQL 5.7
```bash
# 1. 创建 mysql 用户
useradd mysql

# 2. 解压到 /usr/local/mysql
tar -zxvf mysql-5.7.44-linux-glibc2.12-x86_64.tar.gz -C /usr/local/
mv /usr/local/mysql-5.7.44-linux-glibc2.12-x86_64 /usr/local/mysql
mkdir -p /usr/local/mysql/data

# 3. 属主改为 mysql 用户
chown -R mysql:mysql /usr/local/mysql

# 4. 初始化数据目录（--initialize-insecure 表示 root 初始无密码）
/usr/local/mysql/bin/mysqld --initialize-insecure --user=mysql --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data

# 5. 启动（用自带脚本注册为服务）
cp /usr/local/mysql/support-files/mysql.server /etc/init.d/mysqld
service mysqld start
mysql -uroot   # 无密码直接登录，登录后设置密码

# 6. 建立软链接方便全局使用
ln -s /usr/local/mysql/bin/mysql /usr/bin/mysql
```

### 4. 卸载与常用问题
```bash
# 卸载（yum方式）
yum remove -y mysql-community-server mysql-community-client mysql-community-common mysql-community-libs
rm -rf /var/lib/mysql /etc/my.cnf /var/log/mysqld.log

# 常见问题排查
systemctl status mysqld            # 启动失败先看状态
tail -100 /var/log/mysqld.log      # MySQL 日志
mysql -uroot -p                    # Access denied：密码错或未初始化
```

---

## 六、Linux 安装 nginx

### 1. 安装编译依赖
```bash
yum install -y gcc gcc-c++ pcre pcre-devel zlib zlib-devel openssl openssl-devel
```
> pcre 提供正则（rewrite 重写需要）、zlib 提供压缩、openssl 提供 https。

### 2. 源码编译安装
```bash
wget https://nginx.org/download/nginx-1.24.0.tar.gz
tar -zxvf nginx-1.24.0.tar.gz
cd nginx-1.24.0
./configure --prefix=/usr/local/nginx --with-http_ssl_module   # 指定安装目录，支持https
make && make install                                           # 编译并安装
```

### 3. 启动停止与开机自启
```bash
/usr/local/nginx/sbin/nginx              # 启动
/usr/local/nginx/sbin/nginx -s stop      # 停止
/usr/local/nginx/sbin/nginx -s reload    # 平滑重载配置（改完配置用这个，不用重启）
/usr/local/nginx/sbin/nginx -t           # 检查配置文件语法
ps -ef | grep nginx                      # 验证进程
```
开机自启（systemd）：
```bash
vi /etc/systemd/system/nginx.service
# [Unit]
# Description=nginx
# After=network.target
# [Service]
# Type=forking
# ExecStart=/usr/local/nginx/sbin/nginx
# ExecReload=/usr/local/nginx/sbin/nginx -s reload
# ExecStop=/usr/local/nginx/sbin/nginx -s stop
# PrivateTmp=true
# [Install]
# WantedBy=multi-user.target

systemctl daemon-reload
systemctl enable nginx
```

### 4. 常用配置（/usr/local/nginx/conf/nginx.conf）
**静态资源站点：**
```nginx
server {
    listen       80;
    server_name  localhost;
    location / {
        root   /usr/local/myweb;   # 静态文件目录
        index  index.html;
    }
}
```
**反向代理（前端页面请求 /api 转发到后端 8080）：**
```nginx
server {
    listen       80;
    location /api/ {
        proxy_pass http://127.0.0.1:8080/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```
**负载均衡（多台后端轮询）：**
```nginx
upstream backend {
    server 192.168.1.10:8080;
    server 192.168.1.11:8080;
}
server {
    listen 80;
    location / {
        proxy_pass http://backend;
    }
}
```
> ⚠️ 改完配置先 `nginx -t` 检查语法，再 `nginx -s reload`；80 端口被占用会启动失败，用 `netstat -tlnp | grep 80` 排查。

---

## 七、Linux 安装 Redis

### 1. 源码编译安装
```bash
yum install -y gcc make
wget https://download.redis.io/releases/redis-6.2.14.tar.gz
tar -zxvf redis-6.2.14.tar.gz
cd redis-6.2.14
make && make install PREFIX=/usr/local/redis    # 安装到 /usr/local/redis
cp redis.conf /usr/local/redis/redis.conf
```

### 2. 核心配置（/usr/local/redis/redis.conf）
```bash
vi /usr/local/redis/redis.conf
```
```text
daemonize yes              # 后台运行（默认no，不设置的话一关终端就退出）
bind 0.0.0.0               # 允许远程连接（默认只允许本机127.0.0.1）
requirepass 123456         # 设置密码（生产必设）
```

### 3. 启动停止与开机自启
```bash
/usr/local/redis/bin/redis-server /usr/local/redis/redis.conf   # 启动（指定配置文件）
/usr/local/redis/bin/redis-cli -a 123456 ping                   # 测试，返回 PONG
/usr/local/redis/bin/redis-cli -a 123456 shutdown               # 停止
ln -s /usr/local/redis/bin/redis-cli /usr/bin/redis-cli         # 软链接全局可用
```
开机自启：
```bash
vi /etc/systemd/system/redis.service
# [Unit]
# Description=redis
# After=network.target
# [Service]
# Type=forking
# ExecStart=/usr/local/redis/bin/redis-server /usr/local/redis/redis.conf
# ExecStop=/usr/local/redis/bin/redis-cli -a 123456 shutdown
# [Install]
# WantedBy=multi-user.target

systemctl daemon-reload
systemctl enable redis
```
> ⚠️ 连不上 Redis 三连查：1. 配置 `bind 0.0.0.0` 了吗？2. 防火墙开放 6379 了吗？3. 密码对不对？——Spring Boot 连 Redis 报 Connection refused 基本都是这三个原因。
> 注：Redis 7.x 起推荐用 ACL 方式 `user default on >密码` 替代 requirepass，6.x 写法仍然兼容。

---

## 八、Linux 安装 RabbitMQ

> 本仓库 [MQ/]( ./MQ/README.md ) 笔记里有完整的 RabbitMQ 概念与 Java 操作讲解，这里只讲安装部署。

### 1. 安装 Erlang（RabbitMQ 依赖）
```bash
yum install -y socat    # rabbitmq 依赖
curl -s https://packagecloud.io/install/repositories/rabbitmq/erlang/script.rpm.sh | sudo bash
yum install erlang-23.3.4.11-1.el7.x86_64
erl -v                   # 验证（输入 halt(). 退出）
```

### 2. 安装 RabbitMQ
```bash
curl -s https://packagecloud.io/install/repositories/rabbitmq/rabbitmq-server/script.rpm.sh | sudo bash
yum install rabbitmq-server-3.8.16-1.el7.noarch
```

### 3. 启动、Web 管理界面、用户配置
```bash
# 启动（报 epmd error 时在 /etc/rabbitmq/rabbitmq-env.conf 加 NODENAME=rabbit@localhost）
rabbitmq-server start    # 或 systemctl start rabbitmq-server

# 开启 Web 管理界面插件（端口 15672）
rabbitmq-plugins enable rabbitmq_management

# 创建管理员用户并授权
rabbitmqctl add_user admin admin
rabbitmqctl set_user_tags admin administrator
rabbitmqctl list_users

# 开机自启
chkconfig rabbitmq-server on   # 或 systemctl enable rabbitmq-server

# 验证端口（5672=AMQP消息端口、15672=Web管理、25672=集群通信）
netstat -ntlp
```
> 访问管理界面：`http://服务器IP:15672`，用 admin/admin 登录。

---

## 九、Linux 部署 Spring Boot 项目

### 1. 项目打包
```bash
# 本地执行（跳过测试）
mvn clean package -DskipTests
# 打包产物：target/项目名-版本号.jar
```

### 2. 上传 jar 包
```bash
# 本地（Windows）执行，推送到服务器
scp target/xxx.jar root@192.168.1.100:/usr/local/app/
# 或者用 Xftp / WinSCP 拖拽上传
```

### 3. 运行方式一：java -jar + nohup（简单直接）
```bash
mkdir -p /usr/local/app
cd /usr/local/app
# 后台运行并把日志写入文件，&结尾表示后台
nohup java -jar xxx.jar --spring.profiles.active=prod > app.log 2>&1 &
# 验证
tail -f app.log        # 实时看启动日志，看到 "Started ... in x seconds" 即启动成功
jps                    # 查看 java 进程
```
> ⚠️ 不写 `nohup ... &` 的话，关闭终端 SSH 连接进程就被杀了；日志必须重定向到文件，否则没有日志可看。

### 4. 运行方式二：systemd 服务（生产推荐，开机自启 + 崩溃自动重启）
```bash
vi /etc/systemd/system/app.service
```
```ini
[Unit]
Description=Spring Boot App
After=network.target

[Service]
Type=simple
User=root
WorkingDirectory=/usr/local/app
ExecStart=/usr/local/java/jdk1.8.0_391/bin/java -Xms256m -Xmx512m -jar /usr/local/app/xxx.jar --spring.profiles.active=prod
SuccessExitStatus=143
Restart=always            # 崩溃自动重启
RestartSec=10

[Install]
WantedBy=multi-user.target
```
```bash
systemctl daemon-reload
systemctl start app
systemctl enable app     # 开机自启
systemctl status app
journalctl -u app -f     # 查看 systemd 服务日志
```

### 5. 日志查看与常用排错
```bash
tail -f /usr/local/app/app.log        # 实时日志
grep -n "ERROR" /usr/local/app/app.log | tail -50   # 查错误

# 端口被占用（8080被占，启动报 Address already in use）
netstat -tlnp | grep 8080
kill -9 进程号    # 或 fuser -k 8080/tcp 直接按端口杀

# 内存不足（部署多个服务时常见）
free -h

# 数据库连接不上（先测通网络再查授权）
mysql -h 服务器IP -uroot -p
```

### 6. 生产部署建议
1. **内存参数**：`-Xms` 和 `-Xmx` 设置相同值，避免运行期扩容抖动；小项目 256m~512m，大项目按机器内存 1/2~2/3；
2. **独立用户运行**：不要用 root 跑业务进程，创建专用账号（`useradd app`）；
3. **多环境配置**：`application-prod.yml` 单独配置生产数据源、日志级别，启动时 `--spring.profiles.active=prod`；
4. **时区**：数据库连接串加 `serverTimezone=Asia/Shanghai`，启动参数加 `-Duser.timezone=Asia/Shanghai`；
5. **反向代理**：对外统一走 nginx（80 端口 + 域名 + HTTPS），后端 jar 只监听内网 8080；
6. **日志轮转**：配置 logrotate 定期切割日志，防止磁盘写满；
7. **健康检查**：Spring Boot 引入 `spring-boot-starter-actuator`，用 `/actuator/health` 做存活探针。

---

## 十、常见问题速查（避坑清单）

| 现象 | 原因 | 解决 |
|---|---|---|
| 虚拟机没网 | 网卡 ONBOOT=no 或 VMware 网络服务未启动 | 改 ONBOOT=yes；启动 VMware NAT 服务 |
| yum 安装慢/失败 | 官方源在国外 | 换阿里云源（见第三章） |
| 端口连不上 | 防火墙拦截 或 服务没监听 | `firewall-cmd --add-port=端口/tcp --permanent`；`netstat -tlnp` 确认监听 |
| SELinux 导致服务异常 | SELinux 拦截 | 临时 `setenforce 0`，永久改 /etc/selinux/config |
| MySQL 简单密码被拒 | 8.0 密码策略 | `SET GLOBAL validate_password.policy=LOW;` |
| MySQL 远程连不上 | root 只允许 localhost | 创建 `'root'@'%'` 用户并授权 |
| Redis 连不上 | bind/防火墙/密码 | bind 0.0.0.0 + 开放 6379 + requirepass |
| 80 端口启动失败 | 端口被占（如 httpd） | `netstat -tlnp \| grep 80` 查占用并处理 |
| nohup 启动后一关终端就死 | 忘了加 `&` 或没重定向日志 | `nohup java -jar x.jar > app.log 2>&1 &` |
| 服务器时间不准 | 未做时间同步 | `ntpdate ntp.aliyun.com` + 定时任务 |
| 中文乱码 | 系统语言/编码问题 | 设置 `LANG=zh_CN.UTF-8`；程序加 `-Dfile.encoding=UTF-8` |
| 磁盘写满 | 日志/数据占用过大 | `df -h` 定位 → `du -sh *` 排查 → 清理 + logrotate |
