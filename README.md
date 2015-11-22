## scala for the impatient homework

快学Scala这本书每章后面的练习题

## 第一章:基础

1.1 在scala REPL中键入3.，然后tab。有哪些方法可以被应用？
```scala
!=   *   <        ==   >>>            getClass       toChar     toLong     unary_-
##   +   <<       >    ^              hashCode       toDouble   toShort    unary_~
%    -   <=       >=   asInstanceOf   isInstanceOf   toFloat    toString   |
&    /   <init>   >>   equals         toByte         toInt      unary_+ 
```

1.2 在scala REPL中，计算3的平方根，然后对该值求平方，计算这个结果与3相差多少？
```scala
val a = math.sqrt(3)
val resu = 3 - a*a
```

1.3 res变量是val还是var?

> 是val,给res变量重新赋值即可，val是不可边，var是可变的

1.4 在scala REPL中操作"crazy" * 3,这个操作是什么？在Scaladoc中的哪一部分？

> 会得到"crazycrazycrazy",和python中的"a" * 3一样，在scaladoc中的StringOPs部分，Return the current string concatenated n times.

1.5 10 max 2 的含义是什么？max方法定义在哪个类中？

> 返回两个数中较大的那一个，相似的方法还有min,在RichInt类中

1.6 用BigInt计算2的1024次方

```scala
BigInt(2).pow(1024)
```

1.7 为了直接使用probablePrime(100,Random)获取随机素数，需要引入什么？

> 和python中的导包类似,Random在util这个包中,probablePrime在BigInt这个类的伴生对象中

```scala
import scala.math.BigInt._
import scala.util.Random

probablePrime(3,Random)
```

1.8 使用Scala创建一个随机文件(生成一个随机的BigInt，转换成36进制)

```scala
scala.math.BigInt(scala.util.Random.nextInt).toString(36)
```

1.9 获取字符串的首字符和尾字符

```scala
"abc".head
"abc".last
```

1.10 take,drop,takeRight和dropRight这些字符串函数的作用？和substring相比，他们的优点和缺点都是哪些？

> take是从头取字符串，drop从头去除字符串，takeRight和dropRight从尾开始操作，他们都是单向的；substring可以直接取中间字符串


### 第二章 控制结构和函数

2.1 一个数字如果是正数，则返回1，负数返回-1,0返回0,编写一个函数
```scala
def count_signum(num: Int) = if (num > 0) 1 else if (num < 0) -1 else 0
```

2.2 一个空的块变大事{}的值是什么？类型是什么？

> 在scala REPL可得，值是()，类型是Unit

2.3 在Scala中，哪种情况下x = y = 1是合法的？

> 在Scala中，赋值语句的值是Unit类型的，所以y = 1的值是Unit类型，所以把x定义为Unit类型就可以了

2.4 针对下列Java循环编写一个Scala版

```java
for (int i = 10; i >=0; i--) System.out.println(i);
```

```scala
for (i <- 0 to 10 reverse) print (i)
```

2.5 编写一个过程countdown(n: Int),打印从n到0的数字

过程是没有返回值的函数
```scala
def countdown(n: Int) {for (i <- 0 to 10 reverse) print (i)}
```

2.6 编写一个for循环，计算字符串中所有字母的Unicode代码的成绩。

```scala
var count: Long = 1
for (i <- "Hello") count *= i.toLong
```
2.7 不使用循环解决上一个问题

```scala
var count: Long = 1
"Hello".foreach((x:Char) => count *= x.toLong)
```

2.8 编写一个函数product(s: String)，计算前面联系中提到的乘积
```scala
def product(s: String): Long={
    var count: Long = 1
    for (i <- s) count *= i.toLong
    count
    }
```

2.9 把前一个联系中的函数改成递归函数
```scala
def product(s: String): Long= if (s.length == 1) return s.head.toLong else s.take(1).head.toLong * product(s.drop(1))
```
2.10 编写函数计算x^n, 其中$n$是整数，使用如下的递归定义：
 - x^n=y^2, 如果$n$是正偶数的话，这里的y=x^(n/2)
 - x^n=x * x^(n-1)，如果n是正奇数的话
 - x^0 = 1
 - x^n=1/x^(-n), 如果n是负数的话

不能使用return语句

```scala
def power(x: Double, n: Int): Double={
    if (n==0) 1
    else if (n%2 == 0 && n > 0) power(x, n/2)*power(x, n/2)
    else if (n%2 == 1 && n > 0) x*power(x, n-1)
    else 1/power(x, -n)
    }
```

### 第三章 数组相关操作

3.1 编写一段代码，将a设置为一个n个随机整数的数组，要求随机数介于[0, n)之间

查阅scaladoc的random包里面的nextInt方法

```scala
import scala.util.Random

def randomArray(n :Int) = for (i <- 0 until n) yield Random.nextInt(n)
```

3.2 编写一个循环，将整数数组中相邻的元素置换。

```scala
def replacedArray(array: Array[Int]): Array[Int] = {
    for (i <- 1 until (array.length, 2)){
        var tmp = array(i)
        array(i) = array(i-1)
        array(i-1) = tmp
        }
    array
    }
```

3.3 重复上一个练习，这次生成一个新的值交换过的数组。

```scala
def replacedArray(array: Array[Int]): Array[Int] = {
    for (i <- 1 until (array.length, 2)){
        var tmp = array(i)
        array(i) = array(i-1)
        array(i-1) = tmp
        }
    for (i <- array) yield i
    }
```

3.4 给定一个整数数组，产出一个新的数组，包含元数组中的所有正值，以原有顺序排列，之后的元素是所有零或者负值，以原有顺序排列

```scala
def sortedArray(array: Array[Int]): Array[Int] = (array.filter(_ > 0).toBuffer ++ array.filter(_ <= 0)).toArray
```

3.5 如何计算Array[Double]的平均值

```scala
def countAverage(array: Array[Double]): Double = array.sum/array.length
```

3.6 如何重新组织Array[Int]的元素将它们逆序？对于ArrayBuffer[Int]如何做？

> Array和ArrayBuffer均有reverse方法来将元素逆序

3.7 编写一段代码，产出数组中的所有值，去重

```scala
for (i <- array.distinct) print (i)
```

3.8 对3.4，收集负值元素的下标，反序，去掉最后一个下标，然后对每一个下标调用a.remove(i)

```scala
def remove(array: Array[Int]): Array[Int] = {
    val newArray = array.toBuffer
    val index = (for (i <- 0 until array.length if array(i) < 0) yield i).toBuffer
    index.remove(0)
    index.reverse.foreach(newArray.remove(_))
    newArray.toArray
    }
```

3.9 创建一个由java.util.TimeZone.getAvailableIDs返回的时区集合，判断条件是它在每周。去掉"America/"前缀并排序
```scala
import java.util.TimeZone.getAvailableIDs
val timeZones = (for (i <- getAvailableIDs if i.startsWith("America/")) yield i.replace("America/", "")).sorted
```

3.10 引入java.awt.datatransfer.\_,并构建一个类型为SystemFlavorMap类型的对象：
```java
val flavors = SystemFlavorMap.getDefaultFlavorMap().asInstanceof[SystemFlavorMap]
```
然后以DataFlavor.imageFlavor为参数调用getNativesForFlavor方法，以Scala缓冲保存返回值。
```scala
import java.awt.datatransfer._

val flavors = SystemFlavorMap.getDefaultFlavorMap().asInstanceOf[SystemFlavorMap]
val imageArray = flavors.getNativesForFlavor(DataFlavor.imageFlavor).toArray.toBuffer
```
