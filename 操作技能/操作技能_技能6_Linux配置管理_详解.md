# 操作技能 · 技能六 Linux（CentOS 7）配置管理（详解）

> **分值占比**：约8分（占操作技能10%）
> **考频**：⭐⭐⭐（常考命令）
> **难度系数**：⭐⭐☆☆☆
> **学习时长建议**：2-3周

---

## 一、Linux 基础

### 1.1 Linux 特点

| 特点 | 说明 |
|------|------|
| 开源免费 | 自由使用 |
| 多用户 | 多个用户同时用 |
| 多任务 | 同时运行多个程序 |
| 稳定 | 服务器首选 |
| 安全 | 权限严格 |
| 命令行 | 强大Shell |

### 1.2 常见 Linux 发行版

| 发行版 | 特点 |
|--------|------|
| **CentOS** | 稳定、服务器（已转为CentOS Stream）|
| **Ubuntu** | 桌面友好 |
| **Red Hat (RHEL)** | 商业版 |
| **Debian** | 稳定 |
| **Fedora** | 新特性试验田 |

### 1.3 CentOS 7 安装（了解）

- 镜像：CentOS-7-x86_64-DVD
- 虚拟机或物理机
- 分区方案：/boot, /, swap

---

## 二、Linux 文件系统

### 2.1 目录结构 ⚠️ 必考

```
/         根目录
├── /bin        基本命令
├── /sbin       管理员命令
├── /etc        配置文件
├── /home       普通用户主目录
├── /root       root用户主目录
├── /var        可变数据（日志）
├── /tmp        临时文件
├── /usr        用户程序
├── /opt        可选软件
├── /dev        设备文件
├── /proc       进程信息（虚拟）
├── /mnt        临时挂载
└── /lib        库文件
```

**【记忆】** "**根**/bin/sbin、**配置**/etc、**用户**/home/usr"

### 2.2 文件类型

| 符号 | 类型 |
|------|------|
| - | 普通文件 |
| d | 目录 |
| l | 链接 |
| c | 字符设备 |
| b | 块设备 |
| p | 管道 |
| s | 套接字 |

### 2.3 路径

| 类型 | 写法 | 例子 |
|------|------|------|
| **绝对路径** | 以 / 开头 | /home/user/file |
| **相对路径** | 不以 / 开头 | ./file, ../dir |

---

## 三、常用命令 ⚠️ 必考

### 3.1 文件和目录操作 ⚠️ 必考

| 命令 | 作用 | 例 |
|------|------|---|
| `ls` | 列表 | `ls -l`, `ls -a`, `ls -la` |
| `cd` | 切换目录 | `cd /home`, `cd ..`, `cd ~` |
| `pwd` | 显示当前目录 | `pwd` |
| `mkdir` | 创建目录 | `mkdir dir1`, `mkdir -p a/b/c` |
| `rmdir` | 删除空目录 | `rmdir dir1` |
| `rm` | 删除文件/目录 | `rm file`, `rm -rf dir` |
| `cp` | 复制 | `cp src dst`, `cp -r dir1 dir2` |
| `mv` | 移动/重命名 | `mv old new` |
| `touch` | 创建空文件/更新时间 | `touch file` |
| `find` | 查找文件 | `find / -name "*.txt"` |
| `locate` | 快速查找 | `locate file` |

**【ls 选项】**
| 选项 | 含义 |
|------|------|
| -l | 详细列表 |
| -a | 显示隐藏文件 |
| -h | 人性化大小 |
| -t | 按时间排序 |
| -r | 反向 |

**【rm 慎用】** `rm -rf /` 会删除所有文件

**【例题1】** 复制 /etc/passwd 到 /tmp，保留属性
```bash
cp -p /etc/passwd /tmp/
```

### 3.2 文件查看命令 ⚠️ 必考

| 命令 | 作用 |
|------|------|
| `cat` | 全部显示 |
| `less` | 分页，可上下翻 |
| `more` | 分页，只能下 |
| `head` | 前N行 |
| `tail` | 后N行 |
| `grep` | 过滤 |

**【常用】**
```bash
cat /etc/passwd
less /var/log/messages
head -5 file.txt        # 前5行
tail -10 file.txt       # 后10行
tail -f /var/log/messages   # 实时跟踪
grep "error" file.txt   # 搜索"error"
grep -i "error" file.txt  # 忽略大小写
grep -n "error" file.txt  # 显示行号
grep -v "error" file.txt  # 反向（不包含）
```

**【例题2】** 查看 /etc/passwd 的第5-10行
```bash
head -10 /etc/passwd | tail -6
```

### 3.3 用户和权限 ⚠️ 必考

**【用户管理】**

| 命令 | 作用 | 例 |
|------|------|---|
| `useradd` | 添加用户 | `useradd tom` |
| `passwd` | 设置密码 | `passwd tom` |
| `userdel` | 删除用户 | `userdel -r tom` |
| `usermod` | 修改用户 | `usermod -g group user` |
| `groupadd` | 添加组 | `groupadd dev` |
| `su` | 切换用户 | `su - tom` |

**【例题3】** 创建用户 tom，加入 dev 组
```bash
useradd tom
passwd tom
usermod -aG dev tom
```

**【权限管理】** ⚠️ 必考

格式：`-rwxr-xr--`

- 第1位：文件类型
- 2-4位：所有者（u）权限
- 5-7位：组（g）权限
- 8-10位：其他（o）权限

权限值：r=4, w=2, x=1

| 命令 | 作用 |
|------|------|
| `chmod` | 改权限 |
| `chown` | 改所有者 |
| `chgrp` | 改组 |

**【chmod 两种方式】**

数字法：
```bash
chmod 755 file      # rwxr-xr-x
chmod 644 file      # rw-r--r--
chmod 777 file      # 全部权限
```

字母法：
```bash
chmod u+x file      # 所有者加执行
chmod g-w file      # 组减写
chmod o=r file      # 其他只读
chmod a+r file      # 所有都加读
```

**【例题4】** 把 file.txt 改为所有者读写执行，组读，其他无权限
```bash
chmod 740 file.txt
```

### 3.4 进程管理

| 命令 | 作用 |
|------|------|
| `ps` | 进程快照 |
| `top` | 动态进程 |
| `kill` | 杀进程 |
| `bg` | 后台运行 |
| `fg` | 前台运行 |

```bash
ps aux               # 所有进程
ps -ef               # 完整格式
top                  # 动态查看
kill PID             # 杀进程
kill -9 PID          # 强制杀
```

### 3.5 网络配置 ⚠️ 必考

```bash
# 查看 IP（CentOS 7）
ip addr
ip a
ifconfig

# 测试连通
ping 192.168.1.1
ping www.baidu.com

# 查看路由
ip route
route -n
```

**【修改 IP】** CentOS 7：
```bash
vi /etc/sysconfig/network-scripts/ifcfg-ens33
```
修改后重启网络：
```bash
systemctl restart network
```

**【常见参数】**
| 参数 | 说明 |
|------|------|
| BOOTPROTO | 启动协议（static/dhcp）|
| IPADDR | IP地址 |
| NETMASK | 子网掩码 |
| GATEWAY | 网关 |
| DNS1 | DNS服务器 |
| ONBOOT | 开机启动 |

**【例题5】** ifcfg-ens33 配置静态 IP
```
BOOTPROTO=static
ONBOOT=yes
IPADDR=192.168.1.100
NETMASK=255.255.255.0
GATEWAY=192.168.1.1
DNS1=8.8.8.8
```

### 3.6 防火墙（CentOS 7）

```bash
# 查看状态
systemctl status firewalld

# 启动/停止
systemctl start firewalld
systemctl stop firewalld

# 开放端口
firewall-cmd --zone=public --add-port=80/tcp --permanent
firewall-cmd --reload
```

### 3.7 软件包管理 ⚠️

**【yum（CentOS）】**
```bash
yum install package      # 安装
yum remove package       # 卸载
yum update               # 更新
yum search keyword       # 搜索
yum list                 # 列表
```

**【rpm】**
```bash
rpm -ivh package.rpm     # 安装
rpm -e package           # 卸载
rpm -qa                  # 列出所有
```

### 3.8 服务管理 ⚠️ 必考

**【systemctl（CentOS 7）】**
```bash
systemctl start service      # 启动
systemctl stop service       # 停止
systemctl restart service    # 重启
systemctl status service     # 状态
systemctl enable service     # 开机启动
systemctl disable service    # 禁用开机启动
```

**【常见服务】**
| 服务 | 用途 |
|------|------|
| sshd | SSH服务 |
| httpd | Apache |
| nginx | Nginx |
| mysqld | MySQL |
| firewalld | 防火墙 |
| network | 网络 |

---

## 四、Vi/Vim 编辑器 ⚠️ 必考

### 4.1 三种模式

| 模式 | 说明 |
|------|------|
| **命令模式** | 刚进入 |
| **插入模式** | 编辑文本 |
| **末行模式** | 底线命令 |

### 4.2 进入

```bash
vi file.txt        # 打开
```

### 4.3 模式切换

| 命令 | 作用 |
|------|------|
| i | 插入（光标前）|
| a | 追加（光标后）|
| o | 新行 |
| ESC | 回命令模式 |
| : | 进末行模式 |

### 4.4 常用命令

| 命令 | 作用 |
|------|------|
| :w | 保存 |
| :q | 退出 |
| :wq | 保存退出 |
| :q! | 强制退出 |
| :set nu | 显示行号 |
| /word | 搜索 word |
| dd | 删行 |
| yy | 复制行 |
| p | 粘贴 |
| u | 撤销 |

**【例题6】** 用 vi 编辑 /etc/hosts
```bash
vi /etc/hosts
# 进入后按 i 编辑
# 编辑完按 ESC，输入 :wq
```

---

## 五、磁盘与文件操作

### 5.1 磁盘命令

```bash
df -h           # 磁盘使用
du -sh dir      # 目录大小
fdisk -l        # 磁盘分区
mount /dev/sdb1 /mnt   # 挂载
umount /mnt            # 卸载
```

### 5.2 链接文件

```bash
ln file link       # 硬链接
ln -s file slink   # 软链接
```

---

## 六、查找和文本处理

### 6.1 find ⚠️

```bash
find / -name "*.log"          # 按名字
find / -size +100M            # 按大小
find / -type d                # 按类型
find / -mtime -7              # 7天内修改
```

### 6.2 grep ⚠️ 必考

```bash
grep "pattern" file
grep -i "pattern" file         # 忽略大小写
grep -n "pattern" file         # 显示行号
grep -v "pattern" file         # 反向
grep -r "pattern" /etc/        # 递归
```

### 6.3 管道 `|`

把前一个命令的输出作为后一个命令的输入

```bash
cat /etc/passwd | grep "root"
ps aux | grep "nginx"
ls -l | more
```

### 6.4 重定向

| 符号 | 作用 |
|------|------|
| `>` | 覆盖写入 |
| `>>` | 追加写入 |
| `<` | 从文件读入 |
| `2>` | 错误输出 |

```bash
ls > file.txt          # 输出到文件
echo "hello" >> file   # 追加
```

---

## 七、Shell 基础

### 7.1 变量

```bash
name="Tom"
echo $name
echo "Hello $name"
```

### 7.2 环境变量

```bash
echo $PATH
echo $HOME
echo $USER
```

### 7.3 简单脚本

```bash
#!/bin/bash
# 这是注释
echo "Hello World"
name="Linux"
echo "Welcome to $name"
```

保存为 test.sh，加上执行权限：
```bash
chmod +x test.sh
./test.sh
```

---

## 八、cron 定时任务

### 8.1 crontab 命令

```bash
crontab -e       # 编辑
crontab -l       # 列表
crontab -r       # 删除
```

### 8.2 格式

```
分 时 日 月 周 命令
*  *  *  *  *  command
```

### 8.3 例题

| 任务 | 表达式 |
|------|--------|
| 每分钟 | `* * * * * cmd` |
| 每小时 | `0 * * * * cmd` |
| 每天0点 | `0 0 * * * cmd` |
| 每周一8点 | `0 8 * * 1 cmd` |
| 每月1号 | `0 0 1 * * cmd` |

**【例题7】** 每天凌晨2点备份 /home 到 /backup
```
0 2 * * * tar -czf /backup/home_$(date +\%Y\%m\%d).tar.gz /home
```

---

## 九、日志管理

### 9.1 重要日志

| 日志 | 位置 |
|------|------|
| 系统日志 | /var/log/messages |
| 认证日志 | /var/log/secure |
| 启动日志 | /var/log/boot.log |
| 计划任务 | /var/log/cron |
| 内核日志 | /var/log/dmesg |
| yum日志 | /var/log/yum.log |

### 9.2 查看日志

```bash
tail -f /var/log/messages      # 实时跟踪
grep "error" /var/log/messages # 搜索
```

---

## 十、SSH 远程登录

### 10.1 SSH 登录

```bash
ssh user@ip
ssh root@192.168.1.100
```

### 10.2 远程传输

```bash
# 上传
scp local.txt user@ip:/remote/dir/

# 下载
scp user@ip:/remote/file.txt local/
```

---

## 十一、考点速记表

| 序号 | 考点 | 重要度 |
|------|------|--------|
| 1 | 目录结构 | ⭐⭐⭐⭐ |
| 2 | 文件操作命令 | ⭐⭐⭐⭐⭐ |
| 3 | 查看文件 | ⭐⭐⭐⭐⭐ |
| 4 | chmod | ⭐⭐⭐⭐⭐ |
| 5 | 用户管理 | ⭐⭐⭐⭐ |
| 6 | 网络配置 | ⭐⭐⭐⭐⭐ |
| 7 | systemctl | ⭐⭐⭐⭐ |
| 8 | yum | ⭐⭐⭐⭐ |
| 9 | vi/vim | ⭐⭐⭐⭐ |
| 10 | grep/find | ⭐⭐⭐⭐ |
| 11 | 管道/重定向 | ⭐⭐⭐⭐ |

---

> 📌 **学习建议**：
> 1. 装个 CentOS 7 虚拟机
> 2. 每天练10个命令
> 3. 重点记权限相关
> 4. vi 编辑器必会
> 5. 网络配置必考
