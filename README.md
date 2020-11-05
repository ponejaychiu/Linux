# 一、Linux系统基础命令类；

## 1、查看文件大小并列出前10个最大的：
```du -sh * |sort -rh |head -10```

## 2、不重启服务器修改hostname：
```hostnamectl set-hostname XXX```

## 3、查看CPU相关信息：
<!--如果CPU信息有两个相同的”core id”，那么CPU是有超线程的；
总物理核数 = 物理CPU个数 X 每颗物理CPU的核数
总逻辑核数 = 物理CPU个数 X 每颗物理CPU的核数 X 2(超线程数)'-->
### 查看物理CPU个数
```cat /proc/cpuinfo| grep "physical id"| sort uniq| wc -l```
### 查看每个物理CPU中core的个数(即核数)
```cat /proc/cpuinfo| grep "cpu cores"| uniq | awk '{print $4}'```
### 查看逻辑CPU的个数
```cat /proc/cpuinfo| grep "processor"| wc -l```
### 查看CPU信息（型号）
```cat /proc/cpuinfo | grep name | cut -f2 -d: | uniq -c```

## 4、端口相关：
### 查看端口是否开启：
```netstat -an |grep 端口号```
### 查看端口被哪个程序占用：
```netstat -apn |grep 端口号```

```lsof -i:端口号```

# 二、Linux系统基础配置类；
## 1、IP设置
### 静态IP设置：
```
ONBOOT=YES       #开机自启动
BOOTPROTO=STATIC  #设置静态IP
IPADDR=*.*.*.*      #ip地址
NETMASK=*.*.*.*    #子网掩码
GATEWAY=*.*.*.*    #网关
DNS=*.*.*.*        #DNS
DEVICE=eth0        #网卡名称
```

### 动态IP设置：
```
DEVICE=eth1
BOOTPROTO=dhcp   #动态获取IP
HWADDR=00:15:17:B2:DC:B7
ONBOOT=yes
```
### 运行命令获取动态IP地址
```dhclient  eth1```

## 2、ifconfig网卡信息不一致：
### 如果出现ifconfig显示的eth实际系统中不存在此eth时,先新建eth编辑后再删除下面的文件:
```rm -rf /etc/udev/rules.d/70-persistent-net.rules```
### 再重启系统即可;
# 三、