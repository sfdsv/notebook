# go语言中的切片清零

切片清零的方式列举如下：

```go
func main() {
   //方法一：
   ids1 := []int{1, 2, 3, 4, 5}
   fmt.Println(ids1)
   ids1 = ids1[0:0]
   fmt.Println(ids1)

   //方法二：
   ids2 := []int{1, 2, 3, 4, 5, 6}
   fmt.Println(ids2)
   ids2 = []int{}
   fmt.Println(ids2)

   //方法三：
   ids3 := []int{1, 2, 3, 4, 5, 6, 7}
   fmt.Println(ids3)
   ids3 = nil
   fmt.Println(ids3)

   //方法四：
   ids4 := []int{1, 2, 3, 4, 5, 6, 7, 8}
   fmt.Println(ids4)
   ids4 = ids4[0:0:0]
   fmt.Println(ids4)
   
   //方法五：
   ids5 := []int{1, 2, 3, 4, 5, 6, 7, 8, 9}
   fmt.Println(ids5)
   ids5 = make([]int, 0)
   fmt.Println(ids5)
}
```


-----

### 看一个例子

现有一个例子，看看会输出什么：

```go
func main() {

	var x []int
	var y [][]int
	for i := 0; i < 5; i++ {
        
		x = x[0:0]//此处要在后面替换为其它清零方式
        
		x = append(x, i)
		fmt.Println("x=", x)

		y = append(y, x)
		fmt.Println("y=", y)

	}

}
```

输出结果如下：

```
x= [0]
y= [[0]]
x= [1]
y= [[1] [1]]
x= [2]
y= [[2] [2] [2]]
x= [3]
y= [[3] [3] [3] [3]]
x= [4]
y= [[4] [4] [4] [4] [4]]

```
不符合预期  

但是采用以下几种清零方式会得到期待正确的结果:
```
	//x = nil
        //x = []int{}
	//x = x[0:0:0]
	//x = *new([]int)

y= [[0] [1] [2] [3] [4]]
```

**只有这种[0:0]的方式产生意料之外的结果!!!!**

#### 分析

我们首先需要了解以下其中的原理：

**切片的底层源码**：

```go
type slice struce{
    array  unsafe.Pointer //指针，指向底层数组
    len		int				
    cap		int
}
```

**切片的存储**

![image](https://user-images.githubusercontent.com/73980771/206098216-c4c0a77a-5a45-4ab5-9dea-6362df5a3a9f.png)

> 首先，在go中，切片类型的变量实际上存放的是一个地址，该地址即为其引用的底层数组的第一个元素的地址，也可以说是这个数组的地址。
>
> 创建一个名为s的切片，变量s储存在栈区，其地址为0x000050420，而其值并不是数组[1, 2, 3]，而是存放的数组[1, 2, 3]的地址。该数组存放在堆区，地址为0x000074080(第一个元素的地址，后面开辟了连续的地址空间存放后续元素)。
> 
> 来自：[go语言关于切片类型内存地址的理解](https://blog.csdn.net/why502b/article/details/92017168)

**对slice进行切片**

可以从slice中继续切片生成一个新的slice，这样能实现slice的缩减。

对slice切片的语法为：

```go
slice[A:B]
slice[A:B:C]
```
其中A表示从slice的第几个元素开始切，B控制切片的长度(B-A)，C控制切片的容量(C-A)，如果没有给定C，则表示切到底层数组的最尾部。注意截取时“左闭右开”，即从索引A开始截取，截取到索引B为止，但不包括索引B的元素。

**一图了解`slice[i:j]`和`append`做了什么**：

```go
//创建一个整型切片
//其长度和容量都是5个元素
slice:=[]int{10,20,30,40,50}

//创建一个新切片
//其长度为j-i=2个元素，容量为len(slice)-i=4个元素
newSlice:=slice[1:3]

//使用原有容量来分配一个新元素
//将新元素赋值为60
newSlice=append(newSlice,60)
```
![append操作之后的底层数组](https://user-images.githubusercontent.com/73980771/206098348-554131fe-1a32-4c72-975c-a895494704f6.png)

通过上图例子，我们可以发现：
  * `[i:j]`操作切片时，新切片的长度为`j-i=2`，容量为`len(slice)-i=4`。
  * append时，因为newSlice在底层数组里还有额外的容量可用，append操作将可用的元素合并到切片的长度，并对其进行赋值。由于和原始的slice共享同一个底层数组，slice的索引为3的元素的值也被改动了。

**最后来分析`x=x[0:0]`和`x=append(x,i)`**

再来分析例子中的`x[0:0]`操作，可以发现新切片的长度为0,容量为1，当append时，新切片在底层数组里还有1的容量可用，所以直接对其进行赋值，而不用生成新的底层数组。

同时，根据输出结果，可以发现`y=append(y,x)`时，不是append的当时的元素，而是切片的底层数组的地址。

所以破案了，**就是因为上述操作中x的底层数组的地址一直没有变化，最后打印`变量y`时值全是`变量x`的底层数组的当前值。**

最后，我们打印变量x和变量y的底层数组的地址来验证一下：

```go
func main() {

	var x []int
	var y [][]int
	for i := 0; i < 5; i++ {

		//x = nil
		x = []int{}
		//x = x[0:0]
		//x = x[0:0:0]
		//x = *new([]int)
                //x = make([]int, 0)

		fmt.Printf("append之前：x的值=%v,x内存放的的底层数组的地址=%p\n", x, x)

		x = append(x, i)
		fmt.Printf("append之后：x的值=%v,x内存放的的底层数组的地址=%p\n", x, x)
		//fmt.Println("x=", x)

		y = append(y, x)
		fmt.Printf("y的值=%v,y内存放的的底层数组的地址=%p\n", y, y)
		//fmt.Println("y=", y)
		fmt.Println()
	}

}
```

输出结果：

```
append之前：x的值=[],x内存放的的底层数组的地址=0x0
append之后：x的值=[0],x内存放的的底层数组的地址=0xc00001e0d0
y的值=[[0]],y内存放的的底层数组的地址=0xc00000c060

append之前：x的值=[],x内存放的的底层数组的地址=0xc00001e0d0
append之后：x的值=[1],x内存放的的底层数组的地址=0xc00001e0d0
y的值=[[1] [1]],y内存放的的底层数组的地址=0xc000066150

append之前：x的值=[],x内存放的的底层数组的地址=0xc00001e0d0
append之后：x的值=[2],x内存放的的底层数组的地址=0xc00001e0d0
y的值=[[2] [2] [2]],y内存放的的底层数组的地址=0xc000052180

append之前：x的值=[],x内存放的的底层数组的地址=0xc00001e0d0
append之后：x的值=[3],x内存放的的底层数组的地址=0xc00001e0d0
y的值=[[3] [3] [3] [3]],y内存放的的底层数组的地址=0xc000052180

append之前：x的值=[],x内存放的的底层数组的地址=0xc00001e0d0
append之后：x的值=[4],x内存放的的底层数组的地址=0xc00001e0d0
y的值=[[4] [4] [4] [4] [4]],y内存放的的底层数组的地址=0xc00007c000
```

**果然发现，使用`x=x[0:0]`清零方式时的底层数组的地址一直没有变化。而其它方式全都变化了的。!!!!**

### 结论

在一下几种切片清零方式中,不要选用`x=x[0:0]`  
可以采取以下:

		//x = nil
		//x = []int{}
		//x = x[0:0:0]
		//x = *new([]int)
                //x = make([]int, 0)

