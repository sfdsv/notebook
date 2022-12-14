## 文件注释
### 在已有文件里添加文件注释

路径：File -> Settings -> Editor -> Live Templates

![image](https://user-images.githubusercontent.com/73980771/204125467-d4729683-82f2-40e0-a732-c554f3078118.png)

在Go模块里，点击右侧加号，选择Live Template，在下方Abbreviation输入快捷语如cmt，Description输入File Comment， 下⽅text文本⾥，输入如下代码：

```
/**************************************************
*FileName：$fileName$
*Description：*
*History：*
*Author: $Author$ 
*Data: $DATE$
**************************************************/
```

点击EDIT VARIABLES,在模板变量⾥填入如下选项：

![image](https://user-images.githubusercontent.com/73980771/204125566-211956a7-e70a-4b6e-9c88-f2c27c2fb30f.png)

重要！！！在下⽅define处（我这⾥是change），选择Go的File（选择作⽤域的意思）。并点击APPLY、OK即可。

![image](https://user-images.githubusercontent.com/73980771/204125581-ee5679af-eeb8-479f-9e6f-e8af5fc1ce91.png)

使⽤⽅式：鼠标光标定位在文件首行，输入cmt，并回⻋。 

### 新建文件时添加文件注释

路径：File -> Settings -> Editor -> File and Code Templates

在includes下方点击加号，编辑文件名，在下方编辑框添加模板。

```
/**************************************************
*FileName：${FILE_NAME}
*Description：*
*History：*
*Author: luz
*Data: ${DATE}
**************************************************/

package ${GO_PACKAGE_NAME}
```

![image](https://user-images.githubusercontent.com/73980771/204125218-9d183527-1687-4ac2-8135-185494fe4aa6.png)

切换到File下，选择Go File，在右侧编辑框里添加代码如下：

```
#parse("head.go")
```

![image](https://user-images.githubusercontent.com/73980771/204125297-2fcd2643-9cff-42a5-a482-69401cdbcc12.png)

点击右下角APPLY及OK，新建test.go文件效果如下：

![image](https://user-images.githubusercontent.com/73980771/204125388-6ff18c77-79f2-48fe-a349-586ea89dacc2.png)
