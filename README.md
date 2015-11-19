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

2.2 在scala REPL中，计算3的平方根，然后对该值求平方，计算这个结果与3相差多少？
```scala
val a = math.sqrt(3)
val resu = 3 - a*a
```

2.3 res变量是val还是var?

> 是val,给res变量重新赋值即可，val是不可边，var是可变的

2.4 在scala REPL中操作"crazy" * 3,这个操作是什么？在Scaladoc中的哪一部分？

> 会得到"crazycrazycrazy",和python中的"a" * 3一样，在scaladoc中的StringOPs部分，Return the current string concatenated n times.

2.5 10 max 2 的含义是什么？max方法定义在哪个类中？

> 返回两个数中较大的那一个，相似的方法还有min,在RichInt类中

2.6 用BigInt计算2的1024次方

```scala
BigInt(2).pow(1024)
```

2.7 为了直接使用probablePrime(100,Random)获取随机素数，需要引入什么？

> 和python中的导包类似,Random在util这个包中,probablePrime在BigInt这个类的伴生对象中

```scala
import scala.math.BigInt._ 
import scala.util.Random

probablePrime(3,Random)
```

2.8 使用Scala穿建议个随机文件(生成一个随机的BigInt，转换成36进制)

```scala
scala.math.BigInt(scala.util.Random.nextInt).toString(36)
```

2.9 获取字符串的首字符和尾字符

```scala
"abc".head
"abc".last
```

2.10 take,drop,takeRight和dropRight这些字符串函数的作用？和substring相比，他们的优点和缺点都是哪些？

> take是从头取字符串，drop从头去除字符串，takeRight和dropRight从尾开始操作，他们都是单向的；substring可以直接取中间字符串
