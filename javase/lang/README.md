# 基本类型
java中基本类型对象都包含缓冲区，其中Integer通过数组的方式存储了默认-127到128的数字对象，其valueOf方法就是首先查找缓冲区中是否存在，如果不存在，则新建对象（可以通过JVM参数修改缓冲区大小）。
- jdk1.5引入了自动装箱和拆箱的功能，在Integer和int之间操作，具体例子如下：
```java
// Integer.valueOf
Integer i = 10;
// Integer.intValue
int j = i++;
```

- 由于在int和Integer使用时，Integer作为一个对象，会包含对象头（Mark Word，Hash值，GC年龄，锁等信息）和实例数据，所以占内存较大，建议使用int。

- Integer时不可变对象，value是final修饰，但是不是线程安全的。

## float和dobule
1.1 字面量属于 double 类型，不能直接将 1.1 直接赋值给 float 变量，因为这是向下转型。Java 不能隐式执行向下转型，因为这会使得精度降低。
```java
// float f = 1.1;
float f = 1.1f;
short s1 = 1;
// s1 = s1 + 1; no 结果是int
// += 可以隐式转换
s1 += 1;
```

> 因为字面量 1 是 int 类型，它比 short 类型精度要高，因此不能隐式地将 int 类型下转型为 short 类型。

## Switch
- 从 Java 7 开始，可以在 switch 条件判断语句中使用 String 对象;
- switch 不支持 long，是因为 switch 的设计初衷是为那些只需要对少数的几个值进行等值判断，如果值过于复杂，那么还是用 if 比较合适。

# String
String是final修饰的不可变对象（不可继承），内部持有final修饰的字符数组，String不可变好处：
- 缓存hash值，一次计算；
- String Pool，如果已经创建过的String，直接从字符串常量池中获取（堆），如果值经常变化，则无法使用字符串常量池；
- 天生具备线程安全性；

## StringBuffer与StringBuilder
- String 不可变，因此是线程安全的
- StringBuilder 不是线程安全的
- StringBuffer 是线程安全的，内部使用 synchronized 来同步

## String pool
可以使用String.intern()将字符串加入到字符串常量池中（java1.7以前不建议这么做，因为字符串常量池在永久代，容易造成OutOfMemory。

# 参数传递
Java中参数传递是值传递，对象传递是将对象的地址作为值传入到方法中。而传递对象的引用指向是不会被方法改变的，而引用指向的内容是会被方法改变的。（最简单的验证内容，在方法中交换参数引用，方法外不会受影响）

# 抽象类与接口
- 抽象类与正常类的最大区别在于不能实例化，同时可能包含抽象方法（抽象方法一定在抽象类中）。
- 接口，接口是抽象类延申，接口的成员（字段 + 方法）默认都是 public 的，并且不允许定义为 private 或者 protected。接口的字段默认都是 static 和 final 的。JDK1.8之后，可以有默认方法（default修饰）。

## 比较
- 从设计层面上看，抽象类提供了一种 IS-A 关系，那么就必须满足里式替换原则，即子类对象必须能够替换掉所有父类对象。而接口更像是一种 LIKE-A 关系，它只是提供一种方法实现契约，并不要求接口和实现接口的类具有 IS-A 关系。
- 从使用上来看，一个类可以实现多个接口，但是不能继承多个抽象类。
- 接口的字段只能是 static 和 final 类型的，而抽象类的字段没有这种限制。
- 接口的方法只能是 public 的，而抽象类的方法可以有多种访问权限。

# 重写与重载
- 重写（Override）存在于继承体系中，指子类实现了一个与父类在方法声明上完全相同的一个方法；
- 重载（Overload）存在于同一个类中，指一个方法与已经存在的方法名称上相同，但是参数类型、个数、顺序至少有一个不同。应该注意的是，返回值不同，其它都相同不算是重载。

# Object
## equals
满足自反性、对称性、传递性、一致性与null比较。实现：
- 检查是否为同一个对象的引用，如果是直接返回 true；
- 检查是否是同一个类型，如果不是，直接返回 false；
- 将 Object 实例进行转型；
- 判断每个关键域是否相等。

## hashCode
hasCode() 返回散列值，而 equals() 是用来判断两个实例是否等价。等价的两个实例散列值一定要相同，但是散列值相同的两个实例不一定等价。**在覆盖 equals() 方法时应当总是覆盖 hashCode() 方法**，保证等价的两个实例散列值也相等。

> 理想的散列函数应当具有均匀性，即不相等的实例应当均匀分布到所有可能的散列值上。这就要求了散列函数要把所有域的值都考虑进来，可以将每个域都当成 R 进制的某一位，然后组成一个 R 进制的整数。R 一般取 31，因为它是一个奇素数，如果是偶数的话，当出现乘法溢出，信息就会丢失，因为与 2 相乘相当于向左移一位。
> 一个数与 31 相乘可以转换成移位和减法：31*x == (x<<5)-x，编译器会自动进行这个优化。

## toString
默认返回 ToStringExample@4554617c 这种形式，其中 @ 后面的数值为散列码的无符号十六进制表示。

## clone()
应该注意的是，clone() 方法并不是 Cloneable 接口的方法，而是 Object 的一个 protected 方法。Cloneable 接口只是规定，如果一个类没有实现 Cloneable 接口又调用了 clone() 方法，就会抛出 CloneNotSupportedException。

### 深拷贝与浅拷贝
- 浅拷贝：拷贝实例和原始实例的引用类型引用同一个对象；
- 深拷贝：拷贝实例和原始实例的引用类型引用不同对象。

> 使用 clone() 方法来拷贝一个对象即复杂又有风险，它会抛出异常，并且还需要类型转换。Effective Java 书上讲到，最好不要去使用 clone()，可以使用拷贝构造函数或者拷贝工厂来拷贝一个对象。

# 关键字
- final，修饰变量、方法和类。
- static，修饰变量、方法和内部类，与实例对象无关，与类对象相连。


# 反射




# 泛型
泛型是一种Java提供的参数类型的方法，通过将类型以参数的方式传递给类，主要有两个好处：
1. 能够提供编译期的类型检查，保证传递类型安全；
2. 减少代码中的强制类型转换；

JAVA泛型的原理是通过类型擦除的方式实现，主要原因在于兼容非泛型的代码（二进制字节码）能够在支持泛型的虚拟机中正常运行，应该通过类型擦除方式，是否存在泛型的代码之间基本无差别（泛型代码，只是通过chechcast字节码指令进行强制转换）。

- [泛型的内部原理](https://blog.csdn.net/u010842515/article/details/63252717)


# 异常
Java所有异常类父类是Throwable，表示能够被抛出和捕获。其中分为Exception和Error。Error表示严重的问题，属于不应该被捕获异常（unchecked），例如JVM发生异常，一般是不可恢复的；Exception分为两种，一种是继承自RuntimeException，属于unchecked Exception，编译器不要求强制捕获，是否捕获看具体情况，一般是程序逻辑错误（NullPointerException/ArrayIndexOutOfBoundsException）；另一种是checked Exception（IO Exception），编译器要求强制捕获。

# 注解
Annotation，


# 内部类
内部类访问外部类的同名属性会发生隐藏现象。

- [内部类详解](https://www.cnblogs.com/dolphin0520/p/3811445.html)





# Reference
- [Java基础](https://github.com/CyC2018/Interview-Notebook/blob/master/notes/Java%20%E5%9F%BA%E7%A1%80.md#clone)







