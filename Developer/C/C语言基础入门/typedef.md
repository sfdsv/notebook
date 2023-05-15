# typedef

关键字typedef可以为类型取一个新的名字

```c
typedef unsigned char BYTE;
```

## `typedef `vs `#define`

* `typedef`仅限于为类型定义符号名称，`#define`不仅可以为类型定义别名，也能为数值定义别名
* `typedef`是由编译器执行解释的，`#define`语句是由预编译器进行处理的。