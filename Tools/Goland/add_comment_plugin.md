## 安装插件
路径：：File -> Settings -> Plugins，在插件市场里搜索Goanno插件，点击INSTALL。

## 使用方式
  1. 快捷键Ctrl+Meta+/
  2. 鼠标右键 -> Generate -> Goanno
  3. 快捷键Ctrl+Alt+/
 
 > Meta键就是Windows键，需要配置才可以生效：
 > 打开Help -> Edit Custom Properties， 如果是第⼀次打开，会提⽰是否创建这个文件，点击CREATE, 在文件⾥添加如下代码：
 > ```
 > keymap.windows.as.meta=true
 > ```
 > 重启IDE就好了。
 
 ## 编辑模板
 路径:Tools -> Goanno Setting,在Normal Method栏下的编辑框里添加如下代码：

 ```
// ${function_name} ${todo}
// @Author: luz 
// @Date: ${date}
// @Receiver ${receiver_name_type}
// @Param ${param_name}
// @Return ${ret_name_type}
 ```
 
其他栏按需求修改。

Interface：
```
//  ${interface_name} ${todo}
```

Interface Method:
 ```
//  ${function_name}
//  @Description: ${todo}
//  @receiver ${receiver}
//  @param ${params}
//  @return ${return_types}
```

Struct:
```
// ${struct_name} ${todo}
```

Struct Field:
```
//  ${struct_field_name} ${todo}
```

Package:
```
```

Other:
```
//  ${date} ${todo}
```

**插件参数类型**
| Args                  | Desc                                                    |
| --------------------- | ------------------------------------------------------- |
| ${todo}               | Blank Placeholder                                       |
| ${receiver}           | function receiver name or type                          |
| ${params}             | function params name or type                            |
| ${return_types}       | function output name or type                            |
| ${function_name}      | function name                                           |
| ${date}               | yyyy-MM-dd HH:mm:ss                                     |
| ${todo}               | Blank Placeholder                                       |
| ${receiver}           | function receiver name or type                          |
| ${params}             | function params name or type                            |
| ${return_types}       | function output name or type                            |
| ${function_name}      | function name                                           |
| ${param_name}         | function params name                                    |
| ${param_type}         | function params type                                    |
| ${param_name_type}    | function params name and type                           |
| ${receiver_name}      | function receiver name                                  |
| ${receiver_type}      | function receiver type                                  |
| ${receiver_name_type} | function receiver name and type                         |
| ${ret}                | function output name or type, equals to ${return_types} |
| ${ret_name}           | function output name                                    |
| ${ret_type}           | function output type                                    |
| ${ret_name_type}      | return name and type                                    |
| ${interface_name}     | Name of Interface                                       |
| ${struct_name}        | Name of Struct                                          |
| ${struct_field_name}  | Name of Struct Field                                    |
| ${package_name}       | Name of Package                                         |
