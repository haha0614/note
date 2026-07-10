# JVM

## JVM是什么?

<font color='red'>J</font>ava <font color='red'>V</font>irtual <font color='red'>M</font>achine: Java程序的运行环境(Java二进制字节码的运行环境)

好处: 

- 一次编写, 到处运行
- 自动内存管理, 垃圾回收机制

![image-20240914094604301](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914094604301.png)

## JVM由哪些部分组成, 运行流程是什么?

![image-20240914094657900](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914094657900.png)

## 学习目标

- JVM组成
- 类加载器
- 垃圾回收
- JVM实践

![image-20240914094947489](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914094947489.png)

+++

# JVM组成

## 程序计数器

### 什么是程序计数器

程序计数器: 线程私有的, 内部保存的字节码的行号. 用于记录正在执行的字节码指令单地址.

![image-20240914095127985](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914095127985.png)

> `javap -v xx.class` 打印堆栈大小, 局部变量的数量和方法的参数

测试代码:

```java
package com.qqzj.jvm;

public class Application {
    public static void main(String[] args) throws InterruptedException {
        System.out.println("hello world");
    }
}
```

![image-20240914095854033](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914095854033.png)

首先编译一下当前类, 获取该类的`class`文件

![image-20240914095951591](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914095951591.png)

![image-20240914100022831](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914100022831.png)

右键点击, 选择打开于终端

输入`javap -v Application.class`

![image-20240914100141662](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914100141662.png)

回车:

![image-20240914100258445](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914100258445.png)

主要看这里的`main`方法

- `0: getstatic`: 获取一个静态变量, 也就是`System.out`并且这个变量的类型是`PrintStream`
- `3: ldc`: 加载一个常量, 也就是String字符串`hello world`
- `5: invokevirtual`: 表明要调用一个方法, 也就是`println`方法
- `8: return`: 结束了这个方法

### 总结

![image-20240914100934739](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914100934739.png)

**什么是程序计数器?**

线程私有的, 每个线程一份, 内部保存的是字节码的行号. 用于记录正在执行的字节码指令的地址

## Java堆

<font color='red'>线程共享的区域</font>: 主要用来保存<font color='red'>对象实例</font>, <font color='red'>数组</font>等, 当堆中没有内存空间可分配给实例, 也无法再扩展时, 则抛出`OutOfMemoryError`异常

![image-20240914101238583](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914101238583.png)

<font color='red'>**元空间**</font>保存的是类的信息、静态变量、常量、编译后的代码.

<font color='red'>**年轻代**</font>被划分为三部分, `Eden`区和两个大小严格相同的`Survivor`区, 根据JVM的策略, 在经过几次垃圾收集后,仍然存活于`Survivor`的对象将被移动到老年代区间.

<font color='red'>**老年代**</font>主要保存生命周期长的对象, 一般是一些老的对象

### Java1.7和1.8堆的区别

![image-20240914101610981](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914101610981.png)

<font color='red'>**避免OOM**</font>

### 总结

**你能给我详细的介绍Java堆吗?**

- <font color='red'>线程共享的区域</font>: 主要用来保存<font color='red'>对象实例</font>, <font color='red'>数组</font>等, 内存不够则抛出`OutOfMemoryError`异常
- 组成: <font color='red'>年轻代</font> + <font color='red'>老年代</font>
  - <font color='red'>年轻代</font>被划分为三部分, `Eden`区和两个大小严格相同的`Survivor`区
  - <font color='red'>老年代</font>主要保存生命周期长的对象, 一般是一些老的对象
- JDK1.7和1.8堆的区别
  - 1.7中堆中有一个永久代, 存储的是类信息、静态变量、常量、编译后的代码
  - 1.8在堆中移除了永久代, 把数据存储到了本地内存的元空间中, 防止内存溢出

## 虚拟机栈

### 什么是虚拟机栈

Java Virtual Machine Stacks(Java虚拟机栈)

- 每个线程运行时所需要的内存, 称为虚拟机栈, 先进后出
- 每个栈由多个栈帧(frame)组成, 对应着每次方法调用时所占用的内存
- 每个线程只能有一个活动栈帧, 对应着当前正在执行的那个方法

![image-20240914105807798](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914105807798.png)

![image-20240914105934393](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914105934393.png)

![image-20240914105945965](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914105945965.png)

1. **垃圾回收是否涉及栈内存?**

   垃圾回收主要指的是堆内存, 当栈帧弹栈以后, 内存就会释放

2. **栈内存分配越大越好吗?**

   未必, 默认的栈内存通常为1024K

   栈帧过大会导致线程数变少, 例如, 机器总内存为512m, 目前能过冬的线程数则为512个, 如果把栈内存改为2048k, 那么能活动的栈帧就会减半

3. **方法内的局部变量是否线程安全?**

   ![image-20240914110320157](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914110320157.png)

   - 如果方法内局部变量没有逃离方法的作用范围, 它是线程安全的
   - 如果是局部变量引用了对象, 并逃离方法的作用范围, 需要考虑线程安全

### 栈内存溢出(`java.lang.StackOverflowError`)情况

- 栈帧过多导致栈内存溢出, 典型问题: 递归调用
- 栈帧过大导致栈内存溢出

### 总结

1. **什么是虚拟机栈?**

   - 每个线程运行时所需要的内存, 称为虚拟机栈
   - 每个栈由多个栈帧(frame)组成, 对应着每次方法调用时所占用的内存
   - 每个线程只能有一个活动栈帧, 对应着当前正在执行的那个方法

2. **垃圾回收是否涉及栈内存?**

   垃圾回收主要指的是堆内存, 当栈帧弹栈以后, 内存就会释放

3. **栈内存分配越大越好吗?**

   未必, 默认的栈内存通常为1024k, 栈帧过大会导致线程数变少

4. **方法内的局部变量是否线程安全?**

   - 如果方法内局部变量没有逃离方法的作用范围, 它是线程安全的
   - 如果局部变量引用了对象, 并逃离方法的作用范围, 需要考虑线程安全

5. **什么情况下会导致栈内存溢出?**

   - 栈帧过多导致栈内存溢出, 典型问题: 递归调用
   - 栈帧过大导致栈内存溢出

6. **堆栈的区别是什么?**

   - 栈内存一般会用来存储局部变量和方法调用, 但堆内存是用来存储Java对象和数组的. 堆会被GC垃圾回收, 而栈不会

   - 栈内存是线程私有的, 而堆内存是线程共享的

   - 两者异常错误不同, 但如果栈内存或者堆内存不足都会抛出异常

     栈空间不足: `java.lang.StackOverFlowError`

     堆空间不足: `java.lang.OutOfMemoryError`

## 方法区

![image-20240914095127985](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914095127985.png)

### 解释一下方法区

- 方法区(Method Area)是各个线程<font color='red'>共享的内存区域</font>
- 主要存储类的信息, 运行时常量池
- 虚拟机启动的时候创建, 关闭虚拟机时释放
- 如果方法区域中的内存无法满足分配请求, 则会抛出`OutOfMemoryError: Metaspace`

![image-20240914111638556](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914111638556.png)

测试代码:

```java
package com.qqzj.jvm;

import jdk.internal.org.objectweb.asm.ClassWriter;
import jdk.internal.org.objectweb.asm.Opcodes;

/**
 * 演示元空间内存溢出 java.lang.OutOfMemoryError: Metaspace
 * -XX:MaxMetaspaceSize=8m
 */
public class MetaspaceDemo extends ClassLoader { // 可以用来加载类的二进制字节码
    public static void main(String[] args) {
        for (int i = 0; i < 10; i++) {
            int finalI = i;
            new Thread(new Runnable(){
                @Override
                public void run() {
                    System.out.println("thread"+ finalI +" start");
                    MetaspaceDemo test = new MetaspaceDemo();
                    for (int i = 0; i < 1000000; i++) {
                        // ClassWriter 作用是生成类的二进制字节码
                        ClassWriter cw = new ClassWriter(0);
                        // 版本号， public， 类名, 包名, 父类， 接口
                        cw.visit(Opcodes.V1_8, Opcodes.ACC_PUBLIC, "Class" + i, null, "java/lang/Object", null);
                        // 返回 byte[]
                        byte[] code = cw.toByteArray();
                        // 执行了类的加载
                        test.defineClass("Class" + i, code, 0, code.length); // Class 对象
                    }
                }
            }, "thread" + i).start();
        }
    }
}
```

在启动配置上添加 VM参数 : `-XX:MaxMetaspaceSize=200m`

![image-20240914113146023](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914113146023.png)

### 常量池

可以看做是一张表, 虚拟机指令根据这张常量表找到要执行的类名、方法名、参数类型、字面量等信息

可以通过`javap -v XX.class`查看字节码结构(类的基本信息、常量池、方法定义)

类的基本信息:

![image-20240914113739993](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914113739993.png)

![image-20240914113919699](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914113919699.png)

常量池: 

```tes
Constant pool:
   #1 = Methodref          #6.#22         // java/lang/Object."<init>":()V
   #2 = Fieldref           #23.#24        // java/lang/System.out:Ljava/io/PrintStream;
   #3 = String             #25            // hello world
   #4 = Methodref          #26.#27        // java/io/PrintStream.println:(Ljava/lang/String;)V
   #5 = Class              #28            // com/qqzj/jvm/Application
   #6 = Class              #29            // java/lang/Object
   #7 = Utf8               <init>
   #8 = Utf8               ()V
   #9 = Utf8               Code
  #10 = Utf8               LineNumberTable
  #11 = Utf8               LocalVariableTable
  #12 = Utf8               this
  #13 = Utf8               Lcom/qqzj/jvm/Application;
  #14 = Utf8               main
  #15 = Utf8               ([Ljava/lang/String;)V
  #16 = Utf8               args
  #17 = Utf8               [Ljava/lang/String;
  #18 = Utf8               Exceptions
  #19 = Class              #30            // java/lang/InterruptedException
  #20 = Utf8               SourceFile
  #21 = Utf8               Application.java
  #22 = NameAndType        #7:#8          // "<init>":()V
  #23 = Class              #31            // java/lang/System
  #24 = NameAndType        #32:#33        // out:Ljava/io/PrintStream;
  #25 = Utf8               hello world
  #26 = Class              #34            // java/io/PrintStream
  #27 = NameAndType        #35:#36        // println:(Ljava/lang/String;)V
  #28 = Utf8               com/qqzj/jvm/Application
  #29 = Utf8               java/lang/Object
  #30 = Utf8               java/lang/InterruptedException
  #31 = Utf8               java/lang/System
  #32 = Utf8               out
  #33 = Utf8               Ljava/io/PrintStream;
  #34 = Utf8               java/io/PrintStream
  #35 = Utf8               println
  #36 = Utf8               (Ljava/lang/String;)V
```

![image-20240914114150079](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914114150079.png)

![image-20240914115903479](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914115903479.png)

![image-20240914120256917](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914120256917.png)

![image-20240914120657048](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914120657048.png)

![image-20240914120742519](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914120742519.png)

### 运行时常量池

常量池是`*.class`文件中的, 当该类被加载, 它的常量池信息就会<font color='red'>放入运行时常量池</font>, 并把里面的<font color='red'>符号地址变为真实地址</font>

![image-20240914121114783](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914121114783.png)

### 总结

1. **能不能解释一下方法区?**
   - 方法区(Method Area)是各个线程<font color='red'>共享的内存区域</font>
   - 主要存储类的信息、运行时常量池
   - 虚拟机启动的时候创建, 关闭虚拟机时释放
   - 如果方法区域中的内存无法满足分配请求, 则会抛出`OutOfMemoryError: Metaspace`
2. **介绍一下运行时常量池**
   - 常量池: 可以看做是一张表, 虚拟机指令根据这张常量表找到要执行的类名、方法名、参数类型、字面量等信息
   - 当类被加载, 它的常量池信息就会<font color='red'>放入运行时常量池</font>, 并把里面的<font color='red'>符号地址变为真实地址</font>

## 直接内存

### 直接内存

直接内存: 并不属于JVM中的内存结构, 不由JVM进行管理. 是虚拟机的系统内存, 常见于`NIO`操作时, 用于数据缓冲区, 它分配回收成本较高, 但读写性能高

![image-20240914144653527](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914144653527.png)

实例代码:

```java
package com.qqzj.jvm;

import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.nio.ByteBuffer;
import java.nio.channels.FileChannel;

/**
 * 演示 ByteBuffer 作用
 */
public class DirectMemoryDemo {
    static final String FROM = "D:\\AAAStudy\\MP4\\序列 02.mp4";
    static final String TO = "D:\\AAAStudy\\MP\\序列 02.mp4";
    static final int _1Mb = 1024 * 1024;

    public static void main(String[] args) {
        io();  //256.8563
        directBuffer();//63.2449
    }

    private static void directBuffer() {
        long start = System.nanoTime();
        try (FileChannel from = new FileInputStream(FROM).getChannel();
             FileChannel to = new FileOutputStream(TO).getChannel();
            ) {
            ByteBuffer bb = ByteBuffer.allocateDirect(_1Mb);
            while (true) {
                int len = from.read(bb);
                if (len == -1) {
                    break;
                }
                bb.flip();
                to.write(bb);
                bb.clear();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
        long end = System.nanoTime();
        System.out.println("directBuffer 用时：" + (end - start) / 1000_000.0);
    }

    private static void io() {
        long start = System.nanoTime();
        try (FileInputStream from = new FileInputStream(FROM);
             FileOutputStream to = new FileOutputStream(TO);
            ) {
            byte[] buf = new byte[_1Mb];
            while (true) {
                int len = from.read(buf);
                if (len == -1) {
                    break;
                }
                to.write(buf, 0, len);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
        long end = System.nanoTime();
        System.out.println("io 用时：" + (end - start) / 1000_000.0);
    }
}
```

![image-20240914145021591](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914145021591.png)

### 常规IO的数据拷贝流程

![image-20240914145203534](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914145203534.png)

### NIO数据拷贝流程

![image-20240914145431869](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914145431869.png)

### 总结

**你听过直接内存吗?**

- 并不属于JVM中的内存结构, 不由JVM进行管理, 是虚拟机的系统内存
- 常见于NIO操作时, 用于数据缓冲区, 分配回收成本较高, 但读写性能高, 不受JVM内存回收管理

+++

# 类加载器

## 类加载器

![image-20240914094657900](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914094657900.png)

- **<font color='red'>类加载器: 用于装在字节码文件(.class文件)</font>**
- 运行时数据区: 用于分配存储空间
- 执行引擎: 执行字节码文件或本地方法
- 垃圾回收期: 用于对JVM中的垃圾内容进行回收

### 什么是类加载器, 类加载器有哪些

<font color='red'>**类加载器**</font>

JVM只会运行二进制文件, 类加载器的作用就是将<font color='red'>字节码文件加载到JVM中</font>, 从而让Java程序能够启动起来.

![image-20240914150027448](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914150027448.png)

### 总结

1. **什么是类加载器**

   JVM只会运行二进制文件, 类加载器的作用就是将<font color='red'>字节码文件加载到JVM中</font>, 从而让Java程序能够启动起来.

2. **类加载器有哪些**
   - 启动类加载器(BootStrap ClassLoader): 加载`JAVA_HOME/jre/lib`目录下的库
   - 扩展类加载器(ExtClassLoader): 主要加载`JAVA_HOME/jre/lib/ext`目录中的类
   - 应用类加载器(AppClassLoader): 用于加载`classPath`下的类
   - 自定义类加载器(CustomizeClassLoader): 自定义类继承`ClassLoader`, 实现自定义类加载规则

### 什么是双亲委派模型

加载某一个类, 先委托上一级的加载器进行加载, 如果上级加载器也有上级, 则会继续向上委托, 如果该类委托上级没有被加载, 子加载器尝试加载该类

### JVM为什么采用双亲委派机制

1. 通过双亲委派机制可以避免某一个类被重复加载, 当父类已经加载后则无需重复加载, 保证唯一性.

2. 为了安全, 保证类库API不会被修改

   ```java
   package java.lang;
   
    public class String {
   
      public static void main(String[] args) {
        System.out.println("demo info");
      }
    }
   ```

   此时执行`main`函数, 会出现异常, 在类`java.lang.String`中找不到`main`方法

   ![image-20240914151017240](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914151017240.png)

   <font color='red'>由于是双亲委派机制, `java.lang.String`在启动类加载器得到加载, 因为在核心`jre`库中有其相同名字的类文件, 但该类中并没有`main`方法. 这样就能防止恶意篡改核心API库</font>

### 总结

1. **什么是双亲委派模型?**

   加载某一个类, 先委托上一级的加载器进行加载, 如果上级加载器也有上级, 则会继续向上委托, 如果该类委托上级没有被加载, 子加载器尝试加载该类

   ![image-20240914151254536](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914151254536.png)

2. **JVM为什么采用双亲委派模型?**
   - 通过双亲委派机制可以避免某一个类被重复加载, 当父类已经加载后则无需重复加载, 保证唯一性
   - 为了安全, 保证类库API不会被修改

## 类装载的执行过程

### 说一下类装载的执行过程

类从加载到虚拟机中开始, 直到卸载为止, 它的整个生命周期包括了: 加载、验证、准备、解析、初始化、使用和卸载这7个阶段. 其中, 验证、准备和解析这三个部分统称为连接(linking)

![image-20240914151601048](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914151601048.png)

### 加载

![image-20240914151630514](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914151630514.png)

- 通过类的全名, 获取类的二进制数据流
- 解析类的二进制数据流为方法区内的数据结构(Java类模型)
- 创建`java.lang.Class`类的实例, 表示该类型. 作为方法区这个类的各个数据的访问入口

![image-20240914151752101](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914151752101.png)

### 验证

![image-20240914152223470](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914152223470.png)

<font color='red'>**验证类是否符合JVM规范, 安全性检查**</font>

1. 文件格式验证
2. 元数据验证
3. 字节码验证

以上三项属于格式检查, 如: 文件格式是否错误、语法是否错误、字节码是否合规

4. 符号引用验证   Class文件在其常量池会通过字符串记录自己将要使用的其他类或者方法, 检查他们是否存在

   ![image-20240914152518020](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914152518020.png)

### 准备

![image-20240914152554025](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914152554025.png)

<font color='red'>**为类变量分配内存并设置类变量初始值**</font>

- `static`变量, 分配空间在准备阶段完成(设置默认值), 赋值在初始化阶段完成
- `static`变量是`final`的基本类型, 以及字符串常量, 值已确定, 赋值在准备阶段完成
- `static`变量是`final`的引用类型, 那么赋值也会在初始化阶段完成

```java
public class Application {
    static int b = 10;
    static final int c = 20;
    static final String d = "hello";
    static final Object obj = new Object();
}
```

### 解析

![image-20240914153026056](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914153026056.png)

<font color='red'>**把类中的符号引用转换为直接引用**</font>

比如: 方法中调用了其他方法, 方法名可以理解为符号调用, 而直接引用就是使用指针直接指向方法.

![image-20240914153202912](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914153202912.png)

![image-20240914153212881](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914153212881.png)

### 初始化

![image-20240914153306920](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914153306920.png)

<font color='red'>**对类的静态变量, 静态代码块进行初始化操作**</font>

- 如果初始化一个类的时候, 其父类尚未初始化, 则优先初始化其父类.
- 如果同时包含多个静态变量和静态代码块, 则按照自上而下的顺序依次执行.

测试代码:

```java
package com.qqzj.jvm;

public class Application {

    public static void main(String[] args) throws InterruptedException {
        // 1. 首次访问这个类的静态变量或静态方法时
        // System.out.println(Animal.num);
        // 2. 子类初始化，如果父类还没初始化，会引发父类先初始化
        // System.out.println(Cat.sex);
        // 3. 子类访问父类静态变量，只触发父类初始化
        // System.out.println(Cat.num);
    }
}

class Animal {
    static int num = 55;
    static {
        System.out.println("Animal 静态代码块...");
    }
}

class Cat extends Animal {
    static boolean sex = false;
    static {
        System.out.println("Cat 静态代码块...1");
    }

    static {
        System.out.println("Cat 静态代码块...2");
    }
}
```

结果:

```java
// 1. 首次访问这个类的静态变量或静态方法时
System.out.println(Animal.num);
```

![image-20240914153853783](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914153853783.png)

```java
// 2. 子类初始化，如果父类还没初始化，会引发父类先初始化
System.out.println(Cat.sex);
```

![image-20240914153933018](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914153933018.png)

```java
// 3. 子类访问父类静态变量，只触发父类初始化
System.out.println(Cat.num);
```

![image-20240914154004496](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914154004496.png)

### 使用

![image-20240914154042140](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914154042140.png)

JVM开始从入口方法开始执行用户的程序代码

- 调用静态类成员信息(比如: 静态字段、静态方法)
- 使用`new`关键字为其创建对象实例

### 卸载

当用户程序代码执行完毕之后, JVM就开始销毁创建的Class对象了, 就相当于把类给卸载了

### 总结

**说一下类装载的执行过程**

- 加载: 查找和导入Class文件
- 验证: 保证加载类的准确性
- 准备: 为类变量分配内存并设置类变量初始值
- 解析: 把类中的符号引用转换为直接引用
- 初始化: 对类的静态变量, 静态代码块执行初始化操作
- 使用: JVM开始从入口方法开始执行用户的程序代码
- 卸载: 当用户程序代码执行完毕后, JVM便开始销毁创建的Class对象

+++

# 垃圾回收

## 对象什么时候可以被垃圾回收器回收

![image-20240914154633888](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914154633888.png)

简单一句就是: 如果一个或多个对象没有任何的引用指向它了, 那么这个对象现在就是垃圾, 如果定位了垃圾, 则有可能会被垃圾回收器回收.

如果要定位什么是垃圾, 有两种方式来确定, 第一个是<font color='red'>引用计数法</font>, 第二个是<font color='red'>可达性分析算法</font>

### 引用计数法

一个对象被引用了一次, 在当前的对象头上就递增一次引用次数, 如果这个对象的引用次数为0, 代表这个对象可回收

![image-20240914155118587](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914155118587.png)

当对象间出现了循环引用的话, 引用计数法就会失效

![image-20240914155242380](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914155242380.png)

![image-20240914155253678](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914155253678.png)

![image-20240914155359132](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914155359132.png)

![image-20240914155422629](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914155422629.png)

<font color='red'>**循环引用, 会引发内存泄漏**</font>

### 可达性分析算法

现在的虚拟机采用的都是通过可达性分析算法来确定哪些内容是垃圾

![image-20240914155607591](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914155607591.png)

<font color='red'>X, Y这两个节点是可回收的</font>

- Java 虚拟机中的垃圾回收器采用可达性分析来探索所有存活的对象
- 扫描堆中的对象, 看是否能够沿着 `GC Root` 对象为起点的引用链找到该对象, 找不到, 表示可以回收

### 哪些对象可以作为`GC Root`?

- 虚拟机栈(栈帧中的本地变量表)中引用的对象

  ```java
  public static void main(String[] args) {
      Demo demo = new Demo();
      demo = null;
  }
  ```

- 方法区中类静态属性引用的对象

  ```java
  public static Demo a;
  public static void main(String[] args) {
      Demo b = new Demo();
      b.a = new Demo();
      b = null;
  }
  ```

- 方法区中常量引用的对象

  ```java
  public static final Demo a = new Demo();
  public static void main(String[] args) {
      Demo demo = new Demo();
      demo = null;
  }
  ```

- 本地方法栈中 `JNI`(即一般说的`Native`方法)引用的对象

### 总结

**对象什么时候可以被垃圾回收器回收?**

如果一个或多个对象没有任何的引用指向它了, 那么这个对象现在就是垃圾

如果定位了垃圾, 则有可能会被垃圾回收器回收



定位垃圾的方式有两种

- 引用计数法
- <font color='red'>可达性分析算法</font>

## JVM垃圾回收算法

### 有哪些?

- 标记清除算法
- 复制算法
- 标记整理算法

### 标记清除算法

标记清除算法, 是将垃圾回收分为2个阶段, 分别是<font color='red'>标记</font>和<font color='red'>清除</font>

1. 根据可达性分析算法得出的垃圾进行标记
2. 对这些标记为可回收的内容进行垃圾回收

![image-20240914161008984](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914161008984.png)

![image-20240914161023069](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914161023069.png)

优点: 标记和清除速度较快

缺点: 碎片化较为严重, 内存不连贯的

### 标记整理算法

![image-20240914161142463](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914161142463.png)

![image-20240914161156391](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914161156391.png)

优缺点同标记清除算法, 解决了标记清除算法的碎片化的问题, 同时, 标记压缩算法多了一步, 对象移动内存位置的步骤, 其效率也有一定的影响

### 复制算法

![image-20240914161312647](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914161312647.png)

![image-20240914161328957](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914161328957.png)

优点:

- 在垃圾对象多的情况下, 效率较高
- 清理后, 内存无碎片

缺点:

- 分配的两块内存空间, 在同一时刻, 只能使用一半, 内存使用率较低

### 总结

**JVM垃圾回收算法有哪些?**

- <font color='red'>标记清除算法</font>: 垃圾回收分为两个阶段, 分别是标记和清除, 效率高, 有磁盘碎片, 内存不连续
- <font color='red'>标记整理算法</font>: 标记清除算法一样, 将存活对象都向内存另一端移动, 然后清理边界以外的垃圾, 无碎片, 对象需要移动, 效率低
- <font color='red'>复制算法</font>: 将原有的内存空间一分为二, 每次只用其中的一块, 正在使用的对象复制到另一个内存空间中, 然后将该内存空间清空, 交换两个内存的角色, 完成垃圾的回收: 无碎片, 内存使用率低

## JVM的分代回收

### 分代收集算法

在Java8时, 对被分成了两份: <font color='red'>新生代和老年代 (1 : 2)</font>

![image-20240914161920353](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914161920353.png)

对于新生代, 内部又被分为了三个区域

- 伊甸园区`Eden`, 新生的对象都分配到这里
- 幸存者区`Survivor`(分为`from`和`to`)
- `Eden`区, `from`区, `to`区 (8: 1: 1)

### 工作机制

- 新创建的对象, 都会先分配到`Eden`区

  ![image-20240914162144339](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914162144339.png)

- 当伊甸园区内存不足, 标记伊甸园与`from`(现阶段没有)的存活对象

- 将存活对象采用复制算法复制到`to`中, 复制完毕后, 伊甸园区和`from`区内存都得到释放

  ![image-20240914162320036](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914162320036.png)

- 经过一段时间后伊甸园区的内存又出现不足, 标记`Eden`区域`to`区存活的对象, 将存活的对象复制到`from`区

  ![image-20240914162426983](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914162426983.png)

  ![image-20240914162455342](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914162455342.png)

- 当幸存者区对象熬过几次回收(最多15次), 晋升到老年代(幸存者区内存不足或大对象会导致提前晋升)

  ![image-20240914162632850](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914162632850.png)

  ![image-20240914162652382](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914162652382.png)

### `MinorGC`、`Mixed GC`、`FullGC`的区别是什么

![image-20240914162802471](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914162802471.png)

- **Minor GC**(**Young GC**)发生在新生代的垃圾回收, 暂停时间短(STW)
- **Mixed GC**: 新生代 + 老年代 部分区域的垃圾回收, G1收集器特有
- **Full GC**: 新生代 + 老年代 完整垃圾回收, 暂停时间长(STW), 应尽力避免

> <font color='red'>*STW*</font>(Stop-The-World): 暂停所有应用程序线程, 等待垃圾回收的完成

### 总结

**说一下JVM中的分代回收**

1. 堆的区域划分
   1. 堆被分为了两份: 新生代和老年代(1: 2)
   2. 对于新生代, 内部有被分为了三个区域. `Eden`区, 幸存者区`Survivor`(分为`from`和`to`) (8: 1: 1)
2. 对象回收分代回收策略
   1. 新创建的对象, 都会先分配到`Eden`区
   2. 当伊甸园内存不足, 标记伊甸园与`from`(现阶段没有)的存活对象
   3. 将存活对象采用复制算法复制到`to`中, 复制完毕后, 伊甸园和`from`内存都得到释放
   4. 经过一段时间后伊甸园的内存又出现不足, 标记`eden`区和`to`区存活的对象, 将其复制到`from`区
   5. 当幸存者区对象熬过几次回收(最多15次), 晋升到老年代(幸存者区内存不足或大对象会提前晋升)

MinorGC、MixedGC、FullGC的区别是什么

- MinorGC(YoungGC)发生在新生代的垃圾回收, 暂停时间短(STW)
- MixedGC: 新生代 + 老年代 部分区域的垃圾回收, G1 收集器特有
- FullGC: 新生代 + 老年代 完整垃圾回收, 暂停时间长(STW), 应尽力避免

## JVM垃圾回收器

### JVM有哪些垃圾回收器

在JVM中, 实现了多种垃圾收集器, 包括:

- 串行垃圾收集器
- 并行垃圾收集器
- CMS(并发)垃圾收集器
- G1垃圾收集器

### 串行垃圾回收器

`Serial`和`Serial Old`串行垃圾收集器, 是指使用单线程进行垃圾回收, 堆内存较小, 适合个人电脑

- `Serial`作用域新生代, 采用复制算法
- `Serial Old`作用域老年代, 采用标记-整理算法

垃圾回收时, 只有一个线程在工作, 并且Java应用中的所有线程都要暂停(STW), 等待垃圾回收的完成

![image-20240914164409341](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914164409341.png)

### 并行垃圾回收器

`Parallel New`和`Parallel Old`是一个<font color='red'>并行</font>垃圾回收器, <font color='red'>**JDK8默认使用此垃圾回收器**</font>

- `Parallel New`作用于新生代, 采用复制算法
- `Parallel Old`作用于老年代, 采用标记-整理算法

垃圾回收时, 多个线程在工作, 并且Java应用中的所有线程都要暂停(STW), 等待垃圾回收的完成

![image-20240914164649353](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914164649353.png)

### CMS(并发)垃圾回收器

CMS全称Concurrent Mark Sweep, 是一款<font color='red'>并发</font>的、使用<font color='red'>标记-清除</font>算法的垃圾回收器, 该回收器是<font color='red'>针对老年代垃圾回收的</font>, 是一款以获取最短回收停顿时间为目标的收集器, 停顿时间短, 用户体验好. 其最大特点是在进行垃圾回收时, 应用仍然能正常运行

![image-20240914165305827](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914165305827.png)

![Snipaste_2024-09-14_16-55-50](https://gitee.com/w-d1107/typora-image/raw/master/Snipaste_2024-09-14_16-55-50.png)

在并发标记的时候, 可能出现, 原本X并没有被关联, 但是在并发标记的时候, 又关联上了, 或者原本对象D关联了, 但是并发标记的时候, 又取消关联了, 所以这时候就需要**重新标记**一次

### 总结

**说一下JVM有哪些垃圾回收器?**

在JVM中, 实现了多种垃圾回收器, 包括:

- 串行垃圾回收器: `Serial GC`、`Serial Old GC`
- 并行垃圾回收器: `Parallel Old GC`、`Parallel New GC`
- CMS(并发)垃圾回收器: CMS GC, 作用在老年代
- G1垃圾回收器: 作用在新生代和老年代

## G1垃圾回收器

- 应用于新生代和老年代, <font color='red'>**在JDK9之后默认使用G1**</font>
- 划分成多个区域, 每个区域都可以充当 `Eden`, `Survivor`, `old`, `humongous`, 其中`humongous`专门为大对象准备
- 采用复制算法
- 响应时间和吞吐量兼顾
- 分为三个阶段: 新生代回收、并发标记、混合回收
- 如果并发失败(即回收速度赶不上创建新对象的速度), 会触发`FullGC`

![image-20240914170455609](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914170455609.png)

### Young Collection(年轻代垃圾回收)

- 初始时, 所有区域都处于空闲状态

  ![image-20240914170740705](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914170740705.png)

- 创建了一些对象, 挑出一些空闲区域作为伊甸园区存储这些对象

  ![image-20240914170729563](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914170729563.png)

- 当伊甸园区需要垃圾回收时, 挑出一个空闲区域作为幸存者区, 用复制算法复制存活对象, 需要暂停用户线程

  ![image-20240914170843089](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914170843089.png)

  > 在G1中, 新生代的内存占比不是固定的, 是在5%~6%之间波动, 但不管怎么波动, 内存都是限制的, 都会限制伊甸园区不能随意的创建新对象
  
  ![image-20240914171234978](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914171234978.png)

- 随着时间流逝, 伊甸园区的内存又有不足

  ![image-20240914171349563](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914171349563.png)

- 将伊甸园区一级之前幸存者区中的存活对象, 采用复制算法, 复制到新的幸存者区, 其中较老对象晋升至老年代

  ![image-20240914171422401](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914171422401.png)

  ![image-20240914171451998](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914171451998.png)

### Young Collection + Concurrent Mark(年轻代垃圾回收 + 并发标记)

当老年代占用内存超过阈值(默认是45%)后, 触发并发标记, 这时无需暂停用户线程

![image-20240914171602171](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914171602171.png)

- 并发标记之后, 会有重新标记阶段解决漏标问题, 此时需要暂停用户线程

- 这些都完成后就知道了老年代有哪些存活对象, 随后进入混合回收阶段. 此时不会对所有老年代区域进行回收, 而是根据<font color='red'>暂停时间目标</font>优先回收价值高(存活对象少)的区域(这也是Gabage First名称的由来)

  ![image-20240914200941972](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914200941972.png)

  > 内部会有一个预期暂停时间, 回收的暂停时间不能超过这个值

### Mixed Collection(混合垃圾回收)

混合收集阶段中, 参与复制的有`Eden`、`Survivor`、`Old`

![image-20240914201251785](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914201251785.png)

复制完成, 内存得到释放. 进入下一轮的新生代回收、并发标记、混合回收

![image-20240914201612380](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914201612380.png)

### 总结

**详细聊一下G1垃圾回收器**

- 应用于新生代和老年代, <font color='red'>**在JDK9之后默认使用G1**</font>
- 划分成多个区域, 每个区域都可以充当`Eden`、`Survivor`、`Old`、`Humongous`, 其中`Humongous`专为大对象准备
- 采用复制算法
- 响应时间与吞吐量兼顾
- 分成三个阶段: 新生代回收(STW)、并发标记(重新标记STW)、混合收集
- 如果并发失败(即回收速度赶不上创建新对象速度), 会触发`FullGC`

## 强引用、软引用、弱引用、虚引用 区别

- <font color='red'>**强引用**</font>: 只有所有`GC Roots`对象都不通过(强引用)引用该对象, 该对象才能被垃圾回收

  ```java
  User user = new User();
  ```

  ![image-20240914202320882](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914202320882.png)

- <font color='red'>**软引用**</font>: 仅有软引用引用该对象时, 在垃圾回收后, 内存仍然不足时会再次触发垃圾回收

  ```java
  User user = new User();
  SoftReference softReference = new SoftReference(user);
  ```

  ![image-20240914202503577](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914202503577.png)

- <font color='red'>**弱引用**</font>: 仅有弱引用引用该对象时, 在垃圾回收时, 无论内存是否充足, 都会回收弱引用对象

  ```java
  User user - new User();
  WeakReference weakReference = new WeakReference(user);
  ```

  ![image-20240914202724148](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914202724148.png)

  延伸话题: ThreadLocal内存泄漏问题

  ```java
  static class Entry extends WeakReference<ThreadLocal<?>> {
      Object value;
  
      Entry(ThreadLocal<?> k, Object v) {
          super(k);
          value = v; //强引用，不会被回收
      }
  }
  ```

- <font color='red'>**虚引用**</font>: 必须配合引用队列使用, 被引用对象回收时, 会将虚引用入队, 由`Reference Handler`线程调用虚引用相关方法释放直接内存

  ```java
  User user = new User();
  ReferenceQueue referenceQueue = new ReferenceQueue();
  PhantomReference phantomReference = new PhantomReference(user,queue);
  ```

  ![image-20240914203054442](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914203054442.png)

  <font color='red'>弱引用和软引用也可以通过引用队列来释放自己的资源</font>

### 总结

**强引用、软引用、弱引用、虚引用的区别?**

- **强引用**: 只要任意一个`GC Roots`能找到, 就不会被回收
- **软引用**: 需要配合`SoftReference`使用, 当垃圾多次回收, 内存依然不够的时候会回收软引用对象
- **弱引用**: 需要配合`WeakReference`使用, 只要进行了垃圾回收, 就会把弱引用对象回收
- **虚引用**: 必须配合引用队列使用, 被引用对象回收时, 会将虚引用加入到队列中, 由`Reference Handler`线程调用虚引用相关方法释放直接内存

+++

# JVM实践

## JVM调优的参数可以在哪里设置

- war包部署在Tomcat中设置
- jar包部署在启动参数设置

### war包部署在Tomcat中设置

修改TOMCAT_HOME/bin/catalina.sh(Linux系统下, Windows是.bat结尾)文件

![image-20240914204027752](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914204027752.png)

### jar包部署在启动参数设置

通常在Linux系统下直接加参数启动SpringBoot项目

```shell
nohup java -Xms512m -Xmx1024m -jar xxxx.jar --spring.profiles.active=prod &
```

`nohup`: 用于在系统后台不挂断的运行命令, 退出终端不会影响程序的运行

参数`&`: 让命令在后台执行, 终端退出后命令仍旧执行.

### 总结

**JVM调优的参数可以在哪里设置参数值?**

- war包部署在Tomcat中设置

  修改TOMCAT_HOME/bin/catalina.sh(.bat)文件

- jar包部署在启动参数设置

  `java -Xms512m -Xmx1024m -jar xxxx.jar`

## JVM调优的参数

### 常用的JVM调优的参数都有哪些?

对于JVM调优, 主要就是调整年轻代、老年代、元空间的内存空间大小及使用的垃圾回收器类型.

https://www.oracle.com/java/technologies/javase/vmoptions-jsp.html

- 设置堆空间大小
- 虚拟机栈的设置
- 年轻代中`Eden`区和两个`Survivor`区的大小比例
- 年轻代晋升老年代阈值
- 设置垃圾回收器

### 设置堆空间大小

设置堆的初始大小和最大大小, 为了防止垃圾回收器在初始大小、最大大小之间收缩堆而产生额外的时间, 通常把最大、初始大小设置为相同的值.

```shell
-Xms：设置堆的初始化大小
-Xmx：设置堆的最大大小

-Xms:1024
-Xms:1024k
-Xms:1024m
-Xms:1g
```

不指定单位默认为字节; 指定单位, 按照指定的单位设置

堆空间设置多少合适?

- 最大大小的默认值是物理内存的1/4, 初始大小是物理内存的1/64
- 堆太小, 可能会频繁的导致年轻代和老年代的垃圾回收, 会产生STW, 暂停用户线程
- 堆内存大肯定是好的, 但是存在风险, 加入发生了FullGC, 它会扫描整个堆空间, 暂停用户线程的时间长
- 设置参考推荐: 尽量大, 也要考察一下当前计算机其他程序的内存使用情况

### 虚拟机栈的设置

虚拟机栈的设置: <font color='red'>每个线程默认会开启1M的内存</font>, 用于存放栈帧、调用参数、局部变量等, 但一般256K就够用. 通常较少每个线程的堆栈, 可以产生更多的线程, 但这实际上还受限于操作系统.

```shell
-Xss 对每个线程stack大小的调整.
-Xss128k
```

### 年轻代中`Eden`区和两个`Survivor`区的大小比例

设置年轻代中`Eden`区和两个`Survivor`区的大小比例. 该值如果不设置, 则默认比例为 8: 1: 1. 通过增大`Eden`区的大小, 来减少YGC发生的次数, 但有时我们发现, 虽然次数减少了, 但`Eden`区满的时候, 由于占用的空间较大, 导致释放缓慢, 此时STW的时间较长, 因此需要按照程序情况区调优.

```shell
-XXSurvivorRatio=8, 
表示年轻代中的分配比例: 
survivor:eden = 2:8
```

`-XXSurvivorRatio`代表`Eden`区占的份数, 假如设置为3, 则分配比例为: `survivor:eden = 2:3`

### 年轻代晋升老年代阈值

```java
-XX:MaxTenuringThreshold=threshold
```

- 默认为15
- 取值范围0~15

### 设置垃圾回收器

通过增大吞吐量提高系统性能, 可以通过设置并行垃圾回收器

```shell
-XX:+UseParallelGC
-XX:+UseParallelOldGC
```

```java
-XX:+UseG1GC
```

### 总结

**常用的JVM调优的参数都有哪些?**

- 设置堆空间大小
- 虚拟机栈的设置
- 年轻代中`Eden`区和两个`Survivor`区的大小比例
- 年轻代晋升老年代阈值
- 设置垃圾回收器

## JVM调优的工具

### 说一下JVM调优的工具?

- 命令工具
  - `jps`: 进程状态信息
  - `jstack`: 查看Java进程内线程的堆栈信息
  - `jmap`: 查看堆转信息
  - `jhat`: 堆转储快照分析工具
  - `jstat`: JVM统计监测工具
- 可视化工具
  - `jconsole`: 用于堆JVM的内存, 线程, 类 的监控
  - `VisualVM`: 能够监控线程, 内存情况

### `jps`

<font color='red'>进程状态信息</font>

测试代码:

```java
package com.qqzj.jvm;

import java.util.ArrayList;
import java.util.List;

public class ToolDemo {

    public static void main(String[] args) {
        new Thread(()->{
            while (true){

            }
        },"t1").start();
        new Thread(()->{
            while (true){

            }
        },"t2").start();
        new Thread(()->{
            while (true){

            }
        },"t3").start();
    }
}
```

![image-20240914210943090](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914210943090.png)

### `jstack`

<font color='red'>查看Java进程内线程的堆栈信息</font>

```cmd
jstack [option] <pid>
```

![image-20240914211157882](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914211157882.png)

![image-20240914211302707](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914211302707.png)

### `jmap`

用于生成堆转内存快照、内存使用情况

```cmd
jmap -heap pid  # 显示Java堆的信息
```

```java
C:\Users\yuhon>jmap -heap 53280
Attaching to process ID 53280, please wait...
Debugger attached successfully.
Server compiler detected.
JVM version is 25.321-b07

using thread-local object allocation.
Parallel GC with 8 thread(s)   //并行的垃圾回收器

Heap Configuration:  //堆配置
   MinHeapFreeRatio         = 0   //空闲堆空间的最小百分比
   MaxHeapFreeRatio         = 100  //空闲堆空间的最大百分比
   MaxHeapSize              = 8524922880 (8130.0MB) //堆空间允许的最大值
   NewSize                  = 178257920 (170.0MB) //新生代堆空间的默认值
   MaxNewSize               = 2841640960 (2710.0MB) //新生代堆空间允许的最大值
   OldSize                  = 356515840 (340.0MB) //老年代堆空间的默认值
   NewRatio                 = 2 //新生代与老年代的堆空间比值，表示新生代：老年代=1：2
   SurvivorRatio            = 8 //两个Survivor区和Eden区的堆空间比值为8,表示S0:S1:Eden=1:1:8
   MetaspaceSize            = 21807104 (20.796875MB) //元空间的默认值
   CompressedClassSpaceSize = 1073741824 (1024.0MB) //压缩类使用空间大小
   MaxMetaspaceSize         = 17592186044415 MB //元空间允许的最大值
   G1HeapRegionSize         = 0 (0.0MB)//在使用 G1 垃圾回收算法时，JVM 会将 Heap 空间分隔为若干个 Region，该参数用来指定每个 Region 空间的大小。

Heap Usage:
PS Young Generation
Eden Space: //Eden使用情况
   capacity = 134217728 (128.0MB)
   used     = 10737496 (10.240074157714844MB)
   free     = 123480232 (117.75992584228516MB)
   8.000057935714722% used
From Space: //Survivor-From 使用情况
   capacity = 22020096 (21.0MB)
   used     = 0 (0.0MB)
   free     = 22020096 (21.0MB)
   0.0% used
To Space: //Survivor-To 使用情况
   capacity = 22020096 (21.0MB)
   used     = 0 (0.0MB)
   free     = 22020096 (21.0MB)
   0.0% used
PS Old Generation  //老年代 使用情况
   capacity = 356515840 (340.0MB)
   used     = 0 (0.0MB)
   free     = 356515840 (340.0MB)
   0.0% used

3185 interned Strings occupying 261264 bytes.
```

`jmap -dump:format=b,file=heap.hprof pid`

- `format=b`表示以`hprof`二进制格式转储Java堆的内存
- `file=<filename>`用于指定快照`dump`文件的文件名

> dump文件
>
> 它是一个进程或系统在某一给定的时间的快照. 比如在进程崩溃时, 甚至是任何时候, 我们都可以通过工具将系统或某进程的内存备份出来供调试分析用.
>
> dump文件中包含了程序运行的模块信息、线程信息、堆栈调用信息、异常信息等数据, 方便系统技术人员进行错误排查

![image-20240914213330676](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914213330676.png)

![image-20240914213400518](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914213400518.png)

### `jstat`

是JVM统计监测工具. 可以用来显示垃圾回收信息、类加载信息、新生代统计信息等.

1. 总结垃圾回收统计 

   ```java
   jstat -gcutil pid
   ```

   ![image-20240914213536951](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914213536951.png)

2. 垃圾回收统计

   ```java
   jstat -gc pid
   ```

   ![image-20240914213600100](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914213600100.png)

### `jconsole`

用于堆JVM的内存, 线程, 类 的监控, 是一个基于JMX的GUI性能监控工具

打开方式: java安装目录bin目录下 直接启动jconsole.exe就行

![image-20240914213756595](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914213756595.png)

### `VisualVM`

能够监控线程, 内存情况, 查看方法的CPU时间和内存中的对象, 已被GC的对象, 反向查看分配的堆栈

打开方式: java安装目录bin目录下直接启动jvisualvm.exe就行

<font color='red'>只有JDK8是自带的, 更高版本需要去官网下载</font>

![image-20240914214225580](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914214225580.png)

监控程序运行情况

![image-20240914214245910](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914214245910.png)

查看运行中的dump

Dump文件是进程的内存镜像. 可以把程序的执行状态通过调试器保存到Dump文件中

![image-20240914214334398](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914214334398.png)

![image-20240914214345089](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914214345089.png)

### 总结

**说一下JVM的调优的工具?**

命令工具

- `jps`: 进程状态信息
- `jstack`: 查看java进程内线程的堆栈信息
- `jmap`: 查看堆转信息
- `jhat`: 堆转储快照分析工具
- `jstat`: JVM统计监测工具

可视化工具

- jconsole: 用于对JVM的内存, 线程, 类的监控
- VisualVM: 能够监控线程, 内存情况

## java内存泄漏排查思路

![image-20240914215239180](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914215239180.png)

测试代码:

```java
package com.qqzj.jvm;

import java.util.ArrayList;
import java.util.List;

public class ToolDemo {

    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        while (true){
            list.add("北京");
        }
    }
}
```

![image-20240914215436787](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914215436787.png)

![image-20240914215529107](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914215529107.png)

1. 获取堆内存快照dump
2. VisualVM去分析dump文件
3. 通过查看堆信息的情况, 定位内存溢出问题

流程:

1. 通过`jmap`指令打印它的内存快照dump(Dump文件是进程的内存镜像, 可以把程序的执行状态通过调试器保存到dump文件中)

   - 使用`jmap`命令获取运行中程序的dump文件

     `jmap -dump:format=b,file=heap.hprof pid`

   - 使用vm参数获取dump文件

     有的情况是内存溢出之后程序则会直接终端, 而`jmap`只能打印在运行中的程序, 所以建议通过参数的方法生成dump文件

     ```cmd
     -XX:+HeapDumpOnOutOfMemoryError
     -XX:HeapDumpPath=***/***
     ```

2. 通过工具， VisualVM去分析dump文件，VisualVM可以加载离线的dump文件

   文件-->装入--->选择dump文件即可查看堆快照信息

   ![image-20240914220206416](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914220206416.png)

3. 通过查看堆信息的情况，可以大概定位内存溢出是哪行代码出了问题

   ![image-20240914220240282](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914220240282.png)

4. 找到对应的代码，通过阅读上下文的情况，进行修复即可

   ![image-20240914220315500](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914220315500.png)

### 总结

**java内存泄漏的排查思路?**

内存泄漏通常是指堆内存, 通常是指一些大对象不被回收的情况

1. 通过jmap或设置jvm参数获取堆内存快照dump

2. 通过工具， VisualVM去分析dump文件，VisualVM可以加载离线的dump文件

3. 通过查看堆信息的情况，可以大概定位内存溢出是哪行代码出了问题

4. 找到对应的代码，通过阅读上下文的情况，进行修复即可

## CPU飚高排查方案与思路

1. 使用`top`命令查看占用CPU的情况

   ![image-20240914220647498](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914220647498.png)

2. 通过`top`命令查看后，可以查看是哪一个进程占用cpu较高，上图所示的进程为：40940

3. 查看进程中的线程信息

   ```java
   ps H -eo pid,tid,%cpu | grep 40940
   ```

   ![image-20240914220754783](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914220754783.png)

   通过以上分析，在进程40940中的线程40950占用cpu较高

4. 可以根据线程 id 找到有问题的线程，进一步定位到问题代码的源码行号

   ```java
   jstack 40940   此处是进程id
   ```

   ![image-20240914221010650](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914221010650.png)

   ![image-20240914220930840](https://gitee.com/w-d1107/typora-image/raw/master/image-20240914220930840.png)

   十进制转换为十六进制

   ```shell
   printf "%x\n" 40955
   ```

### 总结

**CPU飙高排查方案与思路？**

1. 使用top命令查看占用cpu的情况

2. 通过top命令查看后，可以查看是哪一个进程占用cpu较高

3. 使用ps命令查看进程中的线程信息

4. 使用jstack命令查看进程中哪些线程出现了问题，最终定位问题

+++

# 面试现场

## JVM组成

>**面试官**：JVM由那些部分组成，运行流程是什么？
>
>**候选人:**
>
>嗯，好的~~
>
>在JVM中共有四大部分，分别是ClassLoader（类加载器）、Runtime Data Area（运行时数据区，内存分区）、Execution Engine（执行引擎）、Native Method Library（本地库接口）
>
>它们的运行流程是：
>
>第一，类加载器（ClassLoader）把Java代码转换为字节码
>
>第二，运行时数据区（Runtime Data Area）把字节码加载到内存中，而字节码文件只是JVM的一套指令集规范，并不能直接交给底层系统去执行，而是有执行引擎运行
>
>第三，执行引擎（Execution Engine）将字节码翻译为底层系统指令，再交由CPU执行去执行，此时需要调用其他语言的本地库接口（Native Method Library）来实现整个程序的功能。
>
>**面试官**：好的，你能详细说一下 JVM 运行时数据区吗？
>
>**候选人:**
>
>嗯，好~
>
>运行时数据区包含了堆、方法区、栈、本地方法栈、程序计数器这几部分，每个功能作用不一样。
>
>- 堆解决的是对象实例存储的问题，垃圾回收器管理的主要区域。
>- 方法区可以认为是堆的一部分，用于存储已被虚拟机加载的信息，常量、静态变量、即时编译器编译后的代码。
>- 栈解决的是程序运行的问题，栈里面存的是栈帧，栈帧里面存的是局部变量表、操作数栈、动态链接、方法出口等信息。
>- 本地方法栈与栈功能相同，本地方法栈执行的是本地方法，一个Java调用非Java代码的接口。
>- 程序计数器（PC寄存器）程序计数器中存放的是当前线程所执行的字节码的行数。JVM工作时就是通过改变这个计数器的值来选取下一个需要执行的字节码指令。
>
>**面试官**：好的，你再详细介绍一下程序计数器的作用？
>
>**候选人:**
>
>嗯，是这样~~
>
>java虚拟机对于多线程是通过线程轮流切换并且分配线程执行时间。在任何的一个时间点上，一个处理器只会处理执行一个线程，如果当前被执行的这个线程它所分配的执行时间用完了【挂起】。处理器会切换到另外的一个线程上来进行执行。并且这个线程的执行时间用完了，接着处理器就会又来执行被挂起的这个线程。这时候程序计数器就起到了关键作用，程序计数器在来回切换的线程中记录他上一次执行的行号，然后接着继续向下执行。
>
>**面试官**：你能给我详细的介绍Java堆吗?
>
>**候选人:**
>
>好的~
>
>Java中的堆术语线程共享的区域。主要用来保存**对象实例，数组**等，当堆中没有内存空间可分配给实例，也无法再扩展时，则抛出OutOfMemoryError异常。
>
>​	在JAVA8中堆内会存在年轻代、老年代
>
>​	1）Young区被划分为三部分，Eden区和两个大小严格相同的Survivor区，其中，Survivor区间中，某一时刻只有其中一个是被使用的，另外一个留做垃圾收集时复制对象用。在Eden区变满的时候， GC就会将存活的对象移到空闲的Survivor区间中，根据JVM的策略，在经过几次垃圾收集后，任然存活于Survivor的对象将被移动到Tenured区间。
>
>​	2）Tenured区主要保存生命周期长的对象，一般是一些老的对象，当一些对象在Young复制转移一定的次数以后，对象就会被转移到Tenured区。
>
>**面试官**：能不能解释一下方法区？
>
>**候选人:**
>
>好的~
>
>与虚拟机栈类似。本地方法栈是为虚拟机**执行本地方法时提供服务的**。不需要进行GC。本地方法一般是由其他语言编写。
>
>**面试官**：你听过直接内存吗？
>
>**候选人:**
>
>嗯~~
>
>它又叫做**堆外内存**，**线程共享的区域**，在 Java 8 之前有个**永久代**的概念，实际上指的是 HotSpot 虚拟机上的永久代，它用永久代实现了 JVM 规范定义的方法区功能，**主要存储类的信息，常量，静态变量**，即时编译器编译后代码等，这部分由于是在堆中实现的，受 GC 的管理，不过由于永久代有 -XX:MaxPermSize 的上限，所以如果大量动态生成类（将类信息放入永久代），很容易造成 OOM，有人说可以把永久代设置得足够大，但很难确定一个合适的大小，受类数量，常量数量的多少影响很大。
>
>​	所以在 Java 8 中就把方法区的实现移到了本地内存中的元空间中，这样方法区就不受 JVM 的控制了,也就不会进行 GC，也因此提升了性能。
>
>**面试官**：什么是虚拟机栈
>
>**候选人:**
>
>虚拟机栈是描述的是方法执行时的内存模型,是线程私有的，生命周期与线程相同,每个方法被执行的同时会创建**栈桢**。保存执行方法时的**局部变量、动态连接信息、方法返回地址信息**等等。方法开始执行的时候会进栈，方法执行完会出栈【相当于清空了数据】，所以这块区域**不需要进行 GC**。
>
>**面试官**：能说一下堆栈的区别是什么吗？
>
>**候选人:**
>
>嗯，好的，有这几个区别
>
>第一，栈内存一般会用来存储局部变量和方法调用，但堆内存是用来存储Java对象和数组的的。堆会GC垃圾回收，而栈不会。
>
>第二、栈内存是线程私有的，而堆内存是线程共有的。
>
>第三、两者异常错误不同，但如果栈内存或者堆内存不足都会抛出异常。
>
>栈空间不足：java.lang.StackOverFlowError。
>
>堆空间不足：java.lang.OutOfMemoryError。

## 类加载器

>**面试官**：什么是类加载器，类加载器有哪些?
>
>**候选人:**
>
>嗯，是这样的
>
>JVM只会运行二进制文件，而类加载器（ClassLoader）的主要作用就是将**字节码文件加载到JVM中**，从而让Java程序能够启动起来。
>
>常见的类加载器有4个
>
>第一个是启动类加载器(BootStrap ClassLoader)：其是由C++编写实现。用于加载JAVA_HOME/jre/lib目录下的类库。
>
>第二个是扩展类加载器(ExtClassLoader)：该类是ClassLoader的子类，主要加载JAVA_HOME/jre/lib/ext目录中的类库。
>
>第三个是应用类加载器(AppClassLoader)：该类是ClassLoader的子类，主要用于加载classPath下的类，也就是加载开发者自己编写的Java类。
>
>第四个是自定义类加载器：开发者自定义类继承ClassLoader，实现自定义类加载规则。
>
>**面试官**：说一下类装载的执行过程？
>
>**候选人:**
>
>嗯，这个过程还是挺多的。
>
>类从加载到虚拟机中开始，直到卸载为止，它的整个生命周期包括了：加载、验证、准备、解析、初始化、使用和卸载这7个阶段。其中，验证、准备和解析这三个部分统称为连接（linking）
>
>1.加载：查找和导入class文件
>
>2.验证：保证加载类的准确性
>
>3.准备：为类变量分配内存并设置类变量初始值
>
>4.解析：把类中的符号引用转换为直接引用
>
>5.初始化：对类的静态变量，静态代码块执行初始化操作
>
>6.使用：JVM 开始从入口方法开始执行用户的程序代码
>
>7.卸载：当用户程序代码执行完毕后，JVM 便开始销毁创建的 Class 对象，最后负责运行的 JVM 也退出内存
>
>**面试官**：什么是双亲委派模型？
>
>**候选人:**
>
>嗯，它是是这样的。
>
>如果一个类加载器收到了类加载的请求，它首先不会自己尝试加载这个类，而是把这请求委派给父类加载器去完成，每一个层次的类加载器都是如此，因此所有的加载请求最终都应该传说到顶层的启动类加载器中，只有当父类加载器返回自己无法完成这个加载请求（它的搜索返回中没有找到所需的类）时，子类加载器才会尝试自己去加载
>
>**面试官**：JVM为什么采用双亲委派机制
>
>**候选人:**
>
>主要有两个原因。
>
>第一、通过双亲委派机制可以避免某一个类被重复加载，当父类已经加载后则无需重复加载，保证唯一性。
>
>第二、为了安全，保证类库API不会被修改

## 垃圾回收

>**面试官**：简述Java垃圾回收机制？（GC是什么？为什么要GC）
>
>**候选人:**
>
>嗯，是这样~~
>
>为了让程序员更专注于代码的实现，而不用过多的考虑内存释放的问题，所以，在Java语言中，有了自动的垃圾回收机制，也就是我们熟悉的GC(Garbage Collection)。
>
>有了垃圾回收机制后，程序员只需要关心内存的申请即可，内存的释放由系统自动识别完成。
>
>在进行垃圾回收时，不同的对象引用类型，GC会采用不同的回收时机
>
>**面试官**：强引用、软引用、弱引用、虚引用的区别？
>
>**候选人:**
>
>嗯嗯~
>
>强引用最为普通的引用方式，表示一个对象处于**有用且必须**的状态，如果一个对象具有强引用，则GC并不会回收它。即便堆中内存不足了，宁可出现OOM，也不会对其进行回收
>
>软引用表示一个对象处于**有用且非必须**状态，如果一个对象处于软引用，在内存空间足够的情况下，GC机制并不会回收它，而在内存空间不足时，则会在OOM异常出现之间对其进行回收。但值得注意的是，因为GC线程优先级较低，软引用并不会立即被回收。
>
>弱引用表示一个对象处于**可能有用且非必须**的状态。在GC线程扫描内存区域时，一旦发现弱引用，就会回收到弱引用相关联的对象。对于弱引用的回收，无关内存区域是否足够，一旦发现则会被回收。同样的，因为GC线程优先级较低，所以弱引用也并不是会被立刻回收。
>
>虚引用表示一个对象处于**无用**的状态。在任何时候都有可能被垃圾回收。虚引用的使用必须和引用队列Reference Queue联合使用
>
>**面试官**：对象什么时候可以被垃圾器回收
>
>**候选人:**
>
>思考一会~~
>
>如果一个或多个对象没有任何的引用指向它了，那么这个对象现在就是垃圾，如果定位了垃圾，则有可能会被垃圾回收器回收。
>
>如果要定位什么是垃圾，有两种方式来确定，第一个是引用计数法，第二个是可达性分析算法
>
>通常都使用可达性分析算法来确定是不是垃圾
>
>**面试官**： JVM 垃圾回收算法有哪些？
>
>**候选人:**
>
>我记得一共有四种，分别是标记清除算法、复制算法、标记整理算法、分代回收
>
>**面试官**： 你能详细聊一下分代回收吗？
>
>**候选人:**
>
>关于分代回收是这样的
>
>在java8时，堆被分为了两份：新生代和老年代，它们默认空间占用比例是1:2
>
>对于新生代，内部又被分为了三个区域。Eden区，S0区，S1区默认空间占用比例是8:1:1
>
>具体的工作机制是有些情况：
>
>1）当创建一个对象的时候，那么这个对象会被分配在新生代的Eden区。当Eden区要满了时候，触发YoungGC。
>
>2）当进行YoungGC后，此时在Eden区存活的对象被移动到S0区，并且**当前对象的年龄会加1**，清空Eden区。
>
>3）当再一次触发YoungGC的时候，会把Eden区中存活下来的对象和S0中的对象，移动到S1区中，这些对象的年龄会加1，清空Eden区和S0区。
>
>4）当再一次触发YoungGC的时候，会把Eden区中存活下来的对象和S1中的对象，移动到S0区中，这些对象的年龄会加1，清空Eden区和S1区。
>
>5）对象的年龄达到了某一个限定的值（**默认15岁**  ），那么这个对象就会进入到老年代中。
>
>当然也有特殊情况，如果进入Eden区的是一个大对象，在触发YoungGC的时候，会直接存放到老年代
>
>当老年代满了之后，**触发FullGC**。**FullGC同时回收新生代和老年代**，当前只会存在一个FullGC的线程进行执行，其他的线程全部会被挂起。  我们在程序中要尽量避免FullGC的出现。
>
>**面试官**：讲一下新生代、老年代、永久代的区别？
>
>**候选人:**
>
>嗯！是这样的，简单说就是
>
>**新生代**主要用来存放新生的对象。
>
>**老年代**主要存放应用中生命周期长的内存对象。
>
>**永久代**指的是永久保存区域。主要存放Class和Meta（元数据）的信息。在Java8中，永久代已经被移除，取而代之的是一个称之为“元数据区”（**元空间**）的区域。元空间和永久代类似，不过元空间与永久代之间最大的区别在于：元空间并不在虚拟机中，而是使用本地内存。因此，默认情况下，元空间的大小仅受本地内存的限制。
>
>**面试官**：说一下 JVM 有哪些垃圾回收器？
>
>**候选人:**
>
>在jvm中，实现了多种垃圾收集器，包括：串行垃圾收集器、并行垃圾收集器（JDK8默认）、CMS（并发）垃圾收集器、G1垃圾收集器（JDK9默认）
>
>**面试官**：Minor GC、Major GC、Full GC是什么
>
>**候选人:**
>
>嗯，其实它们指的是不同代之间的垃圾回收
>
>Minor GC 发生在新生代的垃圾回收，暂停时间短
>
>Major GC 老年代区域的垃圾回收，老年代空间不足时，会先尝试触发Minor GC。Minor GC之后空间还不足，则会触发Major GC，Major GC速度比较慢，暂停时间长
>
>Full GC 新生代 + 老年代完整垃圾回收，暂停时间长，**应尽力避免**

## JVM实践（调优）

>**面试官**：JVM 调优的参数可以在哪里设置参数值？
>
>**候选人:**
>
>我们当时的项目是springboot项目，可以在项目启动的时候，java -jar中加入参数就行了
>
>
>
>**面试官**：用的 JVM 调优的参数都有哪些？
>
>**候选人:**
>
>嗯，这些参数是比较多的
>
>我记得当时我们设置过堆的大小，像-Xms和-Xmx
>
>还有就是可以设置年轻代中Eden区和两个Survivor区的大小比例
>
>还有就是可以设置使用哪种垃圾回收器等等。具体的指令还真记不太清楚。
>
>
>
>**面试官**：嗯，好的，你们平时调试 JVM都用了哪些工具呢？
>
>**候选人:**
>
>嗯，我们一般都是使用jdk自带的一些工具，比如
>
>jps 输出JVM中运行的进程状态信息
>
>jstack查看java进程内**线程的堆栈**信息。
>
>jmap 用于生成堆转存快照
>
>jstat用于JVM统计监测工具
>
>还有一些可视化工具，像jconsole和VisualVM等
>
>**面试官**：假如项目中产生了java内存泄露，你说一下你的排查思路？
>
>**候选人:**
>
>嗯，这个我在之前项目排查过
>
>第一呢可以通过jmap指定打印他的内存快照 dump文件，不过有的情况打印不了，我们会设置vm参数让程序自动生成dump文件
>
>第二，可以通过工具去分析 dump文件，jdk自带的VisualVM就可以分析
>
>第三，通过查看堆信息的情况，可以大概定位内存溢出是哪行代码出了问题
>
>第四，找到对应的代码，通过阅读上下文的情况，进行修复即可
>
>**面试官**：好的，那现在再来说一种情况，就是说服务器CPU持续飙高，你的排查方案与思路？
>
>**候选人:**
>
>嗯，我思考一下~~
>
>可以这么做~~
>
>第一可以使用使用top命令查看占用cpu的情况
>
>第二通过top命令查看后，可以查看是哪一个进程占用cpu较高，记录这个进程id
>
>第三可以通过ps 查看当前进程中的线程信息，看看哪个线程的cpu占用较高
>
>第四可以jstack命令打印进行的id，找到这个线程，就可以进一步定位问题代码的行号
