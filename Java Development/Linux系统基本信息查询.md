# Linux系统基本信息查询

## 1、查看Linux发行版

```shell
#适用于所有的Linux发行版
cat /etc/issue
#这种方法只适合Redhat系的Linux：
cat /etc/redhat-release
#列出所有版本信息
lsb_release -a
#查看当前操作系统版本信息
cat /proc/version
```



## 2、查看内核版本、位数

```shell
cat /proc/version
uname -a

#查看版本说明当前CPU运行在32bit模式下， 但不代表CPU不支持64bit
getconf LONG_BIT
#查看服务器名称
hostname 
```



## 3、CPU信息

```shell
cat /proc/cpuinfo | grep "model name" && cat /proc/cpuinfo |grep "physical id"
#Linux下可以在/proc/cpuinfo中看到每个cpu的详细信息。但是对于双核的cpu，在cpuinfo中会看到两个cpu。常常会让人误以为是两个单核的cpu。其实应该通过Physical Processor ID来区分单核和双核。而Physical Processor ID可以从cpuinfo或者dmesg中找到. flags 如果有 ht 说明支持超线程技术 判断物理CPU的个数可以查看physical id 的值，相同则为
```



## 4、内存信息

```shell
cat /proc/meminfo | grep MemTotal
```



## 5、硬盘信息

```shell
fdisk -l | grep Disk
```





## 6、常用系统命令

```shell
uname -a # 查看内核/操作系统/CPU信息 
head -n 1 /etc/issue # 查看操作系统版本 
cat /proc/cpuinfo # 查看CPU信息 
hostname # 查看计算机名 
lspci -tv # 列出所有PCI设备 
lsusb -tv # 列出所有USB设备 
lsmod # 列出加载的内核模块 
env # 查看环境变量资源 
free -m # 查看内存使用量和交换区使用量 
df -h # 查看各分区使用情况 
du -sh <目录名> # 查看指定目录的大小 
grep MemTotal /proc/meminfo # 查看内存总量 
grep MemFree /proc/meminfo # 查看空闲内存量 
uptime # 查看系统运行时间、用户数、负载 
cat /proc/loadavg # 查看系统负载磁盘和分区 
mount | column -t # 查看挂接的分区状态 
fdisk -l # 查看所有分区 
swapon -s # 查看所有交换分区 
hdparm -i /dev/hda # 查看磁盘参数(仅适用于IDE设备) 
dmesg | grep IDE # 查看启动时IDE设备检测状况网络 
ifconfig # 查看所有网络接口的属性 
iptables -L # 查看防火墙设置 
route -n # 查看路由表 
netstat -lntp # 查看所有监听端口 
netstat -antp # 查看所有已经建立的连接 
netstat -s # 查看网络统计信息进程 
ps -ef # 查看所有进程 
top # 实时显示进程状态用户 
w # 查看活动用户 
id <用户名> # 查看指定用户信息 
last # 查看用户登录日志 
cut -d: -f1 /etc/passwd # 查看系统所有用户 
cut -d: -f1 /etc/group # 查看系统所有组 
crontab -l # 查看当前用户的计划任务服务 
chkconfig –list # 列出所有系统服务 
chkconfig –list | grep on # 列出所有启动的系统服务程序 

rpm -qa # 查看所有安装的软件包
```



## 7、查看JDK版本

```shell
echo $JAVA_HOME
echo $PATH

#环境变量配置文件
vi /etc/profile
```



## 8、查看Weblogic版本

```shell
vi /bea/bea/logs/log.txt#或者下面的写法
vi /bea/logs/log.txt
```

