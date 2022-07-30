# python



## 字符串与数字

可以用单引号或者双引号

字符串可以与数字相乘，但不能直接相加

字符串索引

```
name = "abcdefg"
name[1,4]   #下标1到3的子串  "bcd"
name[4:]    #下标4到字符串结束  "efg"
name[ :2]   #下标0到2		"abc"
```

`<string>.find(<string>)`   查找字符串某一子串第一次出现的位置

`<string>.find(<string>,xxx)`   查找字符串某一子串从xxx开始第一次出现的位置



```
7 // 4   ==  1    #表示向下取整除
```

`**  表示乘方运算`



Python拥有常见编程功能的内建支持，例如文本操作、显示图形以及互联网通信。导入语句

```
>>> from urllib.request import urlopen
```

为访问互联网上的数据加载功能。特别是，它提供了叫做`urlopen`的函数，可以访问到统一资源定位器（URL）处的内容，它是互联网上的某个位置。



## 函数



lambda表达式  符号是 **λ**   常用来表示匿名函数

```
λ a,x,b.ax+b     
```

```
lambda a, x, b: a * x + b
```



print()函数返回一个空值`None`

```
>>>print(print(1),print(2))
1
2
None None
```

函数的参数也可以是函数，参数里的函数先执行，然后它的返回值作为参数传给调用它的函数  注意执行顺序

副作用：就是一个函数除了返回值之外它还做了什么其他事，比如打印、改变值之类的



```def incr(n):
def incr(n):
	def f(x):
		return n + x
	return f
	
incr(5)(6)
```

函数体里可以再定义函数，还可以返回这个函数`incr(5)`等价于`f`但是传入了`n=5`



作为准则，用于函数体的大多数数据值应该表示为具名参数的默认值，这样便于查看，以及被函数调用者修改。一些值永远不会改变，就像基本常数`k_b`，应该定义在全局帧中。



hw01  Q5:

将函数作为**参数**传给另一个函数过程，先执行该函数，然后把返回值作为参数   **一定要先执行该函数**

将函数名作为参数传给另一个函数，不用先执行



函数名`f`表示函数引用，指的就是一个函数  （可以理解为函数的地址或者这个函数本身

`f()`表示一种函数调用，可以代表函数的返回值  （指的是函数的返回值，一种调用的过程、结果）



逻辑短路

`A and B`  如果A是假的 就返回A  否则返回B

`A or B` 如果A是真的，就返回A， 否则返回B



## 测试

**断言。**程序员使用`assert`语句来验证预期，例如测试函数的输出。`assert`语句在布尔上下文中只有一个表达式，后面是带引号的一行文本（单引号或双引号都可以，但是要一致）如果表达式求值为假，它就会显示。

```
>>> assert fib(8) == 13, 'The 8th Fibonacci number should be 13'
```

当被断言的表达式求值为真时，断言语句的执行没有任何效果。当它是假时，`asset`会造成**执行中断**。

为`fib`编写的`test`函数测试了几个参数，包含`n`的极限值：

```
>>> def fib_test():        assert fib(2) == 1, 'The 2nd Fibonacci number should be 1'        assert fib(3) == 1, 'The 3nd Fibonacci number should be 1'        assert fib(50) == 7778742049, 'Error at the 50th Fibonacci number'
```

在文件中而不是直接在解释器中编写 Python 时，测试可以写在同一个文件，或者后缀为`_test.py`的相邻文件中。

**Doctest。**Python 提供了一个便利的方法，将简单的测试直接写到函数的文档字符串内。文档字符串的第一行应该包含单行的函数描述，后面是一个空行。参数和行为的详细描述可以跟随在后面。此外，文档字符串可以包含调用该函数的简单交互式会话：

```
>>> def sum_naturals(n):        """Return the sum of the first n natural numbers        >>> sum_naturals(10)        55        >>> sum_naturals(100)        5050        """        total, k = 0, 1        while k <= n:          total, k = total + k, k + 1        return total
```

之后，可以使用[ doctest 模块](http://docs.python.org/py3k/library/doctest.html)来验证交互。下面的`globals`函数返回全局变量的表示，解释器需要它来求解表达式。

```
>>> from doctest import run_docstring_examples>>> run_docstring_examples(sum_naturals, globals())
```

在文件中编写 Python 时，可以通过以下面的命令行选项启动 Python 来运行一个文档中的所有 doctest。

```
python3 -m doctest <python_source_file>
```

高效测试的关键是在实现新的函数之后（甚至是之前）立即编写（以及执行）测试。只调用一个函数的测试叫做单元测试。详尽的单元测试是良好程序设计的标志。



Linux换行却不执行命令

只需要在本行输入 反斜杠 \  然后回车即进入下一行且不运行命令



## 抽象&高阶函数

### 函数作为形参

就是尽可能把可以重复的代码，或者实现了某一小功能的代码抽出来另写一个函数

函数作为参数可以实现这一功能，可以做到把一个大函数分割成一堆小函数的组合

```python
def summation(N,term):
    k = 1
    sum = 0
    while k <= N:
        sum += term(k)
        k+=1
    return sum

def term(k):
    return k*k

summation就可以计算 term(k)(k从1到N) 的求和  
```

函数作为传参的话之后，调用`summation`，通过改变参数就可以实现许多**不同**的功能，不作为传参，只是在函数体内调用的话还是只能实现一个功能

也可以使用`lambda`表达式简化

```python
不用另外定义term函数，直接用lambda
summation(5,lambda x:x*x)   
调用的时候会把lambda x:x*x  传给term     
```

lambda只能替代这样的函数 -> 有且仅有一个返回语句的函数



### 函数作为返回值

![image-20220729163014288](D:\Typora笔记\图片\image-20220729163014288-16590834182191.png)

注意函数名后面加不加括号的区别

因为`add_func()`的返回值是一个函数名，所以用`h`来接收这个函数名（所以h就是个函数名）

> 此前我们已经知道了变量可以用来接收值，也可以用来接收函数，定义函数的过程(def xxx)就是给变量绑定函数的过程



![image-20220729164648225](D:\Typora笔记\图片\image-20220729164648225-16590844097123.png)

过程详解：首先在全局`frame`定义了两个函数`combine_func`和`add`，然后执行`add_func = combine_funcs(add)`定义`add_func`接收，执行函数

执行函数过程：另开一个`frame`为`f1`，我们找到`combine_funcs(op)`发现执行这个函数发生了什么呢？定义`op`为`add(x,y)`，定义函数`combined(f,g)`，然后返回值`combined`给全局`frame`中的`add_func`

综上：`add_func`就是`combined`

其实就是`combine_funcs(add)`压根没执行什么东西，就是在自己的`frame`定义了两个函数，然后把其中的一个函数返回到上层

> frame 可以理解为作用域或者栈帧或者一块空间，每个frame都不一样，但子frame可以向上索取，也可以替换父frame的元素
>
> 上文所说函数可以理解为就是存在内存中的一个代码块
>
> 定义函数就是将代码块写进frame

![image-20220729170058759](D:\Typora笔记\图片\image-20220729170058759-16590852600395.png)



关注最后一句

在全局`frame`中定义h用来接收，然后执行`add_func(sin,cos)`我们已经知道`add_func`就是`combined`所以关注`combined`的执行过程

另开一个`frame``f2`，定义了`f`函数为`sin`,定义了`g`函数为`cos`定义了`val`函数，返回值`val`给全局`frame`中的h,

h就是`val`函数



![image-20220729170608744](D:\Typora笔记\图片\image-20220729170608744-16590855716477.png)



执行`h(-5)`就是执行`val(-5)`

另开`f3`,定义x为-5，当前`frame`中找不到`op() f() g()`，所以向上找，发现`op()是add() f()是sin() g()是cos()`

所以执行`add(sin(-5),cos(-5))`



# hog项目

[nonlocal](https://www.cnblogs.com/liyang93/p/6669874.html)

注意python中的整除是`//`  `/`默认除到小数

python的函数甚至可以返回多个值

Q5查了半天发现是Q4写错了，评测器还没评测出来  

Q5用了递归的方法

































