#### 查看登录历史
```
last -f /var/log/wtmp
```

#### 查看所有监听端口
```
netstat -nltp
```
#### 查看文件大小
```
du -h filename
```

#### 查看磁盘使用情况
```
Filesystem  Size  Used Avail Use% Mounted on  
/dev/hdb2   75G   75G    0   100%    /  

HD硬盘接口的第二个硬盘(b)
容量是75G
用了75G
可用是0
因此利用率是100%
被挂载到根分区目录上(/)

df -hl
```

#### 查看内存
```
free -h
```

#### 恢复数据
```
dd if=/dev/zero of=/dev/vda bs=1G count=9999
```

#### 查看网速
```
sar -n DEV #查看当天从零点到当前时间的网卡流量信息  
sar -n DEV 1 10 #每秒显示一次，共显示10次  
sar -n DEV -f /var/log/sa/saxx #查看xx日的网卡流量历史  
sar -q #查看历史负载  
sar -b #查看磁盘读写  
```

#### 查看逻辑CPU的个数
```
cat /proc/cpuinfo | grep name | cut -f2 -d: | uniq -c
```

#### 实际物理cpu个数
```
cat /proc/cpuinfo | grep physical | uniq -c
```

#### 系统版本
```
cat /etc/issue
```

#### 后台运行
```
nohup ./test >> ./logName.log 2>&1 &
```

#### 文件查看
```
tail -f fileName.log    实时查看
cat filename.log
```

#### 清空文件
- cat /dev/null > test.txt



#### 禁用Guest用户
1、打开终端（快捷键 Ctrl+Alt+T）

2、编辑50-no-guest.conf文件，按照以下命令编辑，

sudo gedit /usr/share/lightdm/lightdm.conf.d/50-no-guest.conf

3、在打开的窗口中输入

[SeatDefaults]

allow-guest=false

4、保存，重启系统，即可禁用Guest用户（Guest User Session）