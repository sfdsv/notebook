# close
>// The close built-in function closes a channel, which must be either  
// bidirectional or send-only. It should be executed only by the sender,  
// never the receiver, and has the effect of shutting down the channel after  
// the last sent value is received. After the last value has been received  
// from a closed channel c, any receive from c will succeed without  
// blocking, returning the zero value for the channel element. The form  
//	x, ok := <-c  
// will also set ok to false for a closed channel.  
```func close(c chan<- Type)```

解析：内置函数close关闭一个信道，这个信道必须是双向的或仅发送的。它应该由发送端执行，而不是接收端。从一个已经关闭的信道里接收到最后一个值后，接收操作不会阻塞而是收到对应信道类型的零值。对一个**已经被关闭**且**没有数据**的信道，```x, ok := <-c```形式也将收到ok为false。

下列情况会出错：
1. 关闭仅能发送的信道
```go
var ch = make(<-chan struct{}, 1)
close(ch)
```
2. 发送到或关闭一个已经关闭的信道
```go
var ch = make(chan struct{}, 1)
close(ch)
ch <- struct{}{}

//panic: send on closed channel
```
```go
var ch = make(chan struct{}, 1)
close(ch)
close(ch)

//panic: close of closed channel
```
3 . 关闭nil信道
```go
var ch chan struct{}
close(ch)

//panic: close of nil channel
```

# len

>// The len built-in function returns the length of v, according to its type:  
//	Array: the number of elements in v.  
//	Pointer to array: the number of elements in *v (even if v is nil).  
//	Slice, or map: the number of elements in v; if v is nil, len(v) is zero.   
//	String: the number of bytes in v.  
//	Channel: the number of elements queued (unread) in the channel buffer;    
//	         if v is nil, len(v) is zero.  
// For some arguments, such as a string literal or a simple array expression, the  
// result can be a constant. See the Go language specification's "Length and  
// capacity" section for details.  
```func len(v Type) int```

解析为：内置函数len根据v的类型，返回v的长度：
|v的类型| 结果 |备注|
|--|--|--|
|字符串类型  |按字节表示的字符串长度  | |
|[n]T, *[n]T    |数组长度（== n）  |即使v为nil，也返回n |
|[]T   |分片长度  |如果v是nil，则返回0 |
|map[K]T  |映射长度（定义的键的个数）  |如果v是nil，则返回0 |
|chan T  |在信道缓冲区内排队的元素个数  |如果v是nil，则返回0 |

对于一些参数，例如字符串文本或简单的数组表达式，结果依然是一个常量。这种情况下，v是不求值的。
```go
const AnyCell = 1

var y [][19]string
println(len(y[AnyCell]))
```

# cap
>// The cap built-in function returns the capacity of v, according to its type:   
//	Array: the number of elements in v (same as len(v)).  
//	Pointer to array: the number of elements in *v (same as len(v)).  
//	Slice: the maximum length the slice can reach when resliced;  
//	if v is nil, cap(v) is zero.  
//	Channel: the channel buffer capacity, in units of elements;  
//	if v is nil, cap(v) is zero.  
// For some arguments, such as a simple array expression, the result can be a  
// constant. See the Go language specification's "Length and capacity" section for  
// details.  

```func cap(v Type) int```

解析：内置函数cap根据v的类型，返回v的容量。
|v的类型| 结果 |备注|
|--|--|--|
|[n]T, *[n]T    |数组长度（== n）  |即使v为nil，也返回n |
|[]T   |分配容量  |如果v是nil，则返回0 |
|chan T  | 信道缓冲区容量  |如果v是nil，则返回0 |

对于一些参数，如如简单的数组表达式，结果也是一个常量。跟内置函数len相同。

分片的容量是为其底层数组所分配的空间所对应的元素个数。任何时间都满足如下关系：
```0 <= len(s) <= cap(s)```
