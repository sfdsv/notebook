# cron定时器

## 环境

操作系统：ubuntu20.04

### crontab和crond

crontab是一个命令，常见于Unix和类Unix的操作系统中，用于设置周期性被执行的指令。该命令从标准输入设备读取指令，并将其存放于`crontab`文件中，以供之后读取和执行。该词来源于希腊语chronos(χρόνος)，原意是时间。

crond是crontab的守护进程。cron服务是一个定时执行的服务，可以通过crontab命令添加或编辑需求定时执行的任务。

## cron

cron是一个Linux定时执行工具，可以在无需人工干预的情况下运行作业。在Ubuntu server 下，cron是被默认安装并启动的。


linux定时任务分为两种：

1. 系统自身轮训的任务

cron服务的启动和停止：

1）service cron start  /*启动服务*/

2）service cron stop /*关闭服务*/

3）service cron restart /*重启服务*/

4）service cron reload /*重新载入配置*/

可以通过以下命令查看cron是否在运行（如果在运行，则会返回一个进程ID）：

```bash
pgrep cron
```

`cat /etc/crontab`:

```
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

### 用法

**cron特殊符号**

1. `*`  代表   “每一”

如`20 * * * *` 代表每小时的第20分钟才执行，比如14:20、15:20等，一天就会执行24次。

2. `,` 代表 '"并列"

如`20,40 * * * *`在每小时的第20分钟和第40分钟分别执行一次程序。这样每小时就会执行2次，每天就会执行48次。

3. `-` 代表 "连续"

如`20-40 * * * *` 第20分钟到第40分钟每分钟都会执行1次，【20,40】，每小时执行21次【闭区间】，所以每天执行21*24次

4. `/`  代表“整除”

如`*/2 * * * *` 代表当前分钟数能被2整除的话就执行，等价于0,2,4,6,,,,,,,58

**例1**

需求：每天定时执行脚本，脚本中需要sudo执行的指令。

做法：
1. 执行如下命令添加任务，使用root用户启动命令：

```bash
sudo  crontab -u root -e 
```

提示：第一次使用crontab -e创建一个需要执行的任务表时会想让你选择使用哪一种编辑器，通过输入对应数字进行选择，如下图所示:

添加命令，实现每天8点定时执行脚本的功能（记得脚本文件需要绝对路径）：
```
00 08 * * * xxx.sh
```

可通过以下命令查看运行中的定时任务：
```bash
sudo crontab -l
```


