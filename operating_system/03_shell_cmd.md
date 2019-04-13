# Linux 命令行

> 这篇是可以用来教新人用的。从刚装了系统后的使用讲起： 设置用上和密码、浏览文件、安装软件，运行程序。

![img](../resource/linux_basic_cmd.jpg)

## 用户和密码

- passwd 修改密码
- useradd 创建用户

- chown 改变文件所属用户

- chgrp 改变文件所属组

## 浏览文件

- cd 变换目录
- ls 列出文件

## 软件的下载安装

- CentOS 体系
  - 下载安装包（*.rpm）后安装

    - rpm -i 安装
    - rpm -e 删除

  - 通过软件管家安装，软件管家是 yum

    - yum search jdk

    - yum install *program*
    - yum erase *program*

  - 软件列表文件：/etc/yum.repos.d/CentOs-Base.repo

- Ubuntu 体系
  - 下载安装包(*.deb)后安装
    - dpkg -i 安装
    - dpkg -r 删除
    - dpkg -l  | grep *program*  查看安装的软件列表并搜索程序
  - 通过软件管家安装，软件管家是 apt-get
    - apt-cache search jdk
    - apt-get install *program*
    - apt-get purge *program*
  - 软件列表文件： /etc/apt/sources.list

- 软件安装后的存储位置
  - 主执行文件 /usr/bin 或者 /usr/sbin
  - 库文件 /var
  - 配置文件 /etc

- 下载文件
  - wget *url*
- 解压缩
  - tar
  - zip
- 配置环境变量
  - export 仅在当前命令行的会话中管用，退出后重新登录就不可用
  - ~/.bashrc
  - 
- vim 编辑器
- grep 搜索
- | 是管道，用于连接两个程序，前面的输出放到管道里面，作为后者的输入。

- 部分显示，多和 | 合用

  - more 将结果分页展示， 分页后只能往后翻页，翻到最后一页自动结束返回命令行

  - less 往前往后都能翻页，输入q 返回命令行

- 运行程序

  - 在shell 中让程序在前台运行 ./program， 退出命令行后程序关闭

  - 后台运行 nohup  & (no hang up，不要挂起，即退出命令行后，程序还要在， & 后台运行)

    `nohup command > out.file 2>&1 &` 1是文件描述符1，表示标准输出，2是文件描述符2，表示输出错误输出 2>&1 表示合并标准输出和标准错误输出

  - 如何关闭该后台程序

    ```
    ps -ef |grep 关键字  |awk '{print $2}'|xargs kill -9
    
    ps -ef 列出所有当前正在运行的程序
    awk '{print $2}' 指第二列的内容，这里是运行的程序ID
    xargs 传给 kill -9
    ```

  - 以服务的方式运行
    - 比如运行 MySQL: `systemctl start mysql` 启动 MySQL, `system enable mysql` 设置开机启动
    - 背后是在 `/lib/systemd/system` 目录下创建了一个 ***.service 的配置文件，在里面定义了如何启动和关闭，从而成为一个服务

- 关机和重启

  - shutdown -h now 当前关机
  - reboot 重启





