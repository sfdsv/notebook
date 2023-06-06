# linux下设置conda自动补全

## 1.安装`conda-bash-completion`插件

Github : https://github.com/tartansandal/conda-bash-completion

```bash
conda install -c conda-forge conda-bash-completion
```

关闭终端并重新打开即可。

## 2.禁止自动激活bash环境情况下的设置

在设置`auto_activate_base:false`的时候，禁止自动激活`base`环境(重启终端生效)。

```bash
conda config --set auto_activate_base false
```

此时需要在`~/.bashrc`中添加下列内容自动补全才能生效：

```
if [[ -r /home/hcsci/.env/anaconda3/etc/profile.d/bash_completion.sh ]];then
	. ~/.env/anaconda3/etc/profile.d/bash_completion.sh
else 
	echo "WARNING: could not find conda-bash-completion setup script"
fi
```



