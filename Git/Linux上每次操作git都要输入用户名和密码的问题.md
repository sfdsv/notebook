配置缓存密码:
```bash
git config --global credential.helper store
```

使用此命令后还会需要输入一次用户名和密码,但是下一次就不需要了。

清除缓存密码:
```bash
git config --local --unset credential.helper
git config --global --unset credential.helper
git config --system --unset credential.helper
```


>使用 git + https 操作项目，直接提示403，并没有让输入用户名密码, 可使用此办法操作:
>
>[git clone时，报403错误，完美解决方案](https://www.cnblogs.com/jarvisjin/p/5915419.html)
