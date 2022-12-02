## 配置用户信息

```bash
$ git config --global user.name "username"
$ git config --global user.email email
```

## 初始化本地仓库

使用当前目录作为Git仓库：

```bash
$ git init 
```

该命令执行后将在当前目录生成一个名为```.git```的子目录，该目录包含了资源的所有元数据，其他的项目目录保持不变（不像 SVN 会在每个子目录生成 .svn 目录，Git 只在仓库的根目录生成 .git 目录）

使用指定目录作为仓库：

```bash
$ git init myrepo
```

同样的，初始化后，在```myrepo```目录下会生成一个```.git```的子目录，所有Git需要的数据和资源都在这个目录中。

### 克隆仓库

克隆项目到当前目录：

```bash
$ git clone <repo>
```

自定义本地仓库的名字：

```bash
$ git clone <url> <directory>
```

**参数说明**：

**repo** : Git仓库路径

**directory**  :  该仓库克隆后的目录名字。
