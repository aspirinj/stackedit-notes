
## Scala 介绍
Scala是一门现代的多范式编程语言，平滑地集成了面向对象和函数式语言的特性，旨在以简练、优雅的方式来表达常用编程模式。Scala的设计吸收借鉴了许多种编程语言的思想，只有很少量特点是Scala自己独有的。Scala语言的名称来自于“可伸展的语言”，从写个小脚本到建立个大系统的编程任务均可胜任。Scala运行于Java平台（JVM，Java 虚拟机）上，并兼容现有的Java程序，Scala代码可以调用Java方法，访问Java字段，继承Java类和实现Java接口。在面向对象方面，Scala是一门非常纯粹的面向对象编程语言，也就是说，在Scala中，每个值都是对象，每个操作都是方法调用。  
  
Spark的设计目的之一就是使程序编写更快更容易，这也是Spark选择Scala的原因所在。总体而言，Scala具有以下突出的优点：  
* Scala具备强大的并发性，支持函数式编程，可以更好地支持分布式系统；  
* Scala语法简洁，能提供优雅的API；  
* Scala兼容Java，运行速度快，且能融合到Hadoop生态圈中。

实际上，AMP实验室的大部分核心产品都是使用Scala开发的。Scala近年来也吸引了不少开发者的眼球，例如，知名社交网站Twitter已将代码从Ruby转到了Scala。  

Scala是Spark的主要编程语言，但Spark还支持Java、Python、R作为编程语言，因此，若仅仅是编写Spark程序，并非一定要用Scala。Scala的优势是提供了REPL（Read-Eval-Print Loop，交互式解释器），因此，在Spark Shell中可进行交互式编程（即表达式计算完成就会输出结果，而不必等到整个程序运行完毕，因此可即时查看中间结果，并对程序进行修改），这样可以在很大程度上提升开发效率。现在的计算机都是多核CPU，想充分利用其多核处理，需要写可并行计算的代码。而函数式编程在并行操作性有着天生的优势，函数式编程没有可变变量，就不会有内存共享的问题。

Scala有两种类型的变量，一种是val，是不可变的，在声明时就必须被初始化，而且初始化以后就不能再赋值；另一种是var，是可变的，声明的时候需要进行初始化，初始化以后还可以再次对其赋值。  


## val变量
```scala
val myStr =  "Hello World!"
```
`myStr`变量的类型是`String`类型，Scala具有“类型推断”能力，可以自动推断出变量的类型。当然，我们也可以显式声明变量的类型：
```scala
val myStr2 :  String  =  "Hello World!"
```

需要说明的是，上面的`String`类型全称是`java.lang.String`，也就是说，Scala的字符串是由Java的`String`类来实现的，因此，我们也可以使用`java.lang.String`来声明，具体如下：
```scala
scala> val myStr3 : java.lang.String  =  "Hello World!"

```

但是，为什么可以不用`java.lang.String`，而只需要使用`String`就可以声明变量呢？这是因为，在每个应用程序中，Scala都会自动添加一些引用，这样，就相当于在每个程序源文件的顶端都增加了一行下面的代码：
```java
import java.lang._ //java.lang包里面所有的东西
```

上面已经声明了一个String类型的不可变的变量，下面我们可以使用该变量，比如要打印出来：

```scala
println(myStr)
// Hello  World!
```

因为myStr是val变量，因此，一旦初始化以后，就不能再次赋值，所以，下面我们执行的再次赋值操作会报错：
```scala
val myStr =  "Hello Scala!"
<console>:27: error: reassignment to val
myStr =  "Hello Scala!"
```

## var变量

如果一些变量，需要在初始化以后还要不断修改它的值（比如商品价格），则需要声明为var变量。  
下面我们把myPrice声明为var变量，并且在声明的时候需要进行初始化：

```scala
var myPrice :  Double  =  9.9
myPrice:  Double  =  9.9
```
```scala
myPrice =  10.6
myPrice:  Double  =  10.6
```

## 小技巧：如何在Scala解释器中输入多行代码

在Scala解释器中，当在命令提示符后面输入一个表达式并且按回车以后，代码就会被执行并显示出结果，比如下面我们输入一行表达式并回车：
```scala
2*9+4
res1:  Int  =  22
```

这是输入单行代码的情形，但是，有时候，我们需要在命令提示符后面输入多行代码，这该如何实现呢？怎么才能让Scala解释器意识到你要输入多行代码呢？  
在Java中，每个语句都是以英文的分号结束，但是，在Scala中，可以不用分号。当然，如果你想把多条语句全部写在一行上面，这时还是需要使用分号来隔开各个语句的。  
通常而言，只要Scala解释器推断出你的代码还没有结束，应该延续到下一行，解释器就会在下一行显示一个竖线“|”，你可以继续输入剩余的代码，比如，我们要输入表达式val myStr4 = “Hello World!”，我们只在命令提示符后面输入“val myStr4 = ”然后就回车，显然，这个表达式还没有结束，所以，解释器会在下一行显示一个竖线“|”，你可以在第2行继续输入”Hello World!”然后回车，解释器就会得到执行结果，具体如下：
```scala
val myStr4 =
|  "Hello World!"
```

此外，如果我们在命令提示符后面输入“val myStr5 = ”然后就回车，解释器会在下一行显示一个竖线“|”，这时如果我们发现变量名称错误，想放弃本次输入，就可以在“|”后面连续敲入两个回车，结束本次输入，具体如下：
```scala
val myStr5 =
 |
 |
You typed two blank lines.  Starting a new command.
```


## 基本数据类型

Scala的数据类型包括：Byte、Char、Short、Int、Long、Float、Double和Boolean。和Java不同的是，在Scala中，这些类型都是“类”，并且都是包scala的成员，比如，Int的全名是scala.Int。对于字符串，Scala用java.lang.String类来表示字符串。  
  
这里要明确什么是“字面量”？字面量包括整数字面量、浮点数字面量、布尔型字面量、字符字面量、字符串字面量、符号字面量、函数字面量和元组字面量。举例如下：

```scala
val i =  123  //123就是整数字面量
val i =  3.14  //3.14就是浮点数字面量
val i =  true  //true就是布尔型字面量
val i =  'A'  //'A'就是字符字面量
val i =  "Hello"  //"Hello"就是字符串字面量
```

Scala允许对“字面量”直接执行方法，比如：
```scala
5.toString()  //产生字符串"5"
"abc".intersect("bcd")  //输出"bc"
```
上面的intersect()方法用来输出两个字符串中都存在的字符。


## 操作符

在Scala中，可以使用加(+)、减(-) 、乘(*) 、除(/) 、余数（%）等操作符，而且，**这些操作符就是方法**。例如，5 + 3和(5).+(3)是等价的，也就是说：

`a 方法 b` == `a.方法(b)`
上面这二者是等价的。前者是后者的简写形式，这里的+是方法名，是Int类中的一个方法。具体代码如下：

```scala
scala> val sum1 =  5  +  3  //实际上调用了 (5).+(3)
sum1:  Int  =  8
scala> val sum2 =  (5).+(3)  //可以发现，写成方法调用的形式，和上面得到相同的结果
sum2:  Int  =  8
```

需要注意的是，和Java不同，在Scala中并没有提供++和–操作符，当需要递增和递减时，可以采用如下方式表达：
```scala
scala>  var i =  5;
i:  Int  =  5
scala> i +=  1  //将i递增
scala> println(i)
6
```

此外，也可以使用关系和逻辑操作，比如，大于(>)、小于(<)、大于等于(>=)和小于等于(<=)，会产生Boolean类型的结果。


## Range

在执行for循环时，我们经常会用到数值序列，比如，i的值从1循环到5，这时就可以采用Range来实现。Range可以支持创建不同数据类型的数值序列，包括Int、Long、Float、Double、Char、BigInt和BigDecimal等。  
  
在创建Range时，需要给出区间的起点和终点以及步长（默认步长为1）。下面通过几个实例来介绍：  
（1）创建一个从1到5的数值序列，包含区间终点5，步长为1
```scala
scala>  1 to 5
res0: scala.collection.immutable.Range.Inclusive  =  Range(1,  2,  3,  4,  5)
```

之前我们已经介绍过，在Scala中允许对“字面量”直接执行方法，所以，上面的代码，也可以用下面的代码来实现：
```scala
scala>  1.to(5)
res0: scala.collection.immutable.Range.Inclusive  =  Range(1,  2,  3,  4,  5)
```

（2）创建一个从1到5的数值序列，不包含区间终点5，步长为1
```scala
scala>  1  until  5
res1: scala.collection.immutable.Range  =  Range(1,  2,  3,  4)
```

（3）创建一个从1到10的数值序列，包含区间终点10，步长为2
```scala
scala>  1 to 10  by  2
res2: scala.collection.immutable.Range  =  Range(1,  3,  5,  7,  9)
```

（4）创建一个Float类型的数值序列，从0.5f到5.9f，步长为0.3f
```scala
scala>  0.5f to 5.9f  by  0.8f
res3: scala.collection.immutable.NumericRange[Float]  =  NumericRange(0.5,  1.3,  2.1,  2.8999999,  3.6999998,  4.5,  5.3)
```


## 读写文件
### 写入文本文件
Scala需要使用java.io.PrintWriter实现把数据写入到文本文件。  
假设当前正使用用户名hadoop登录Linux系统，打开Scala解释器进入命令提示符状态后，输入以下代码:

```scala
import java.io.PrintWriter
val outWriter  =  new  PrintWriter("output.txt")
for (i <-  1 to 5)  outWriter.println(i)
outWriter.close()
```


## 读取文本文件中的行

可以使用Scala.io.Source的getLines方法实现对文件中所有行的读取。  
仍然假设当前是用hadoop用户登录了Linux系统，并且使用scala命令启动了Scala解释器，现在，我们要把上面刚生成的、在hadoop用户工作目录下的output.txt文件读取出来，下面给出了完整的读取文件实例代码：

```scala
import scala.io.Source
val inputFile =  Source.fromFile("output.txt")
val lines = inputFile.getLines //返回的结果是一个迭代器
for  (line <- lines) println(line)
```


## 控制
### if

请在Linux系统中进入到/usr/local/scala目录，并在之前已经创建好的mycode目录下新建test.scala，用于测试我们撰写的代码。  

1.  cd /usr/local/scala
2.  vim ./mycode/test.scala

Shell 命令

请在test.scala文件中输入以下代码。

```
val x = 6
if (x>0) {println("This is a positive number")
} else {
    println("This is not a positive number")
}

```

保存test.scala文件，然后，使用下面命令执行代码（再次注意，当前工作目录是/usr/local/scala）：

1.  scala ./mycode/test.scala

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTg0Njk3NTY5NywtODE0Mzk2NzEsMTY2OD
U3ODI4LC01MjkyMDcwNCwxNzAwMTk1Njk2LDExMDA1NjAwMDEs
LTIxMTgwMTIxODgsMTY2NzMwMTk1NiwtMTU5MTk2NzM4MCw0Nz
Y2Njc0ODIsLTEzMjA1MjM4MzIsMTU2NDAzNTA4OCwxOTQ3Mzg5
MzYxLC0xMzk1MTg5NTg4LDQyMDUyNjQ2OCwzNzQ2MDA3NTksMT
E2MjY1MjkxMywtODY1OTg5NDMwLDEwODYyMzUxMzcsLTE1NDMy
NzkxMzVdfQ==
-->