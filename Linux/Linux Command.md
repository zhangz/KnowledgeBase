## Commands
- `.` 当前目录
- `..` 上一级目录
- `mkdir` 创建目录
- `cd` 切换目录
- `cd ..` 到上一级目录
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
- `cp` 复制文件或目录
  - `cp .bashrc .bashrc-backup`
- `mv` 移动文件或目录
  - `mv ../mynote .`把上一级目录的mynote文件移动到当前目录
- `rm` 删除文件
- `rmdir` 删除空文件夹
- `chmod` 改变权限
- `cat` 编辑文件
- `less` 显示文件内容
- `cat /proc/version` 查看Linux内核版本信息
- `lsb_release -a` 查看Linux系统版本信息
- 配置DNS
  
  `vi /etc/resolv.conf` 加入:
  ```
  nameserver 8.8.8.8
  nameserver 8.8.4.4
  ```
- `history` 查看执行过的命令

## 文件所有者和权限

Linux 的文件权限分为三个用户组：`user`, `group` 和 `others`，每组有三个权限：读(`r`)，写(`w`)，执行(`x`)。我们可以利用 `chmod`命令来修改权限，如：`chmod 777 filename.txt`

不用数字的话，可以用字母，`chmod u+x filename.txt` 这个命令就给 user 添加了执行权限。如果要更改文件所属，使用 `chown` 命令，具体可以参见 `man chown`

## SSH

在执行 SSH 命令时，如果太久没有操作，就会自动断开，执行的脚本也会终止，如果不希望这样，有以下几种方式可以避免 SSH 超时：

1.  [关闭 SSH 客户端的超时](https://docs.oseems.com/general/application/ssh/disable-timeout)
2.  用 `nohup` 执行脚本
3.  通过远程屏幕管理(remote sreen management)来保持 terminal sessions

## 磁盘操作

#### 管理分区

`fdisk` 是比较老的分区管理工具，现在几乎已经被 `parted` 取代，因为 `parted` 支持 GUID 分区表以及大于 2TB 的磁盘，所以我们这里使用 `parted`。

`parted -l` 会列出系统中的所有分区。具体的顺序是按照 `/dev` 下的顺序。

`parted` 还可以用来为每个分区设置类型和文件系统，例如
```
parted /dev/xvdX mklabel gpt
parted /dev/xvdX mkpart db ext4 0% 10G
```
如果不加任何参数，`parted` 就会进入交互模式，这里你可以利用 `mkfs` 来格式化一个分区，如 `mkfs.ext4 /dev/xvdX1`

#### 挂载分区

在分区和格式化之后，如果想要使用就需要把分区挂载在某个挂载点上，通常来说会挂载在 `/mnt` 或者 `/mount` 上，可移除的媒介，例如 USB 和 CDROM，会挂载在 `/media` 上，当然，你可以把分区挂载在任何地方，如：
```
mkdir /storage/mountpoint
mount /dev/yourdevice /storage/mountpoint
```
## 配置服务和启动

如果想要开机启动，需要参考应用本身的帮助文档，但是总体来说，服务需要安装在 `/etc/init.d` 中，这使得这些脚本可以在启动时运行。例如，如果 mysql 脚本在 `/etc/init.d` 中，那么可以使用这个命令 `service mysql [start|stop|restart]`。

想要了解更多，请参阅[这里](http://www.tldp.org/HOWTO/HighQuality-Apps-HOWTO/boot.html)

## 系统管理

- `top` 查看进程状态
- `kill` shut down process by id
- `kill -9 PID` 强制结束进程，进程在结束后不能自我清理，会导致系统资源无法正常释放，一般不推荐使用，除非其他办法都无效。
- `kill -15 PID` 建议使用15来结束进程
- `killall`  shut down process by name
- `ps -aux | grep firefox` 查看进程状态
- `su` stands for switch user, if you don’t specify an account, it allows you to switch to the root account, assuming you know the password. Ubuntu locks the root account, so su won’t work for root access.
- `sudo` get administrative privileges but not using root. In order to use this command, the login user should be in the sudo group.

## File System Hierarchy
- `/` The directory holding all of your directories. Called root.
- `/root` The home directory of the root user.
- `/home` Your personal files and personal configurations.
- `/bin, /sbin, and /usr` Your binary applications, or programs. The binaries in `/sbin` are usually require root access to run. The `/usr` directory holds user applications, it has its own `/bin` and `/sbin` folders.
- `/etc` Your systemwide configuration files. These aren’t binaries.
- `/lib` Your systemwide libraries.
- `/opt` Applications installed from outside of your software repository.
- `/tmp` Your temporary files. Files saved here usually disappear between reboots.

## Reference

- http://linuxcommand.org/lc3_learning_the_shell.php
- [The Advanced Bash Scripting Guide](http://tldp.org/LDP/abs/html/)
- [The Art of the Command Line](https://github.com/jlevy/the-art-of-command-line)
- http://billie66.github.io/TLCL/index.html
- https://explainshell.com/

