# ubuntu20.04安装Anaconda

[参考链接](https://blog.csdn.net/JIEJINQUANIL/article/details/107008948)。

## 安装

下载地址：https://repo.anaconda.com/archive/

选择对应的版本下载。

```bash
bash ~/Downloads/Anaconda3-2022.10-Linux-x86_64.sh
```

1. 
![image](https://user-images.githubusercontent.com/73980771/218251115-ece94434-8f93-49b3-836b-12461b6d9582.png)


点击“Enter”查看“许可证协议”。

2. 
![image](https://user-images.githubusercontent.com/73980771/218251119-85987146-ad74-4c5f-9548-667fb4662a37.png)


点击空格翻页。

3. 
![image](https://user-images.githubusercontent.com/73980771/218251123-b6cb7d09-f8d3-4e3e-9a53-c585a9ff9fa0.png)


在“许可证协议”界面将屏幕滚动至底，输入“yes”表示同意许可证协议内容。

4. 
![image](https://user-images.githubusercontent.com/73980771/218251130-58ed065c-3e82-487e-acb5-0913e647565b.png)


“按回车键确认安装路径，按'CTRL-C'取消安装或者指定安装目录。”
可以指定目录，输入要安装的目录即可。

5. 
![image](https://user-images.githubusercontent.com/73980771/218251132-fe21f179-a637-4343-b396-a6a24ecb5e17.png)


初始化，输入“yes”。

6. 
![image](https://user-images.githubusercontent.com/73980771/218251136-fce2c0b9-e65d-44d9-8be8-8fbec1adb8a7.png)


成功，结束！

## 使用


**验证conda已被安装，查看conda版本号：**

```bash
(base) luz@luz-Vostro-3470:~$ conda --version
conda 22.9.0
```

**激活环境：**

```bash
luz@luz-Vostro-3470:~$ conda activate
```

**退出环境至root：**

```bash
(base) luz@luz-Vostro-3470:~$ conda deactivate
```

**切换环境：**

```bash
(base) luz@luz-Vostro-3470:~$ conda activate <env_name>
```

**显示已经安装的包名和版本号：**

```bash
(base) luz@luz-Vostro-3470:~$ conda list
# packages in environment at /home/luz/env/python/anaconda3:
#
# Name                    Version                   Build  Channel
_ipyw_jlab_nb_ext_conf    0.1.0            py39h06a4308_1  
_libgcc_mutex             0.1                        main  
_openmp_mutex             5.1                       1_gnu  
alabaster                 0.7.12             pyhd3eb1b0_0  
anaconda                  2022.10                  py39_0  
anaconda-client           1.11.0           py39h06a4308_0 
...
```

**anaconda交互界面：**

```bash
(base) luz@luz-Vostro-3470:~$ anaconda-navigator
```

**显示已创建环境：**

```bash
$ conda info --envs
$ conda info -e
$ conda env list
```

```bash
(base) luz@luz-Vostro-3470:~$ conda env list
# conda environments:
#
base                  *  /home/luz/env/python/anaconda3
```

**查看conda帮助信息：**

```bash
$ conda --help
$ conda -h
```

**表示卸载名为conda_test环境中的pandas包:**

```bash
$ conda remove --name <env_name> <package_name>
```

```bash
$ conda remove --name conda_test pandas
```

**卸载当前环境中的包：**

```bash
$ conda remove <package_name>
```





