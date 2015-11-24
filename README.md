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

### 第四章 映射和元组

4.1 设置一个映射，其中包含你想要的一些装备，以及它们的价格。然后构建另一个映射，采用同一组键，但在价格上打9折

```scala
val equipment = Map(("a", 10), ("b",15), ("c", 30))
val newEquipment = for ((k, v) <- equipment) yield (k, 0.9*v))
```

4.2 编写一段程序，从文件中读取单词。用一个可变映射来清点每一个单词出现的频率，打印出所有单词和它们出现的次数

```scala
import scala.io.Source

val source = Source.fromFile("myfile.txt")
val tokens = source.mkString.split("\\s+")
val wordsCount = new scala.collection.mutable.HashMap[String, Int]
for (word <- tokens) wordsCount(word) =  wordsCount.getOrElse(word, 0) + 1
for ((k, v) <- wordsCount) print (k, v)
```

4.3 重复前一个练习，这次使用不可变映射

```scala
import scala.io.Source

val Source = Source.fromFile("myfile.txt")
val tokens = source.mkString.split("\\s+")
var wordsCount = Map[String, Int]()
for (word <- tokens) wordsCount += (word -> (wordsCount.getOrElse(word, 0)+1))
for ((k, v) <- wordsCount) print (k, v)
```

4.4 重复前一个练习，这次使用已排序映射，以便单词可以按顺序打印出来

修改以下映射类型
```scala
val wordsCount = scala.collection.immutable.SortedMap[String, Int]()
```

4.5 重复前一个练习使用java.util.TreeMap并使之适用于Scala API
```scala
import scala.io.Source
import scala.collection.JavaConversions.mapAsScalaMap
import java.util.TreeMap

val source = Source.fromFile("myfile.txt")
val tokens = source.mkString.split("\\s+")
val wordsCount: scala.collection.mutable.Map[String, Int] = new java.util.TreeMap[String, Int]
for (word <- tokens) wordsCount(word) = wordsCount.getOrElse(word, 0) + 1
for ((k, v) <- wordsCount) print (k, v)
```

4.6 定义一个链式哈希映射，将"Monday"映射到java.util.Calendar.MONDAY, 依次类推加入其他日期。展示元素是以插入的顺序被访问的

使用LinkedHashMap
```scala
import scala.collection.mutable.LinkedHashMap
import java.util.Calendar

val day = new LinkedHashMap[String, Int]

day += ("Monday" -> Calendar.MONDAY)
day += ("Tuesday" -> Calendar.TUESDAY)
day += ("Wednesday" -> Calendar.WEDNESDAY)
day += ("Thursday" -> Calendar.THURSDAY)
day += ("Friday" -> Calendar.FRIDAY)
day += ("Saturday" -> Calendar.SATURDAY)
day += ("Sunday" -> Calendar.SUNDAY)
print (day.mkString(" , "))
```

4.7 打印所有java系统属性的表格，格式化

```scala
import scala.collection.JavaConversions.propertiesAsScalaMap
val props: scala.collection.Map[String, String] = System.getProperties()
val maxLength = (for (key <- props.keySet) yield key.length).max
for (key <- props.keySet) print (key + " " * (maxLength-key.length) + "| " + props(key)+"\n")
```

4.8 编写一个函数minmax(values: Array[Int]), 返回数组中最小值和最大值的对偶

```scala
def minmax(values: Array[Int]) = (values.max, values.min)
```

4.9 编写一个函数iteqgt(values: Array[Int], v: Int),返回数字钟小于v、等于v和大于v的数量，要求三个值一起返回

```scala
def iteqgt(values: Array[Int], v: Int) = (values.filter(_ < v).length, values.filter(_ = v).elgnth, values.filter(_ > v).lenght)
```

4.10 当你将两个字符串拉链在一起，比如"Hello".zip("World"),会是什么结果，想出一个讲的通的用例

```scala
scala.collection.immutable.IndexedSeq[(Char, Char)] = Vector((H,W), (e,o), (l,r), (l,l), (o,d))
```
> Returns a string formed from this string and another iterable collection by combining corresponding elements in pairs, 因为字符串"World"是可迭代集合，它的单位是每个字符，所以会返回两两组合的字符

### 第五章 类

5.1 改进5.1节的Counter类，让它不要在Int.Max.Value时变成负数

使用BigInt就好啦
```scala
class Counter {
    private var value: BigInt = 0
    def increment() { value += 1}
    def current() = value
    }
```

5.2 编写一个BankAccount类，加入deposit和withdraw方法，和一个只读的balance属性.

```scala
class BankAccount {
    val balance = 0
    def deposit() {}
    def withdraw() {}
    }
```

5.3 编写一个Time类，加入只读属性hours和minutes，和一个检查某一时刻是否早于另一时刻的方法before(other: Time): Boolean，Time对象应该
以new Time(hrs, min)方式构建，其中hrs小时数以军用时间格式呈现(介于0和23之间)

```scala
class Time(val hours:Int, val minutes:Int){
    def before(other: Time): Boolean = hours < other.hours || (hours == other.hours && minutes < other.minutes)
    }
```

5.4 重新实现前一个练习中的Time类，将每部接口呈现改成自午夜起的分钟数(介于0到24\*60-1)之间。不改变公有接口

```scala
class Time(val hours:Int, val minutes:Int){
    def before(other: Time): Boolean = hours * 60 + minutes < other.hours * 60 + other.minutes
    }
```

5.5 创建一个Student类，加入可读写的JavaBeans属性name(类型为String)和id(类型为Long)。有哪些方法被生成？(使用
javap查看)，可以在scala中调用JavaBeans版的getter和setter方法吗？应该这样做吗？


> 会生成8个方法，name和id各四个，分别是scala的setter和getter还有java的setfoo和getfoo方法
```scala
//在scala-2.11.7这个版本，Bean属性在scala.beans.BeanProperty这里
import scala.beans.BeanProperty

class Student {
    @BeanProperty var name: String = _
    @BeanProperty var id: Long = _
    }
```
下面是javap Student的结果
```scala
Compiled from "Student.scala"
public class Student {
    public java.lang.String name();
    public void name_$eq(java.lang.String);
    public void setName(java.lang.String);
    public long id();
    public void id_$eq(long);
    public void setId(long);
    public java.lang.String getName();
    public long getId();
    public Student();
    }
```

5.6 在5.2节的Person类中提供一个主构造器，将负年龄转换为0

> 主构造器会执行类定义中所有语句，在类定义中转换即可
```scala
class Person(var age:Int){
    age = if (age<0) 0 else age
    }
```

5.7 编写一个Person类，其主构造器接受一个字符串，该字符串包含名字、空格和姓，如new Person("Fred Smith"),
提供只读属性firstName和lastName.主构造器参数应该是var,val还是普通参数呢？为什么？

> val, 因为提供的两个属性是只读的

5.8 创建一个Car类，以只读属性对应制造商、型号名称、型号年份以及一个可读写的属性用于车牌。提供四组构造器。
每一个构造器都要求制造商和型号名称为必填。型号年份以及车牌为可选，如果未填，则型号年份设置为-1，车牌设置
为空字符串。你会选择哪一个作为你的主构造器？为什么？

> 这四组构造器是什么？

5.9 在Java、C#和C++重做前一个练习。Scala相比精简多少？

> 这三门语言我一门也不会真是为难我了

5.10 考虑如下类
```scala
class Employee(val name:String, var salary: Double) {
    def this() {this("John Q. Public", 0.0)}
    }
```
重写改类，使用显示的字段定义，和一个缺省主构造器。你更倾向于那种形式？为什么？

```scala
class Employee{
    val name: String = "John Q. Public"
    val salary: Double = 0.0
    }
```
第二种，感觉什么东西带上了this就费解了好几倍

### 第六章 对象
