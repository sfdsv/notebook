# 迁移Gitlab全过程记录

如果不耐烦看步骤，可以直接跳到结果章节。

## 问题1-迁移后,删除原Gitlab仓库或修改仓库路径报错500

查看日志:

```
==> /var/log/gitlab/gitlab-rails/production.log <==
Started DELETE "/luz/test" for 10.5.10.87 at 2023-02-03 04:02:07 +0000
Processing by ProjectsController#destroy as HTML
  Parameters: {"authenticity_token"=>"[FILTERED]", "namespace_id"=>"luz", "id"=>"test"}
Completed 500 Internal Server Error in 16ms (ActiveRecord: 1.5ms | Elasticsearch: 0.0ms | Allocations: 11515)
```

日志显示为权限token问题。

定位问题为备份仓库时忽略了gitlab-secrets.json文件。

备份时输出：

![image](https://user-images.githubusercontent.com/73980771/218413226-2e6141d5-e004-4695-bb54-cf0c912d4e07.png)


解决办法：

复制原仓库gitlab-secrets.json文件到新仓库对应目录。

重启gitlab : `gitlab-ctl restart`

## 问题2-本地备份推送到smb远端

使用Gitlab自动备份在本地后，推送到smb远端服务器。

**正确步骤：**

1. Gitlab宿主机安装smb客户端：

```bash
sudo apt install cifs-utils
```

2.挂载文件夹

```bash
# 创建本地目录
sudo mkdir /mnt/remote

# 映射远程共享文件(匿名登录)
sudo mount -t cifs //10.5.10.95/dashuai_ubuntu2004 /mnt/remote -o guest
```

3.在crontab的备份命令后添加执行脚本：

backup_remote.sh

```shell
#!/bin/bash

backup_path=/data/gitlab/data/backups

filename=`ls -t $backup_path |head -n1|awk '{print $0}'`

dest_path=/mnt/remote/Backup/gitlab/backup

cp $backup_path/$filename $dest_path

find $dest_path -ctime +100 -name "*.tar" -delete
```

3.1.给脚本赋予执行权限：

```bash
chmod +x backup_remote.sh
```

3.2.添加脚本执行命令，先''crontab -e''打开编辑器,修改原来的备份命令为：

```
00 04 * * 1 docker exec -t gitlab gitlab-backup create && sleep 1h && /data/backup_remote.sh
```

3.3.''crontab -l''检查命令是否成功。

**错误步骤：**

安装完smb客户端后，直接将gitlab的备份目录作为挂载目录,测试备份命令时，备份成功，但是备份目录和挂载目录都未发现备份文件。

## 问题3-临时挂载，重启失效

上诉问题2提到的挂载为临时挂载，服务器重启后失效。

**修改方案1:**

考虑开机自动挂载smb远端服务器。因为挂载服务的成功受限于远端服务器是否启动，所以如果远端服务器因为某种原因未启动，该服务器则可能开机启动错误。放弃。

**修改方案2:**

在执行备份到远端服务器之前执行挂载，挂载失败则不执行，只打印错误。

步骤：

1. 修改backup_remote.sh文件。

```shell
#!/bin/bash

TIME=$(date +%Y/%m/%d,%X)

echo "$TIME"

# determine whether the diretory is mounted
if mountpoint -q /mnt/remote;then
echo "has mounted"
else
echo "not mounted"
# mount smb remote server to specified diretory which is pre-created.
# echo error and shell exit when mount faild.
mount -t cifs //10.5.10.95/dashuai_ubuntu2004 /mnt/remote -o guest
if [ $? -ne 0 ]; then
    	echo "mount failed"
    	exit 1
fi
echo "mount succeed!"
fi

backup_path=/data/gitlab/data/backups
dest_path=/mnt/remote/Backup/gitlab/backup

# get filename which the latest file by time order in backup_path.
filename=`ls -t $backup_path |head -n1|awk '{print $0}'`
echo "$filename"

# copy gitlab local backup file to smb remote server directory
cp $backup_path/$filename $dest_path
if [ $? -eq 0 ]; then
    echo "cp succeed"
else
    echo "cp failed"
    exit 1
fi

# delete remote server backup files which created time is over 100 days.
find $dest_path -ctime +100 -name "*.tar" -delete
if [ $? -eq 0 ]; then
    echo "find succeed"
else
    echo "find failed"
    exit 1
fi

echo -e "backup succeed!\n"
```

2. 修改crontab定时任务，备份到远端的日志输出到文件中：

```
00 04 * * 1 docker exec -t gitlab gitlab-backup create && sleep 1h && /data/backup_remote.sh >> /data/backup_remote.log
```

### [问题3.1]如果挂载成功后再挂载，报错''设备Busy''。

```bash
root@dell-Precision-3650-Tower:~# mount -t cifs //10.5.10.95/dashuai_ubuntu2004 /mnt/remote -o guest
mount error(16): Device or resource busy
Refer to the mount.cifs(8) manual page (e.g. man mount.cifs) and kernel log messages (dmesg)
```

解决办法：

先判断目标文件夹是否被挂载，再决定是否挂载。

### [问题3.2]如果挂载成功后，远端服务器断网了或被拔了网线，会发生什么？

#### 情景1

test.sh

```shell
#!/bin/bash

# determine whether the diretory is mounted
if  mountpoint -q /mnt/remote;then
echo "has mounted"
else
echo "not mounted"
# mount smb remote server to specified diretory which is pre-created.
# echo error and shell exit when mount faild.
mount -t cifs //10.5.10.95/dashuai_ubuntu2004 /mnt/remote -o guest
if [ $? -ne 0 ]; then
    	echo "mount failed"
    	exit 1
fi
echo "mount succeed!"
fi

```

在挂载成功的状态下，拔出网线，执行脚本,输出显示：

```
not mounted
mount error: Server abruptly closed the connection.
This can happen if the server does not support the SMB version you are trying to use.
The default SMB version recently changed from SMB1 to SMB2.1 and above. Try mounting with vers=1.0.
mount error(112): Host is down
Refer to the mount.cifs(8) manual page (e.g. man mount.cifs) and kernel log messages (dmesg)
mount failed
```

#### 情景2

test.sh

```shell
#!/bin/bash

backup_path=/home/luz/workspace
dest_path=/mnt/remote/Backup/gitlab/backup

# get filename which the latest file by time order in backup_path.
filename=`ls -t $backup_path |head -n1|awk '{print $0}'`

echo "$filename"

# copy gitlab local backup file to smb remote server directory
cp $backup_path/$filename $dest_path
if [ $? -eq 0 ]; then
    echo "cp succeed"
else
    echo "cp failed"
    exit 1
fi

# delete remote server backup files which created time is over 100 days.
find $dest_path -ctime +100 -name "*.tar" -delete
if [ $? -eq 0 ]; then
    echo "find succeed"
else
    echo "find failed"
    exit 1
fi

```

同样在挂载成功的状态，拔出网线，执行脚本,输出显示：

```
test.sh
cp: 访问 '/mnt/remote/Backup/gitlab/backup' 失败: 主机关闭
cp failed
```

## 结果

backup_remote.sh

```shell
#!/bin/bash

TIME=$(date +%Y/%m/%d,%X)

echo "$TIME"

# determine whether the diretory is mounted
if mountpoint -q /mnt/remote;then
echo "has mounted"
else
echo "not mounted"
# mount smb remote server to specified diretory which is pre-created.
# echo error and shell exit when mount faild.
mount -t cifs //10.5.10.95/dashuai_ubuntu2004 /mnt/remote -o guest
if [ $? -ne 0 ]; then
    	echo "mount failed"
    	exit 1
fi
echo "mount succeed!"
fi

backup_path=/data/gitlab/data/backups
dest_path=/mnt/remote/Backup/gitlab/backup

# get filename which the latest file by time order in backup_path.
filename=`ls -t $backup_path |head -n1|awk '{print $0}'`
echo "$filename"

# copy gitlab local backup file to smb remote server directory
cp $backup_path/$filename $dest_path
if [ $? -eq 0 ]; then
    echo "cp succeed"
else
    echo "cp failed"
    exit 1
fi

# delete remote server backup files which created time is over 100 days.
find $dest_path -ctime +100 -name "*.tar" -delete
if [ $? -eq 0 ]; then
    echo "find succeed"
else
    echo "find failed"
    exit 1
fi

echo -e "backup succeed!\n"
```

crontab定时器：

```
00 04 * * 1 docker exec -t gitlab gitlab-backup create && sleep 1h && /data/backup_remote.sh >> /data/backup_remote.log
```

