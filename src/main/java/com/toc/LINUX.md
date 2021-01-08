<a name="index">**Index**</a>

<a href="#0">Linux 基本知识</a>  
&emsp;<a href="#1">1. 基本概念</a>  
&emsp;&emsp;<a href="#2">1.1. 标准输⼊和命令参数</a>  
&emsp;&emsp;<a href="#3">1.2. 输入重定向</a>  
&emsp;&emsp;<a href="#4">1.3. 输出重定向</a>  
&emsp;&emsp;<a href="#5">1.4. 管道</a>  
&emsp;&emsp;<a href="#6">1.5. 相关资料</a>  
&emsp;<a href="#7">2. 其他资料</a>  
&emsp;&emsp;<a href="#8">2.1. 单引号与双引号</a>  
# <a name="0">Linux 基本知识</a><a style="float:right;text-decoration:none;" href="#index">[Top]</a>

## <a name="1">基本概念</a><a style="float:right;text-decoration:none;" href="#index">[Top]</a>
### <a name="2">标准输⼊和命令参数</a><a style="float:right;text-decoration:none;" href="#index">[Top]</a>

标准输⼊就是编程语⾔中诸如 scanf 或者 readline 这种命令； ⽽参数是指程序的 main 函数传⼊的 args 字符数组。

以cat命令为例，cat命令的功能是从命令行给出的文件中读取数据，并将这些数据直接送到标准输出。若使用如下命令：
```
$ cat file 
Hello world
Hello world
Bye
```

> 使用标准输入/输出文件存在以下问题：
> - 输入数据从终端输入时，用户费了半天劲输入的数据只能用一次。下次再想用这些数据时就得重新输入。而且在终端上输入时，若输入有误修改起来不是很方便。
> - 为了解决上述问题，Linux系统为输入、输出的传送引入了另外两种机制，即输入/输出重定向和管道。

### <a name="3">输入重定向</a><a style="float:right;text-decoration:none;" href="#index">[Top]</a>
输入重定向是指把命令（或可执行程序）的标准输入重定向到指定的文件中。也就是说，输入可以不来自键盘，而来自一个指定的文件。所以说，输入重定向主要用于改变一个命令的输入源，特别是改变那些需要大量输入的输入源。

命令wc统计指定文件包含的行数、单词数和字符数
> 直接输入wc，wc将等待用户告诉它统计什么，这时shell就好象死了一样，从键盘键入的所有文本都出现在屏幕上，但并没有什么结果，直至按下ctrl+d，wc才将命令结果写在屏幕上。
```
[root@VM-0-16-centos ~]# wc
dsf
sadf123
qwe
      3       3      16


[root@VM-0-16-centos ~]# wc < file 
1 1 7
[root@VM-0-16-centos ~]# wc << file 
> delim
> sdfasdf
> sadff
> delim
> file
 4  4 26
```

### <a name="4">输出重定向</a><a style="float:right;text-decoration:none;" href="#index">[Top]</a>
输出重定向是指把命令（或可执行程序）的标准输出或标准错误输出重新定向到指定文件中。这样，该命令的输出就不显示在屏幕上，而是写入到指定文件中。

```
[root@VM-0-16-centos ~]# echo "sdfadf" > file


```

错误重定向： 和程序的标准输出重定向一样，程序的错误输出也可以重新定向。使用符号2>（或追加符号2>>）表示对错误输出设备重定向。例如下面的命令：
```
$ ls /usr/tmp 2> err.file
```

标准输入输出均重定向：可以使用另一个输出重定向操作符（&>）将标准输出和错误输出同时送到同一文件中。例如：
```
$ ls /usr/tmp &> output.file
```


### <a name="5">管道</a><a style="float:right;text-decoration:none;" href="#index">[Top]</a>
> 将一个程序或命令的输出作为另一个程序或命令的输入，有两种方法，一种是通过一个临时文件将两个命令或程序结合在一起，例如上个例子中的/tmp/dir文件将ls和wc命令联在一起；另一种是Linux所提供的管道功能。

管道可以把一系列命令连接起来，这意味着第一个命令的输出会作为第二个命令的输入通过管道传给第二个命令，第二个命令的输出又会作为第三个命令的输入，以此类推。显示在屏幕上的是管道行中最后一个命令的输出,通过使用管道符“|”来建立一个管道行。

```
[root@VM-0-16-centos ~]# cat file|grep adf
sdfadf
[root@VM-0-16-centos ~]# cat file|grep adf|wc -l
1
```


### <a name="6">相关资料</a><a style="float:right;text-decoration:none;" href="#index">[Top]</a>
[百度百科](https://baike.baidu.com/item/%E6%A0%87%E5%87%86%E8%BE%93%E5%85%A5%E8%BE%93%E5%87%BA/4714867?fr=aladdin#3)



## <a name="7">其他资料</a><a style="float:right;text-decoration:none;" href="#index">[Top]</a>

### <a name="8">单引号与双引号</a><a style="float:right;text-decoration:none;" href="#index">[Top]</a>
1. 单引号、双引号用于用户把带有空格的字符串赋值给变量的分界符。
```
[root@VM-0-16-centos ~]# str="sdf111 sdf"
[root@VM-0-16-centos ~]# echo $str
sdf111 sdf
// 如果没有单引号或双引号，shell会把空格后的字符串解释为命令。
[root@localhost sh]# str=Today is Monday
bash: is: command not found
```
2. 单引号和双引号的区别。单引号告诉shell忽略所有特殊字符，而双引号忽略大多数，但不包括 ` $ \ `

双引号中的`'$'`（参数替换）和``'`'``（命令替换）是例外，所以，两者基本上没有什么区别，除非在内容中遇到了参数替换符`$`和命令替换符```。

3. 反引号 (```) 位于键盘的Tab键的上方，1键的左方。注意与单引号(')位于Enter键的左方的区别。在Linux中起着命令替换的作用。命令替换是指shell能够将一个命令的标准输出插在一个命令行中任何位置。
> 如，shell会执行反引号中的date命令，把结果插入到echo命令显示的内容中。
```
[root@localhost sh]# echo The date is `date`
The date is 2011年 03月 14日 星期一 21:15:43 CST
```