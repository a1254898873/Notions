# Python

## 变量

### 原理

Python 中一切都是对象，变量中存放的是对象的引用。

```
 x = 'blue'  
 y = 'green'  
 z = x  
```

当Python执行上面第一句的时候，会在heap中首先创建一个str对象，其文本内容为blue，同时还创建一个名为x的对象引用，x引用的就是这个str对象。

　　第二句也是类似；

　　第三条创建了一个名为z的新对象引用，并将其设置为对象引用x所指向的相同对象。

![img](https://raw.githubusercontent.com/a1254898873/images/master/202203242157634.png)



### 使用

1、Python 中的变量不需要声明类型



2、用“=”号来给变量赋值



3、每个变量在使用前都必须赋值，变量赋值以后才会被创建。



4、Python允许同时为多个变量赋值。

```
a = b = c = 1
```



5、可以同时为多个变量赋值，用逗号分隔，逐一对应。

a, b, c = 1, 2, 3，最后a是1，b是2，c是3.







### 数据类型

1、数字

a. int（整型）

　　在32位机器上，整数的位数为32位，取值范围为-2**31～2**31-1，即-2147483648～2147483647
　　在64位系统上，整数的位数为64位，取值范围为-2**63～2**63-1，即-9223372036854775808～9223372036854775807

b. long（长整型）

跟C语言不同，Python的长整数没有指定位宽，即：Python没有限制长整数数值的大小，但实际上由于机器内存有限，长整数数值不可能无限大。
　　注意，自从Python2.2起，如果整数发生溢出，Python会自动将整数数据转换为长整数，所以如今在长整数数据后面不加字母L也不会导致严重后果了。

c. float（浮点型）

　　浮点数用来处理实数，即带有小数的数字。类似于C语言中的double类型，占8个字节（64位），其中52位表示底，11位表示指数，剩下的一位表示符号。

d. complex（复数）
　　复数由实数部分和虚数部分组成，一般形式为x＋yj，其中的x是复数的实数部分，y是复数的虚数部分，这里的x和y都是实数。

​    注：Python中存在小数字池：-5 ～ 257





2、字符串

split

```
>>> a  = 'hello world'
>>> b = a.split('l')
>>> b
['he', '', 'o wor', 'd']
>>> 
```

len 字符串长度获取

```
>>> a  = 'hello world'
>>> b = len(a)
>>> b
11
>>> 
```

replace

```
>>> a  = 'hello world'
>>> b = a.replace('l','t')
>>> b
'hetto wortd'
>>> 
```



[] 截取字符串

注意：一定要搞清楚下标是从0开始的，列表右边的元素是不被包含的

```
>>>a = '0123456789'
>>>b = a[0:3] # 截取第一位到第三位的字符
>>>b
'012'

>>>b = a[:] # 截取字符串的全部字符
>>>b
'0123456789' 

>>>b = a[6:] # 截取第七个字符到结尾
>>>b
'6789' 

>>>b = a[:-3] # 截取从头开始到倒数第三个字符之前
>>>b
'0123456'

>>>b = a[2] # 截取第三个字符
>>>b
'2' 

>>>b = a[-1] # 截取倒数第一个字符
>>>b
'9' 

>>>b = a[::-1] # 创造一个与原字符串顺序相反的字符串
>>>b
'9876543210' 

>>>b = a[-3:-1] # 截取倒数第三位与倒数第一位之前的字符
>>>b
'78' 

>>>b = a[-3:] # 截取倒数第三位到结尾
>>>b
'789'
```



3、list

实质是长度可变的动态数组，并不是链表

len() 方法返回对象（字符、列表、元组等）长度或项目个数。



4、元组

元组其实也称为只读列表，列表支持的函数元组同样也支持，唯一区别是元组tuple中的数据不能被修改，这就意味着不能删除元组tuple中的数据，也不能直接给元组tuple中的数据赋值。



5、字典

字典是通过散列表或说哈希表实现的

```
>>> d = {'Michael': 95, 'Bob': 75, 'Tracy': 85}
>>> d['Michael']
95
```



6、集合

要创建一个set，需要提供一个list作为输入集合

```
>>> s = set([1, 1, 2, 2, 3, 3])
>>> s
{1, 2, 3}
```







## 循环

### for循环



```
for 变量 in 可迭代对象(序列):
    循环语句块
```

range()函数介绍

 Python中range()函数返回的是一个整数序列的对象，一般用在 for 循环中。 

函数语法 range(start, stop,[step]) 参数说明：start: 计数从start开始。默认是从 0 开始。end: 计数到end结束，但不包括 end。step：步长，默认为1。

```
range(10)  # 从 0 开始到 0, 1, 2, 3, 4, 5, 6, 7, 8, 9
range(1, 11) # 从 1 开始到 11 [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
range(0, 30, 5)  # 步长为 5 [0, 5, 10, 15, 20, 25]
range(-10, 0, 2)  # 负数 [-10, -8, -6, -4, -2]
```



### while循环

```
while 条件表达式 :
    代码块
else :
    代码块
```





## 函数



```
def 函数名(参数1，参数2....参数n):
    函数体
    return 语句
```

- 函数代码块以 def 关键词开头，后接函数标识符名称和圆括号()
- 任何传入参数和自变量必须放在圆括号中间。圆括号之间可以用于定义参数
- 函数的第一行语句可以选择性地使用文档字符串（用于存放函数说明）
- 函数内容以冒号起始，并且缩进
- return [表达式] 结束函数，选择性地返回一个值给调用方。不带表达式的 return 相当于返回 Non







## 面向对象

### 类的实例



```
class MyClass(object):
    def __init__(self):        
        print("MyClass类的构造方法被调用！")   
        
class1=MyClass()
```

```
class MyClass(object):
    def __init__(self,data1,data2):
        self.__data1=data1
        self.data2=data2    
        
class1=MyClass('first_data','sencond_data')
print(class1.data2)
```

self代表当前类的实例



### 类的封装

类的封装有两层含义，一个是对数据的封装，一个是对实现逻辑即方法的封装。



### 访问级别

- Python中以双下划线开头的字段访问级别是private;
- Python中以下划线开头的字段访问级别是protected;
- Python中未以下划线开头的字段的访问级别是public;



### 类的继承

```
class 类名([父类名[,父类名[,...]]]):
    pass
```

Python类中没有方法的重载，对于具有相同名称的方法，后面的定义会覆盖掉前面的定义；子类会覆盖父类中同名的方法。

如果你想强制调用父类的成员可以使用super()函数。

语法：super(子类名, self).方法名()，需要传入的是子类名和self，调用的是父类里的方法，按父类的方法需要传入参数。

```
class A:
    def __init__(self, name):
        self.name = name
        print("父类的__init__方法被执行了！")
    def show(self):
        print("父类的show方法被执行了！")

class B(A):
    def __init__(self, name, age):
        super(B, self).__init__(name=name)
        self.age = age

    def show(self):
        super(B, self).show()

obj = B("jack", 18)
obj.show()
```



### __init__方法和__new__方法

1、__init__方法是面向对象编程中，给未来创建的对象所定义的初始化属性。当对象一旦被创建，Python将会自动调用__init__方法，里面的属性将会赋予这个对象

```
class Teacher(object):
    def __init__(self,name,age):
        self.name = name
        self.age = age


t1 = Teacher('zs',30)  

```





2、__init__是属于Python中的魔法方法。所谓魔法方法，即是Python中内置的、当进行特定操作时，会自动调用的方法，表现为方法名前后有两个下划线



3、__new__方法是将对象创建出来的方法。在实际运行中，先走__new__方法，生成对象并返回，后调用__init__方法，将对象的引用传给__init__方法中的self

```
lass Teacher(object):
    def __init__(self):
        print('__init__')

    def __new__(cls,*args,**kwargs):
        print('__new__')
        return object.__new__(cls)

t1 = Teacher()

```



### __str__方法

和java的tostring一样

```
    def __str__(self):
        """返回一个对象的描述信息"""
        # print(num)
        return "名字是:%s , 年龄是:%d" % (self.name, self.age)
```



## *args和**kwargs的区别

python语言中，可以在函数定义中使用*args和kwargs传递可变长参数。其中，*args用作传递非命名键值可变长参数列表（位置参数）；kwargs用作传递键值可变长参数列表。

1、*args的使用方法

*args 用来将参数打包成tuple(元组)给函数体调用

```
def function(x, y, *args):
    print(x, y, args)

function(1, 2, 3, 4, 5)
```





2、**kwargs的使用方法

**kwargs 用来将参数打包成dict(字典)t给函数体调用

```
def function(**kwargs):
    print(kwargs)

function(a=1, b=2, c=3)
```



3、 arg，*args和**kwargs的使用方法

参数arg、*args、**kwargs三个参数的位置必须是一定的。必须是(arg,*args,**kwargs)这个顺序，否则程序会报错。

```
 def function(arg,*args,**kwargs):
    print(arg,args,kwargs)

function(6,7,8,9,a=1, b=2, c=3)

```





## 文件操作

1、读文件

```
with open('/path/to/file', 'r') as f:
    print(f.read())
```

二进制文件

前面讲的默认都是读取文本文件，并且是UTF-8编码的文本文件。要读取二进制文件，比如图片、视频等等，用'rb'模式打开文件即可：

```
>>> f = open('/Users/michael/test.jpg', 'rb')
>>> f.read()
b'\xff\xd8\xff\xe1\x00\x18Exif\x00\x00...' # 十六进制表示的字节
```



字符编码

要读取非UTF-8编码的文本文件，需要给open()函数传入encoding参数，例如，读取GBK编码的文件：

```
>>> f = open('/Users/michael/gbk.txt', 'r', encoding='gbk')
>>> f.read()
'测试'
```



2、写文件

```
with open('/Users/michael/test.txt', 'w') as f:
    f.write('Hello, world!')
```









## with语句

在python中读写操作资源，最后需要释放资源。可以使用try…finally结构实现资源的正确释放，python提供了一个with语句能更简便的实现释放资源。

```
with open('data', 'r', encoding='utf-8') as f:
    data = f.readlines()
```

上面的写法，与下面的写法在最终效果上是一致的

```
f = open('data', 'r', encoding='utf-8')
try:
    data = f.readlines()
except:
    pass
finally:
    f.close()
```

对比两段代码不难发现，使用with语句时，代码更加简洁，而且不用主动关闭文件，在with语句体退出时，会自动关闭文件，即便with语句体中发生了异常。



## 浅拷贝与深拷贝

```
import copy
a = [1,2,3,4,5]
b = a   #浅拷贝，a,b同时指向一个id,当其中一个修改时，另外一个也会被修改。
c = copy.deepcopy(a) #深拷贝，c单独开辟一个id,用来存储和a一样的内容。
d =a[:] #这样也是深拷贝。
e = copy.copy(a) #当拷贝内容是可变类型时，那么就会进行深拷贝，如果是不可变类型时，那么就会进行浅拷贝。
```





## 垃圾回收

Python GC主要使用引用计数（reference counting）来跟踪和回收垃圾。在引用计数的基础上，通过“标记-清除”（mark and sweep）解决容器对象可能产生的循环引用问题，通过“分代回收”（generation collection）以空间换时间的方法提高垃圾回收效率。

### 1、引用计数

PyObject是每个对象必有的内容，其中ob_refcnt就是做为引用计数。当一个对象有新的引用时，它的ob_refcnt就会增加，当引用它的对象被删除，它的ob_refcnt就会减少.引用计数为0时，该对象生命就结束了。

优点:

1. 简单
2. 实时性

缺点:

1. 维护引用计数消耗资源
2. 循环引用

### 2、标记-清除机制

基本思路是先按需分配，等到没有空闲内存的时候从寄存器和程序栈上的引用出发，遍历以对象为节点、以引用为边构成的图，把所有可以访问到的对象打上标记，然后清扫一遍内存空间，把所有没标记的对象释放。

### 3、分代技术

分代回收的整体思想是：将系统中的所有内存块根据其存活时间划分为不同的集合，每个集合就成为一个“代”，垃圾收集频率随着“代”的存活时间的增大而减小，存活时间通常利用经过几次垃圾回收来度量。

Python默认定义了三代对象集合，索引数越大，对象存活时间越长。





## 常用内置函数

1、set()

当需要对一个列表进行去重操作的时候，set()函数就派上用场了。

```
obj = ['a','b','c','b','a']
print(set(obj))
# 输出：{'b', 'c', 'a'}
```

集合对象创建后，还能使用并集、交集、差集功能。

```
A = set('hello')
B = set('world')

A.union(B) # 并集，输出：{'d', 'e', 'h', 'l', 'o', 'r', 'w'}
A.intersection(B) # 交集，输出：{'l', 'o'}
A.difference(B) # 差集，输出：{'d', 'r', 'w'}
```



2、sorted()

在处理数据过程中，我们经常会用到排序操作，比如将列表、字典、元组里面的元素正/倒排序。

这时候就需要用到sorted() ，它可以对任何可迭代对象进行排序，并返回列表。

对列表升序操作：

```
a = sorted([2,4,3,7,1,9])
print(a)
# 输出：[1, 2, 3, 4, 7, 9]
```

对元组倒序操作：

```
sorted((4,1,9,6),reverse=True)
print(a)
# 输出：[9, 6, 4, 1]
```



3、reversed()

如果需要对序列的元素进行反转操作，reversed()函数能帮到你。

reversed()接受一个序列，将序列里的元素反转，并最终返回迭代器。

```
a = reversed('abcde')
print(list(a))
# 输出：['e', 'd', 'c', 'b', 'a']

b = reversed([2,3,4,5])
print(list(b))
# 输出：[5, 4, 3, 2]
```





4、len

```
str = "runoob"
print( len(str) )          # 字符串长度
 
l = [1,2,3,4,5]
print( len(l) )            # 列表元素个数
```









## 代码规范

1、尽量用 enumerate 取代 range

```
for i, flavor in enumerate(flavor_list):
    print(f'{i + 1}: {flavor}')
```

- enumerate 函数可以用简洁的代码迭代 iterator，而且可以指出当前这轮循环的序号。
- 不要先通过 range 指定下标的取值范围，然后用下标去访问序列，而是应该直接用 enumerate 函数迭代。
- 可以通过 enumerate 的第二个参数指定起始序号（默认为 0）。





2、不要在 for 与 while 循环后写 else

Python 有种特殊的语法，可以把 else 块紧跟在整个 for 循环或 while 循环的后面。 只有在整个循环没有因为 break 提前跳出的情况下，else 块才会执行。 把 else 块紧跟在整个循环后面，会让人不太容易看出这段代码的意思，所以要避免这样写。



3、学会对序列做切片

- 切片要尽可能写得简单一些：如果从头开始选取，就省略起始下标 0 ；如果选到序列末尾，就省略终止下标。
- 切片允许起始下标或终止下标越界，所以很容易就能表达“取开头多少个元素”（例如 a[:20]）或“取末尾多少个元素”（例如 a[-20:0]）等含义，而不用担心切片是否真有这么多元素。
- 把切片放在赋值符号的左侧可以将原列表中这段范围内的元素用赋值符号右侧的元素替换掉，但可能会改变原列表的长度。



