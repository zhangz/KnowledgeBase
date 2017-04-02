## Commands
- `mkdir` 创建目录
- `rmdir` 删除目录
- `cd` 切换目录
  - `cd /` 到root目录
  - `cd ~` 到当前用户的home目录
- `pwd` 打印工作目录路径
- `whoami` 打印登录用户id
- `ls` 列目录
  - `ls -a` 包含权限信息
  - `ls -la` 显示隐藏文件及目录
- `clear` 清屏
- `free` 查看内存，默认以kb为单位
  - `free -g`
  - `free -m`
- `cp` 复制文件
- `mv` 更改文件名
- `rm` 删除文件
- `chmod` 改变权限
- `cat` 编辑文件
- `less` 显示文件内容
- `cat /proc/version` 查看Linux内核版本信息
- `lsb_release -a` 查看Linux系统版本信息
- 配置DNS
  - `vi /etc/resolv.conf` 加入:
  - ```
    nameserver 8.8.8.8
    nameserver 8.8.4.4
    ```

## Reference
http://linuxcommand.org/lc3_learning_the_shell.php

