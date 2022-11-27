## 文件注释
### 在已有文件里添加文件注释



### 新建文件时添加文件注释

路径：File->Settings->Editor->File and Code Templates

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
