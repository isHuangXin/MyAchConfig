# MyAchConfig
记录打造最适合自己的Arch &amp; Gnome开发环境过程

## 安装系列

### 1. Install arch with GUI

选择Gnome桌面环境的最简安装，进入网页选Gnome Editions -> Gnome Pure Edition
```shell
https://archlinuxgui.in/
```

GUI傻瓜式安装完成后，输入下列指令 wow amzing

```shell
$ sudo pacman -S neofetch
$ neofetch
```

![neofetch.png](./screenshot/neofetch.png)





---



## 踩坑系列

### 1. 声卡的配置 

```shell
# 终端输入alsamixer在F6找不到声卡
3$ alsamixer

# 安装sof-firmware驱动，然后再次进入alsamixer选择声卡 defualt：0 sof-hda-dsp 
# 按M打开headphone声音，just enjoy music
$ sudo pacman - S sof-firmware
```

![sound-card-config.png](./screenshot/sound-card-config.png)



### 2. weixin.deepin安装后中文字体显示为方框

```shell
# 安装weixin.deepin
$ pacman -S deep-wine-wechat

# 显示方框是因为缺少中文字体
$ sudo pacman -S wqy-microhei
```

![wechat.png](./screenshot/wechat.png)



### 3. 错误2002(HY000)：无法通过Socket‘/var/run/mysqld/mysqld.sock’连接到本地MySQL服务器(2)

```shell
➜  ~ mysql         
ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/run/mysqld/mysqld.sock' (2)

# 这就是我为解决我的问题所做的：

# 检查文件是否/var/run/mysqld/mysqld.sock存在。如果没有，则通过以下方式手动创建touch /var/run/mysqld/mysqld.sock。

# 所以MySQL进程可以使用这个文件。通过输入更改所述文件的所有权chown mysql /var/run/mysqld/mysqld.sock

# 一旦“2”完成，请通过以下方式重新启动MySQL服务 sudo systemctl start mysqld.service

# 经过上述步骤后，我的问题得到了解决。

➜  ~ sudo touch /var/run/mysqld/mysqld.sock               
[sudo] password for huangxin: 
➜  ~ cd /var/run/mysqld/            
➜  ~ ls
mysqld.sock
➜  ~ sudo chown mysql /var/run/mysqld/mysqld.sock                                 
➜  ~ sudo systemctl start mysqld.service
➜  ~systemctl status mysqld.service
● mysqld.service - MySQL Server
     Loaded: loaded (/usr/lib/systemd/system/mysqld.service; enabled; vendor preset: di>
     Active: active (running) since Sat 2022-04-09 11:15:41 CST; 6s ago
       Docs: man:mysqld(8)
             http://dev.mysql.com/doc/refman/en/using-systemd.html
    Process: 239759 ExecStartPre=/usr/bin/mysqld_pre_systemd (code=exited, status=0/SUC>
   Main PID: 239782 (mysqld)
     Status: "Server is operational"
      Tasks: 38 (limit: 18776)
     Memory: 362.2M
        CPU: 601ms
     CGroup: /system.slice/mysqld.service
             └─239782 /usr/bin/mysqld

# bug fixed, just enjoy
➜  mysqld mysql -uroot -p     
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 8.0.28

Copyright (c) 2000, 2022, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> 

```

