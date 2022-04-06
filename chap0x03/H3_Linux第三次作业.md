# H3_Linux第三次作业

### 实验环境：

宿主机：Windows 10

虚拟机:  Ubuntu 20.04.2LTS



## 实验要求：

- Systemd跟练

- 自查清单

## 实验过程以及结果：

- ### Systemd跟练

  - #### [Systemd 入门教程：命令篇 by 阮一峰的网络日志](http://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-commands.html)

    1. ##### 系统管理：

       ###### 代码如下：

       ```shell
       # 查看启动耗时
       $ systemd-analyze                                                                                       
       
       # 查看每个服务的启动耗时
       $ systemd-analyze blame
       
       # 显示瀑布状的启动过程流
       $ systemd-analyze critical-chain
       
       # 显示指定服务的启动流
       $ systemd-analyze critical-chain atd.service
       
       # 显示当前主机的信息
       $ hostnamectl
       
       # 设置主机名。
       $ sudo hostnamectl set-hostname cuc-ou
       
       # 查看本地化设置
       $ localectl
       
       # 设置本地化参数。
       $ sudo localectl set-locale LANG=en_GB.utf8
       $ sudo localectl set-keymap en_GB
       
       # 查看当前时区设置
       $ timedatectl
       
       # 显示所有可用的时区
       $ timedatectl list-timezones                                                                                   
       
       # 设置当前时区
       $ sudo timedatectl set-timezone Asia/Hong_Kong
       $ sudo timedatectl set-time YYYY-MM-DD
       $ sudo timedatectl set-time HH:MM:SS
       
       # 列出当前session
       $ loginctl list-sessions
       
       # 列出当前登录用户
       $ loginctl list-users
       
       # 列出显示指定用户的信息
       $ loginctl show-user cuc
       ```

       ###### asciinema录屏：[![asciicast](https://asciinema.org/a/r5pwIzDvSxFEkkmMxMR7QlrDC.svg)](https://asciinema.org/a/r5pwIzDvSxFEkkmMxMR7QlrDC)

       

    2. ##### Unit:

       ###### 代码如下：

       ```shell
       # 列出正在运行的 Unit
       $ systemctl list-units
       
       # 列出所有Unit，包括没有找到配置文件的或者启动失败的
       $ systemctl list-units --all
       
       # 列出所有没有运行的 Unit
       $ systemctl list-units --all --state=inactive
       
       # 列出所有加载失败的 Unit
       $ systemctl list-units --failed
       
       # 列出所有正在运行的、类型为 service 的 Unit
       $ systemctl list-units --type=service
       
       # 显示系统状态
       $ systemctl status
       
       # 显示单个 Unit 的状态
       $ sysystemctl status ssh.service
       
       # 显示某个 Unit 是否正在运行
       $ systemctl is-active ssh.service
       
       # 显示某个 Unit 是否处于启动失败状态
       $ systemctl is-failed ssh.service
       
       # 显示某个 Unit 服务是否建立了启动链接
       $ systemctl is-enabled ssh.service
       
       # 立即启动一个服务
       $ sudo systemctl start ssh.service
       $ systemctl is-active ssh.service
       
       # 立即停止一个服务
       $ sudo systemctl stop ssh.service
       
       # 重启一个服务
       $ sudo systemctl restart ssh.service
       
       # 杀死一个服务的所有子进程
       $ sudo systemctl kill ssh.service
       
       # 重新加载一个服务的配置文件
       $ sudo systemctl reload apache.service
       
       # 重载所有修改过的配置文件
       $ sudo systemctl daemon-reload
       
       # 显示某个 Unit 的所有底层参数
       $ systemctl show atd.service
       
       # 显示某个 Unit 的指定属性的值
       $ systemctl show -p CPUShares atd.service
       
       # 设置某个 Unit 的指定属性
       $ sudo systemctl set-property atd.service CPUShares=500
       
       $ systemctl list-dependencies nginx.service
       
       $ systemctl list-dependencies --all nginx.service
       
       ```

       ###### asciinema录屏：[![asciicast](https://asciinema.org/a/akwjlot5lsE3yGfDiL9oO4zMs.svg)](https://asciinema.org/a/akwjlot5lsE3yGfDiL9oO4zMs)

       

    3. ##### Unit的配置文件：

       ###### 代码如下：

       ```shell
       $ sudo systemctl enable ssh.service
       
       $ sudo systemctl disable ssh.service
       
       # 列出所有配置文件
       $ systemctl list-unit-files
       
       # 列出指定类型的配置文件
       $ systemctl list-unit-files --type=service
       
       $ systemctl status ssh.service
       
       $ sudo systemctl daemon-reload
       $ sudo systemctl restart atd.service
       
       $ systemctl cat atd.service
       ```

       ###### asciinema录屏：[![asciicast](https://asciinema.org/a/JuOt1PurPWB4a5KRDNPNbWdqx.svg)](https://asciinema.org/a/JuOt1PurPWB4a5KRDNPNbWdqx)

       

    4. ##### Target:

       ###### 代码如下：

       ```shell
       # 查看当前系统的所有 Target
       $ systemctl list-unit-files --type=target
       
       # 查看一个 Target 包含的所有 Unit
       $ systemctl list-dependencies multi-user.target
       
       # 查看启动时的默认 Target
       $ systemctl get-default
       
       # 设置启动时的默认 Target
       $ sudo systemctl set-default multi-user.target
       
       # 切换 Target 时，默认不关闭前一个 Target 启动的进程，
       # systemctl isolate 命令改变这种行为，
       # 关闭前一个 Target 里面所有不属于后一个 Target 的进程
       $ sudo systemctl isolate multi-user.target
       $ systemctl list-unit-files --type=targetcl
       ```

       ###### asciinema录屏：[![asciicast](https://asciinema.org/a/fyoNkFeZrVbBViqrsnJTEKCs2.svg)](https://asciinema.org/a/fyoNkFeZrVbBViqrsnJTEKCs2)

       

    5. ##### 日志管理：

       ###### 代码如下：

       ```shell
       # 查看所有日志（默认情况下 ，只保存本次启动的日志）
       $ sudo journalctl
       
       # 查看内核日志（不显示应用日志）
       $ sudo journalctl -k
       
       # 查看系统本次启动的日志
       $ sudo journalctl -b
       $ sudo journalctl -b -0
       
       # 查看上一次启动的日志（需更改设置）
       $ sudo journalctl -b -1
       
       # 查看指定时间的日志
       $ sudo journalctl --since "20 min ago"
       $ sudo journalctl --since yesterday
       
       # 显示尾部的最新10行日志
       $ sudo journalctl -n
       
       # 显示尾部指定行数的日志
       $ sudo journalctl -n 20
       
       # 实时滚动显示最新日志
       $ sudo journalctl -f
       
       # 查看指定服务的日志
       $ sudo journalctl /usr/lib/systemd/systemd
       
       # 查看指定进程的日志
       $ sudo journalctl _PID=1
       
       # 查看某个路径的脚本的日志
       $ sudo journalctl /usr/bin/bash
       
       # 查看指定用户的日志
       $ sudo journalctl _UID=33 --since today
       
       # 查看某个 Unit 的日志
       $ sudo journalctl -u nginx.service
       $ sudo journalctl -u nginx.service --since today
       
       # 实时滚动显示某个 Unit 的最新日志
       $ sudo journalctl -u nginx.service -f
       
       # 合并显示多个 Unit 的日志
       $ journalctl -u nginx.service -u php-fpm.service --since today
       
       # 查看指定优先级（及其以上级别）的日志，共有8级
       # 0: emerg
       # 1: alert
       # 2: crit
       # 3: err
       # 4: warning
       # 5: notice
       # 6: info
       # 7: debug
       $ sudo journalctl -p err -b
       
       # 日志默认分页输出，--no-pager 改为正常的标准输出
       $ sudo journalctl --no-pager
       
       # 以 JSON 格式（单行）输出
       $ sudo journalctl -b -u nginx.service -o json
       
       # 以 JSON 格式（多行）输出，可读性更好
       $ sudo journalctl -b -u nginx.serviceqq
        -o json-pretty
       
       # 显示日志占据的硬盘空间
       $ sudo journalctl --disk-usage
       
       # 指定日志文件占据的最大空间
       $ sudo journalctl --vacuum-size=1G
       
       # 指定日志文件保存多久
       $ sudo journalctl --vacuum-time=1years
       
       ```

       ###### asciinema录屏：[![asciicast](https://asciinema.org/a/QqdVJ1Uh8idnJ2jpclAv4sTkZ.svg)](https://asciinema.org/a/QqdVJ1Uh8idnJ2jpclAv4sTkZ)

  

  ------

  

  - #### [Systemd 入门教程：实战篇 by 阮一峰的网络日志](http://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-part-two.html)

    ###### 代码如下：

    ```shell
    $ sudo systemctl enable ssh
    
    $ sudo systemctl start ssh
    
    $ sudo systemctl stop ssh.service
    
    $ systemctl is-active ssh.service
    
    $ sudo systemctl kill ssh.service
    
    $ sudo systemctl restart ssh.service
    
    $ systemctl cat ssh.service
    
    $ systemctl get-default
    
    $ systemctl list-dependencies multi-user.target
    
    $ systemctl cat multi-user.target
    
    $ sudo systemctl daemon-reload
    
    $ sudo systemctl restart ssh
    ```

    ###### asciinema录屏：[![asciicast](https://asciinema.org/a/no8NRWfMs1aMUCHNAv0vVFpMj.svg)](https://asciinema.org/a/no8NRWfMs1aMUCHNAv0vVFpMj)



------



- ### 自查清单

  - #### 如何添加一个用户并使其具备sudo执行程序的权限？

    ###### 代码如下：

    ```shell
    sudo adduser user
    
    sudo usermod -G sudo user
    
    getent group sudo
    ```

    ###### 使用`sudo adduser user`命令新建用户`user`：

    ![adduser](img/adduser.png)

    ###### 给予用户权限：

    ![usermod](img/usermod.png)

  

  ------

  

  - #### 如何将一个用户添加到一个用户组？

    ###### 代码如下：

    ```shell
    sudo addgroup group_1
    
    sudo usermod -a -G group_1 user
    
    groups user
    ```

    ###### 新建并将用户`user`添加用户组`group_1`:

    ![group](img/group.png)

    ###### 检查用户是否添加进用户组：

    ![check_group](img/check_group.png)

  

  ------

  

  - #### 如何查看当前系统的分区表和文件系统详细信息？

    ###### 代码如下：

    ```shell
    sudo fdisk -l
    
    df -h
    ```

    ###### 分区表：

    ![fdisk](img/fdisk.png)

    ###### 文件系统详细信息：

    ![detail](img/detail.png)

  

  ------

  

  - #### 如何实现开机自动挂载Virtualbox的共享目录分区？

    ###### 代码如下：
  
    ```shell
    sudo mount -t vboxsf sharefile /mnt/share
    
    sudo vim /etc/fstab
    ```
  
    ###### 创建并在VirtualBox中添加共享文件夹：
  
    ![share](img/share.png)
  
    ###### 在虚拟机上挂载：
  
    ![mount](img/mount.png)
  
    ###### 实现开机自动挂载：
  
    ![vim](img/vim.png)
  
    ![check](img/check.png)
  
  
  
  ------
  
  
  
  - #### 基于LVM（逻辑分卷管理）的分区如何实现动态扩容和缩减容量？
  
    ###### 代码如下：
  
    ```shell
    ##创建磁盘分区
    fdisk /dev/sdb
    
    ##PV阶段
    pvcreate /dev/sdb{1,2,3} -y 
    
    ##VG阶段
    vgcreate test-vg /dev/sdb{1,2} 
    
    ##LV阶段
    lvcreate -L 4G -n test-lv test-vg 
    
    ##扩容
    lvextend -L +100M /dev/test-vg/test-lv 
    
    ##减容
    lvreduce -L -100M /dev/test-vg/test-lv 
    ```
  
    
  
    ###### 添加虚拟磁盘并创建分区：
  
    ![disk](img/disk.png)
  
    ![check_disk](img/check_disk.png)
  
    ###### PV阶段：
  
    ![PV](img/PV.png)
  
    ###### VG阶段：
  
    ![VG](img/VG.png)
  
    ###### LV阶段：
  
    ![LV](img/LV.png)
  
    ###### 磁盘扩/减容：
  
    ![change](img/change.png)
  
  
  
  ------
  
  
  
  - #### 如何通过 systemd 设置实现在网络连通时运行一个指定脚本，在网络断开时运行另一个脚本？
  
    ###### 代码如下：
  
    ```shell
    systemctl start test.service
    
    systemctl status test.service
    
    systemctl stop network-online.target
    ```
  
    ###### 编写test.service脚本：
  
    ![test.service](img/test.service.png)
  
    ###### 运行脚本：
  
    ![start](img/start.png)
  
    ###### 停止脚本：
  
    ![stop](img/stop.png)
  
  
  
  ------
  
  ##### 
  
  - #### 如何通过 systemd 设置实现一个脚本在任何情况下被杀死之后会立即重新启动,实现''杀不死''?
  
    ```shell
    ##修改test.service配置文件，将service区块中的restart改为always
    
    [Service]
    Restart =always
    
    ##加载配置文件
    sudo systemctl daemon-reload
    
    ##重新启动服务
    sudo systemctl restart test.service
    ```
  
    
  
  ------
  
  ## 实验中遇到的问题以及解决方法：

1. 分配磁盘分区时无法分配。

   通过查阅资料了解到需要获得root权限才能分配磁盘。（还有好几次分配出错了，快照救我）

2. 配置脚本

   创建脚本之后试图运行脚本却无法运行，最后才发现没配置脚本文件，使用命令`sudo vim /etc/systemd/system/test.service`编辑脚本文件才能正常运行

## 参考资料

- [Systemd 入门教程：命令篇 by 阮一峰的网络日志](http://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-commands.html)
- [Systemd 入门教程：实战篇 by 阮一峰的网络日志](http://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-part-two.html)
- [vbox linux共享文件夹权限,VirtualBox 共享文件夹权限设置及使用方法](https://blog.csdn.net/weixin_36378793/article/details/116823324)
- [Linux磁盘的分区操作](https://blog.csdn.net/TE_M_D/article/details/120910408)
- [Linux中利用LVM实现分区动态扩容](https://blog.csdn.net/seteor/article/details/6708025/)
- [Linux添加自定义服务（service文件)](https://blog.csdn.net/yanhanhui1/article/details/117196904)

