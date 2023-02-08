# cron定时器

## 环境

操作系统：ubuntu20.04

## cron

Linux系统使用cron程序来计划要定期执行的作业。cron程序会在后台运行并检查特殊的称作cron时间表的表，来获得计划执行的作业。

通常，ubuntu安装完成后，默认安装 cron服务，且 cron 服务默认就是自启动的。

可以通过以下命令查看cron是否在运行（如果在运行，则会返回一个进程ID）：

```bash
pgrep cron
```

```bash
luz@luz-Vostro-3470:~$ pgrep cron
652
```

cron服务的启动和停止（会提示输入密码来启动cron.service）:

```bash
# 启动服务
service cron start

# 关闭服务
service cron stop

# 重启服务
service cron restart 
```

### crond

crond是crontab的守护进程。crond 进程每分钟会定期检查是否有要执行的任务，如果有，则会自动执行该任务。

### crontab

crontab是一个命令工具，用来管理定时任务列表，设置周期性被执行的指令。

#### 用法1

`crontab -e`命令添加定时任务。

当``crontab -e`编辑完成之后，一旦保存退出，那么这个定时任务实际就会写入` /var/spool/cron/ `目录中，每个用户的定时任务用自己的用户名进行区分。而且 crontab 命令只要保存就会生效，只要 crond 服务是启动的。

语法：

```bash
crontab [ -u user ] file
```
或

```bash
crontab [ -u user ] { -l | -r | -e }
```

* -u user 是指设定指定 user 的时程表，这个前提是你必须要有其权限(比如说是 root)才能够指定他人的时程表。如果不使用 -u user 的话，就是表示设定自己的时程表。

- -e : 执行文字编辑器来设定时程表。
- -r : 删除目前的时程表
- -l : 列出目前的时程表

注意，这里的 file 指的是命令文件的名字，表示将 file 作为 crontab 的任务列表文件并载入 crontab，若在命令行中未指定文件名，则此命令将接受标准输入（键盘）上键入的命令，并将它们键入 crontab。

**提示**：第一次使用crontab -e创建一个需要执行的任务表时会想让你选择使用哪一种编辑器，通过输入对应数字进行选择:

```bash
luz@luz-Vostro-3470:~$ crontab -e
no crontab for luz - using an empty one

Select an editor.  To change later, run 'select-editor'.
  1. /bin/nano        <---- easiest
  2. /usr/bin/vim.basic
  3. /usr/bin/vim.tiny
  4. /usr/bin/code
  5. /bin/ed

Choose 1-5 [1]:
```

时间特殊符号：

| 特殊符号        | 含义   | 举例说明                                                     |
| --------------- | ------ | ------------------------------------------------------------ |
| `*`  （星号）   | "每一" | `20 * * * *` 代表每小时的第20分钟才执行，比如14:20、15:20等，一天就会执行24次。 |
| `,` （逗号）    | "并列" | `20,40 * * * *`在每小时的第20分钟和第40分钟分别执行一次程序。这样每小时就会执行2次，每天就会执行48次。 |
| `-` （中杠）    | "连续" | `20-40 * * * *` 第20分钟到第40分钟每分钟都会执行1次，【20,40】，每小时执行21次【闭区间】，所以每天执行21*24次 |
| `/`  （正斜线） | "整除" | `*/2 * * * *` 代表当前分钟数能被2整除的话就执行，等价于0,2,4,6,,,,,,,58 |

**[例1]**每天定时执行脚本，脚本中需要sudo执行的指令。

步骤：
1. 执行如下命令添加任务，使用root用户启动命令：

```bash
sudo crontab -u root -e 
```

2. 添加命令，实现每天8点定时执行脚本的功能（**记得脚本文件需要绝对路径**）：

```
00 08 * * * xxx.sh
```

3. 可通过以下命令查看运行中的定时任务：

```bash
sudo crontab -l
```

#### 用法2

直接编辑配置文件`/etc/crontab`。该配置文件中的定时任务需要定义用户身份。

```
luz@luz-Vostro-3470:~$ cat /etc/crontab 
# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name command to be executed
17 *	* * *	root    cd / && run-parts --report /etc/cron.hourly
25 6	* * *	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6	* * 7	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6	1 * *	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
#
```

run-parts是一个Shell脚本，保存在`/usr/bin/`中，作用就是把其后面跟随的目录中的所有可执行文件依次执行。

根据`/etc/crontab`脚本中预置的定时任务，也可以把需要定时执行的工作写入脚本程序，并赋予执行权限，然后直接把脚本赋值到` /etc/cron.{daily，weekly，monthly} `目录中的任意一个。

具体的执行时间如上。