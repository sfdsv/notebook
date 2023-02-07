# ubuntu20.04安装配置git

## 安装

```bash
sudo apt install git
```

## 初始配置

```
git config --global user.name "sfdsv"
git config --global user.email 2640961343@qq.com
```

## 提交上传到Github

上传代码时报错鉴权失败：
```
luz@luz-R720-15IKBN:~/workspace/notebook$ git push origin main
Username for 'https://github.com': sfdsv
Password for 'https://sfdsv@github.com': 
remote: Support for password authentication was removed on August 13, 2021.
remote: Please see https://docs.github.com/en/get-started/getting-started-with-git/about-remote-repositories#cloning-with-https-urls for information on currently recommended modes of authentication.
fatal: 'https://github.com/sfdsv/notebook.git/' 鉴权失败
```
解决办法：
在github上获取token（参考[这篇文章](../Vscode/vscode-picgo-github%E6%90%AD%E5%BB%BA%E5%9B%BE%E5%BA%8A.md)中的开头部）后，执行如下操作：
```
luz@luz-R720-15IKBN:~/workspace/notebook$ git remote -v
origin	https://github.com/sfdsv/notebook.git (fetch)
origin	https://github.com/sfdsv/notebook.git (push)
luz@luz-R720-15IKBN:~/workspace/notebook$ git remote remove origin
luz@luz-R720-15IKBN:~/workspace/notebook$ git remote add origin https://获取的token@github.com/用户名/notebook.git
luz@luz-R720-15IKBN:~/workspace/notebook$ git push origin main
枚举对象中: 23, 完成.
对象计数中: 100% (23/23), 完成.
使用 4 个线程进行压缩
压缩对象中: 100% (19/19), 完成.
写入对象中: 100% (19/19), 6.33 KiB | 6.33 MiB/s, 完成.
总共 19（差异 4），复用 9（差异 0），包复用 0
remote: Resolving deltas: 100% (4/4), completed with 4 local objects.
To https://github.com/sfdsv/notebook.git
   59189c5..7ce966e  main -> main
```