## 编码问题

> 编码问题是程序员在开发过程中经常遇到的问题。在计算机中，所有文字都是由0和1组成的字符串。
>
> 为了能让汉字正常显示在屏幕上，我们需要做以下两件事情：
>
> 1、给所有的汉字一个独一无二的数字编号，做一个数字编号到汉字的mapping关系（即字符集）
>
>  2、把这个数字编号能用0和1表示出来¹。
>
> 常见的编码方式有ASCII、GB2312、GBK、GB18030、UTF8、ANSI、Latin1等¹。不同的编码方式有不同的特点和适用范围。例如，ASCII编码只能表示128个字符，适用于只有英文字母场景下的编码需求²。而GB2312、GBK和GB18030都是中国国家标准，可以表示更多的汉字¹。
>
> 你想了解哪种编码方式呢？
>
> 源: 与必应的对话， 2023/4/13(1) 程序员必备：彻底弄懂常见的7种中文字符编码 - 知乎. https://zhuanlan.zhihu.com/p/46216008 访问时间 2023/4/13.
> (2) 一文彻底搞懂Unicode编码问题 - 知乎 - 知乎专栏. https://zhuanlan.zhihu.com/p/370601172 访问时间 2023/4/13.
> (3) Java中的编码问题_谢公子的博客-CSDN博客. https://blog.csdn.net/qq_36119192/article/details/107328066 访问时间 2023/4/13.
> (4) 作为2020年的程序员，你还在为编码问题困惑？ - 知乎. https://zhuanlan.zhihu.com/p/256438773 访问时间 2023/4/13

## UTF-8编码

> UTF-8（8-bit Unicode Transformation Format）是一种针对Unicode的可变长度字符编码，又称万国码。它由Ken Thompson于1992年创建，现在已经标准化为RFC 3629。UTF-8用1到4个字节编码UNICODE字符²。
>
> UTF-8的编码规则如下：1、对于单字节的符号，字节的第一位设为0，后面7位为这个符号的Unicode码。因此对于英语字母，UTF-8编码和ASCII码是相同的。所以UTF-8能兼容ASCII编码，这也是互联网普遍采用UTF-8的原因之一。2、对于n字节的符号（n>1），第一个字节的前n位都设为1，第n+1位设为0，后面字节的前两位一律设为10。剩下的没有提及的二进制位，全部为这个符号的Unicode码¹。
>
> 你还想了解什么呢？
>
> 源: 与必应的对话， 2023/4/13(1) 在线UTF-8编码解码 - 码工具 - MaTools. https://www.matools.com/code-convert-utf8 访问时间 2023/4/13.
> (2) Unicode、UTF-8、UTF-16 终于懂了 - 知乎 - 知乎专栏. https://bing.com/search?q=UTF-8%e7%bc%96%e7%a0%81 访问时间 2023/4/13.
> (3) HTML UTF-8 Reference - W3School. https://www.w3schools.com/charsets/ref_html_utf8.asp 访问时间 2023/4/13.
> (4) 百度百科-验证. https://baike.baidu.com/item/UTF-8/481798 访问时间 2023/4/13.

## ASCII编码