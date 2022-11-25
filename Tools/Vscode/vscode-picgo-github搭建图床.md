1. 创建github仓库,生成token

新建github仓库：仓库名最好不要带奇怪符号，就简单英文单词就好(如pic)：

![20221125145606](https://cdn.jsdelivr.net/gh/sfdsv/pic/img/20221125145606.png)

>仓库属性一定要公开！！！不然网页上不能直接看到。

点击setting->deverloper settings->Personal access tokens->generate new token(classic):

![20221125145820](https://cdn.jsdelivr.net/gh/sfdsv/pic/img/20221125145820.png)

填写名称，勾选repo就好了：

![20221125145912](https://cdn.jsdelivr.net/gh/sfdsv/pic/img/20221125145912.png)

划到底部点击generate token，生成新token：

![20221125145450](https://cdn.jsdelivr.net/gh/sfdsv/pic/img/20221125145450.png)

保存好这个token，一会要用。

2. 安装与配置picgo

在vscode扩展中安装picgo，右键打开扩展设置：

![20221125150503](https://cdn.jsdelivr.net/gh/sfdsv/pic/img/20221125150503.png)

具体设置如下图：

![20221125150617](https://cdn.jsdelivr.net/gh/sfdsv/pic/img/20221125150617.png)

* Current：选择github
* Branch：根据自己需要，github默认为main
* Custom Url:https://cdn.jsdelivr.net/gh/username/仓库名

      CDN加速链接的设置，要替换为你的用户名和仓库名

      jsDeliver是免费CDN，放在Github的资源在国内加载速度比较慢，因此需要使用CDN加速来优化网站打开速度，jsDeliver + Github便是免费且好用的CDN，非常适合博客网站使用。
* Path:可选项，图片路径
* Repo：仓库，github上的用户名/仓库名
* Token：第一步获取到的Token

3. 打开vscode,使用picgo上传图片

    * ctrl+alt+e，打开资源管理器选择图片文件上传；
    * 截图，ctrl+alt+u，从剪切板中上传图片；

> ubuntu20.04使用X11系统，默认没有安装xclip，需要手动安装：
>![20221125151621](https://cdn.jsdelivr.net/gh/sfdsv/pic/img/20221125151621.png)
>![20221125151449](https://cdn.jsdelivr.net/gh/sfdsv/pic/img/20221125151449.png)

上传完成时，vscode右下角可以看到：

![20221125151800](https://cdn.jsdelivr.net/gh/sfdsv/pic/img/20221125151800.png)

打开github，可以观察到已上传成功。且在md文件中已自动生成链接。
