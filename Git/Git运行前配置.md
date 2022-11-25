安装完Git后，需要对计算机上的环境信息进行配置。

每台计算机只需要配置一次。程序升级时会保留配置信息。

可以在任何时候通过运行命令来修改它们。

**用户信息**

配置个人的用户名和电子邮件地址：

```bash
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
```

如果用了 `--global` 选项，那么更改的配置文件位于你用户主目录(`$HOME`)下，以后的所有项目都会默认使用这里配置的用户信息。

如果要在某个特定的项目中使用其他名字或者电子邮件地址，只要去掉 `--global` 选项再次配置即可，新的配置保存在当前项目的 `.git/config` 文件里。

**查看配置文件**

检查已有的配置信息，可以使用```git config --list```命令：

```bash
$ git config --list
diff.astextplain.textconv=astextplain
filter.lfs.clean=git-lfs clean -- %f
filter.lfs.smudge=git-lfs smudge -- %f
filter.lfs.process=git-lfs filter-process
filter.lfs.required=true
http.sslbackend=openssl
http.sslcainfo=C:/Program Files/Git/mingw64/ssl/certs/ca-bundle.crt
core.autocrlf=true
core.fscache=true
core.symlinks=false
pull.rebase=false
credential.helper=manager-core
credential.https://dev.azure.com.usehttppath=true
init.defaultbranch=master
credential.https://git.hcsci.com.provider=generic
user.name=sfdsv
user.email=73980771+sfdsv@users.noreply.github.com
filter.lfs.clean=git-lfs clean -- %f
filter.lfs.smudge=git-lfs smudge -- %f
filter.lfs.process=git-lfs filter-process
filter.lfs.required=true
```

或者还可以添加```--show-origin```选项查看所有配置和它们所在的文件：
```bash
$ git config --list --show-origin
file:C:/Program Files/Git/etc/gitconfig diff.astextplain.textconv=astextplain
file:C:/Program Files/Git/etc/gitconfig filter.lfs.clean=git-lfs clean -- %f
file:C:/Program Files/Git/etc/gitconfig filter.lfs.smudge=git-lfs smudge -- %f
file:C:/Program Files/Git/etc/gitconfig filter.lfs.process=git-lfs filter-process
file:C:/Program Files/Git/etc/gitconfig filter.lfs.required=true
file:C:/Program Files/Git/etc/gitconfig http.sslbackend=openssl
file:C:/Program Files/Git/etc/gitconfig http.sslcainfo=C:/Program Files/Git/mingw64/ssl/certs/ca-bundle.crt
file:C:/Program Files/Git/etc/gitconfig core.autocrlf=true
file:C:/Program Files/Git/etc/gitconfig core.fscache=true
file:C:/Program Files/Git/etc/gitconfig core.symlinks=false
file:C:/Program Files/Git/etc/gitconfig pull.rebase=false
file:C:/Program Files/Git/etc/gitconfig credential.helper=manager-core
file:C:/Program Files/Git/etc/gitconfig credential.https://dev.azure.com.usehttppath=true
file:C:/Program Files/Git/etc/gitconfig init.defaultbranch=master
file:C:/Users/luz/.gitconfig    credential.https://git.hcsci.com.provider=generic
file:C:/Users/luz/.gitconfig    user.name=sfdsv
file:C:/Users/luz/.gitconfig    user.email=73980771+sfdsv@users.noreply.github.com
file:C:/Users/luz/.gitconfig    filter.lfs.clean=git-lfs clean -- %f
file:C:/Users/luz/.gitconfig    filter.lfs.smudge=git-lfs smudge -- %f
file:C:/Users/luz/.gitconfig    filter.lfs.process=git-lfs filter-process
file:C:/Users/luz/.gitconfig    filter.lfs.required=true
```

有时候会看到重复的变量名，说明它们来自不同的配置文件（比如 `/etc/gitconfig` 和 `~/.gitconfig`），这种情况下，Git 会使用它找到的每一个变量的最后一个配置。

也可以直接查阅某个环境变量的设定，只要把特定的名字跟在后面即可：

```bash
$ git config user.name
sfdsv
```

