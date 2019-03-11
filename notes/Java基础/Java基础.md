## 
总结一下常见的Java面试知识点：
### 1.Java面向对象的特点：   
抽象&nbsp;&nbsp; 封装&nbsp;&nbsp; 继承&nbsp;&nbsp; 多态  
抽象：对现实的一类事物，抽取其特点，并对其特点进行整合  
封装：把同一个类事物的属性和方法归到一个类中，目的就是隐藏内部的细节，只保留一些外部接口使之与外界进行联系  
继承：从已有的类派生出新的类，并可以扩展新的功能  
多态：可以理解为事物存在的多种体现形态，基类指针指向派生类，同样的引用会导致不同的结果  
实现方式：  
（1）方法重载：实现的是编译时的多态性，也称为前绑定  
a.函数名一致  
b.形参列表不一致（形参的个数或者形参的类型不一致）  
c.与返回值类型无关    
（2）方法重写：实现的是运行时的多态性（两同两小一大原则）  
a.方法名和形参列表必须一致  
b.子类的权限修饰符大于等于父类的权限修饰符  
c.子类的返回值类型必须小于等于父类的返回值类型  
d.子类抛出的异常类型小于等于父类抛出的异常类型  
优点：提高了代码的扩展性  
弊端：不能使用子类自己特有的功能（可以通过向下转型来实现）  
### 2.Obiect类源码分析
![image](http://tu.027cgb.com/613363/object.png)
### 3.八大基本包装类型
byte<char<short<int<long<float<double
![eight](http://tu.027cgb.com/613363/031101.png)  
感觉面试最常问的应该就是Integer的源码和自动拆装箱了  
**Integer源码分析：**  
```
public final class Integer extends Number implements Comparable<Integer> 
```
#### 从Integer的定义可以知道三点：  
1.Integer不能被继承  
2.他实现了Comparable接口，所以可以用compareTo方法进行比较并且Integer对象只能和Integer类型的对象进行比较，不能和其他类型比较（至少调用compareTo方法无法比较）。  
3.Integer继承了Number类，所以该类可以调用longValue、floatValue、doubleValue等系列方法返回对应的类型的值。
#### Integer提供了两个构造方法
```
//构造一个新分配的 Integer 对象，它表示指定的 int 值。
public Integer(int value) {
    this.value = value;
}

//构造一个新分配的 Integer 对象，它表示 String 参数所指示的 int 值。
public Integer(String s) throws NumberFormatException {
    this.value = parseInt(s, 10);
}
```
里面的parseInt方法可以把实现字符串的和int类型的转换,Integer里面有如下方法都可以实现转换
```
Integer getInteger(String nm)
Integer getInteger(String nm, int val)
Integer getInteger(String nm, Integer val)
Integer decode(String nm)
Integer valueOf(String s)
Integer valueOf(String s, int radix)
int parseUnsignedInt(String s)
int parseUnsignedInt(String s, int radix)
int parseInt(String s)
int parseInt(String s, int radix)
```
他们之间的调用栈是这样的：  
getInteger(String nm) ---> getInteger(nm, null);--->Integer.decode()--->Integer.valueOf()--->parseInt()  
以上方法的区别就是说parseInt方法返回的是基本类型，其他方法返回的Integer类型  
#### 再来说所int转String的方法
```
String  toString()
static String   toString(int i)
static String   toString(int i, int radix)
static String   toBinaryString(int i)
static String   toHexString(int i)
static String   toOctalString(int i)
static String   toUnsignedString(int i)
static String   toUnsignedString(int i, int radix)
```
#### 下面引出拆装箱及进行分析
```
public class IntegerTest {
    public static void main(String[] args) {
        Integer i = new Integer(10);
        i = 5;
    }
}
```
我们都知道这一段代码的结果肯定是5，但是他之中是新建了一个对象还是返回已经有的对象了呢？？  
根据代码反编译后的结果，i=5编译器会转成i = Integer.valueOf(5)执行。这里就解释一下valueOf(int i)方法是如何给变量赋值的。其实在Integer的内部维护了一个-128到127的缓冲数组，valueOf方法就是其中的参数在不在这个数组如果在的话直接返回如果超过了这个限制，会新建一个Integer对象返回。这其实就是装箱的过程。也就是说Valueof方法一定会返回一个Integer类型的对象。  
```
static {
        // high value may be configured by property
        int h = 127;
        String integerCacheHighPropValue =
            sun.misc.VM.getSavedProperty("java.lang.Integer.IntegerCache.high");
        if (integerCacheHighPropValue != null) {
            try {
                int i = parseInt(integerCacheHighPropValue);
                i = Math.max(i, 127);
                // Maximum array size is Integer.MAX_VALUE
                h = Math.min(i, Integer.MAX_VALUE - (-low) -1);
            } catch( NumberFormatException nfe) {
                // If the property cannot be parsed into an int, ignore it.
            }
        }
        high = h;

        cache = new Integer[(high - low) + 1];
        int j = low;
        for(int k = 0; k < cache.length; k++)
            cache[k] = new Integer(j++);

        // range [-128, 127] must be interned (JLS7 5.1.7)
        assert IntegerCache.high >= 127;
    }
```
自动拆箱调用的是IntValue方法,intValue()是java.lang.Number类的方法，Number是一个抽象类。Java中所有的数值类都继承它。也就是说，不单是Integer有intValue方法，Double，Long等都有此方法。 
### 3.String，StringBuffer，StringBuilder
（1）都是final类不允许被继承  
（2）String不可变，StringBuffer，StringBuilder可变  
（3）StringBuffer是安全的String和StringBuilder不安全  
（4）StringBuilder比StringBuffer拥有更好的性能  
（5）大多数情况StringBuffer比String的性能要高   
举个例子：  
        String str="Hello"+"World";              StringBuffer sb=new StringBuffer("Hello").append("World");  
这个时候String的效率高这是因为JVM就把 String str="Hello"+"World"; 认为是一个赋值，不认为是一个拼接
 String str1="World";        String str="Hello"+str1;   
      StringBuffer sb=new StringBuffer("Hello").append("World");  
这个时候StringBuffer的效率更高，是因为String追加是重新开辟新的内存存入新的内容把原来的指针指过去而StringBuffer是直接在原来的地方进行追加
#### String源码分析：  
感觉String里面最重要的就是字符串常量池的思想  
```
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence
```
首先String是由final关键修饰的也就是说String对象是不可变量，j仅仅是说String对象的引用不可变，但里面的字符串的值还是可以进行改变的。  
#### String的equals方法：
```
public boolean equals(Object anObject) {
    //如果引用的是同一个对象，返回真
    if (this == anObject) {
        return true;
    }
    //如果不是String类型的数据，返回假
    if (anObject instanceof String) {
        String anotherString = (String) anObject;
        int n = value.length;
        //如果char数组长度不相等，返回假
        if (n == anotherString.value.length) {
            char v1[] = value;
            char v2[] = anotherString.value;
            int i = 0;
            //从后往前单个字符判断，如果有不相等，返回假
            while (n-- != 0) {
                if (v1[i] != v2[i])
                        return false;
                i++;
            }
            //每个字符都相等，返回真
            return true;
        }
    }
    return false;
}
```
1.首先判断是否引用同一个对象 == 也就是判断这两个引用的内存地址是否相同，如果相同直接返回true    
2 会判断是否类型相同是否是同一种数据类型  
3 类型相同就会比较转换成的字符数组的长度是否相同  
4 从后往前比较每一个字符是否相同  
判断顺序 =》 1.内存地址 2.数据类型 3.字符数组长度 4.单个字符比较

**字符串常量池的设计思想：**  
每个字符串都是一个String对象，系统开发中将会频繁使用字符串，如果像其他对像那样创建销毁将极大影响程序的性能。  
* JVM为了提高性能和减少内存开销，在实例化字符串的时候进行了优化  
1.为字符串开辟了一个字符串常量池，类似于缓存区  
2.创建字符串常量时，首先判断字符串常量池是否存在该字符串  
3.存在该字符串返回引用实例，不存在，实例化字符串，放入池中  
* 实现基础  
1.实现该优化的基础是每个字符串常量都是final修饰的常量，不用担心常量池存在数据冲突  
2.运行时实例创建的全局字符串常量池中有一个表，总是为池中每个唯一的字符串对象维护一个引用,这就意味着它们一直引用着字符串常量池中的对象，所以，在常量池中的这些字符串不会被垃圾收集器回收

由于字符串常量池存在于方法区，对象的主体是在堆内存创建的，栈中还要存储局部变量等信息  所以String对象在Java内存布局如下
```
String str1 = “abc”;
String str2 = “abc”;
String str3 = “abc”;
String str4 = new String(“abc”);
String str5 = new String(“abc”);
``` 
![String](https://sfault-image.b0.upaiyun.com/367/903/367903008-5951291f92d71)
最后分享两个常见的这块的面试题：  
1.String str4 = new String(“abc”) 创建多少个对象？  
new String的话无论字符串常量池中有没有此对象都会重新创建一个，
“abc”的话如果常量池中有创建一个对象如果没有的话创建两个对象，最后会把对象给str4这个引用。  
2.为什么String对象不可变？？  
相信很多人和我一样刚开始的时候都会认为就是因为String对象是由final修饰的所以是不可变的，这是一种错误的思想。final修饰常量的时候，常量的值是不可变的，这是对的，但是当fianl修饰变量的时候，仅仅代表着这个字符串的引用是不会变的，但是字符串里面的值还是可以改变的，那为什莫String对象是不可变的呢？这是因为他里面的字符串缓冲数组是由private修饰的，这就意味着，private的权限很大的，只能在缓冲数组的内部实现方法改变其值但是看完整个源码都没有找到里面的可以改变字符串字符的方法。




