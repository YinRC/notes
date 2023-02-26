# 1. 什么是注解

不是程序本身，可以被编译器读取

可以添加参数值 @SuppressWarnings(value=“unckecked”)

可以在package，class，method，field上面为它们添加辅助信息

这些辅助信息可以通过反射机制进行访问

注解也有检查和约束的作用



# 2. 内置注解

**@Override**：定义在 java.lang.Override 中，此注释只是表明一个方法打算重写超类中的同名方法。

**@Deprecated** ：定义在 java.lang.Deprecated 中，含义是不推荐，表明危险或有更好的替代。

**@SuppressWarnings**：定义在 java.lang.SuppressWarnings 中，用来抑制编译时的警告信息。

```java
package com.isaiah.anno;

@SuppressWarnings("all")
public class Test1 {
    @Override
    public String toString() {
        return super.toString();
    }

    @Deprecated
    public static void test() {
        System.out.println("Deprecated");
    }

    public static void main(String[] args) {
        test();
    }
}
```



# 3. 元注解

负责注解其它的注解

java 定义了4个标准的 meta-annotation 类型，用来对 annotation 类型进行说明

**@Target**：描述注解的使用范围

**@Retention**：描述注解的生命周期（SOURCE < CLASS < RUNTIME）

**@Document**：该注解被包含在 javadoc 中

**@Inherited**：子类可以继承父类中的该注解

```java
public enum RetentionPolicy {
    /**
     * Annotations are to be discarded by the compiler.
     */
    SOURCE,

    /**
     * Annotations are to be recorded in the class file by the compiler
     * but need not be retained by the VM at run time.  This is the default
     * behavior.
     */
    CLASS,

    /**
     * Annotations are to be recorded in the class file by the compiler and
     * retained by the VM at run time, so they may be read reflectively.
     *
     * @see java.lang.reflect.AnnotatedElement
     */
    RUNTIME
}
```

```java
package com.isaiah.anno;

import java.lang.annotation.*;

@MyAnnotation
public class Test2 {
    @MyAnnotation
    public void test() {

    }
}

@Target(value={ElementType.METHOD, ElementType.TYPE})
@Retention(value= RetentionPolicy.RUNTIME)
@Documented
@Inherited
@interface MyAnnotation {

}
```



# 3. 自定义注解

```java
package com.isaiah.anno;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

public class Test3 {
//    @MyAnnotation2(name="isaiah", age = 18)
    @MyAnnotation2(school = {"bjut", "北工大"})   // default name ""
    public void test() {}

    @MyAnnotation3("isaiah")
    public void test2() {}
}

@Target({ElementType.METHOD, ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@interface MyAnnotation2 {
    String name() default "";
    int age() default 0;
    int id() default -1;   // not exist
    String[] school();
}

@Target({ElementType.METHOD, ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@interface MyAnnotation3 {
    String value();
}
```



# 4. 静态 VS 动态

**动态语言**：运行时可以改变其代码结构的语言。例如，新的函数，新的对象，甚至是引入一段新的代码。

​	主要有：Object-C，C#，JavaSript，PHP，Python

**静态语言**：运行时结构不可变的语言。

​	主要有：Java，C，C++

**Java 不是动态语言，但可以称之为 ”准动态语言“**。即 Java 有一定的动态性，可以利用反射机制实现这一点。

```javascript
function f() {
    var x = "var a = 3; b = 5; alert(a + b)"
    eval(x);
}
```



# 5. Java 反射机制

反射机制允许程序在**运行期**借助 Reflection API **取得类的所有信息**，并能**直接操作任意对象的内部属性及方法**

```java
Class c = Class.forName("java.lang.String");
```

**类被加载完成之后**，**堆栈**中就产生了一个 **Class 类型的对象**（一个类只有一个 Class 对象），这个对象就包含了完整的类的结构信息。我们可以通过这个对象看到类的结构。这个对象就像是类的一面镜子，由此可以看到类的结构。所以**这种获取信息的方式称之为反射**

**Object 类中定义了 getClass()  方法**，此方法将被**所有子类继承**

```java
package com.isaiah.reflect;

public class Test2 extends Object {
    public static void main(String[] args) {
        try {
            Class<?> c1 = Class.forName("com.isaiah.reflect.User");
            System.out.println(c1);

            // one class have only one class instance in the memory
            // once a class been loaded, entire class struct will encapsulation into a class instance
            Class<?> c2 = Class.forName("com.isaiah.reflect.User");
            Class<?> c3 = Class.forName("com.isaiah.reflect.User");
            Class<?> c4 = Class.forName("com.isaiah.reflect.User");
            System.out.println(c2.hashCode());
            System.out.println(c3.hashCode());
            System.out.println(c4.hashCode());
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}

class User {
    private String name;
    private int id;
    private int age;

    public User(String name, int id, int age) {
        this.name = name;
        this.id = id;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", id=" + id +
                ", age=" + age +
                '}';
    }
}
```

**正常方式**：引入需要的 “包类” 名称 ——> 通过 new 实例化 ——> 取得实例化对象

**反射方式**：实例化对象 ——> getClass() 方法 ——> 得到完整的 “包类” 名称

+ 在运行时**判断任意一个对象所属的类**
+ 在运行时**构造任意一个类的对象**
+ 在运行时**判断任意一个类所具有的成员变量和方法**
+ 在运行时**获取泛型信息**
+ 在运行时**调用任意一个对象的成员变量和方法**
+ 在运行时**处理注解**
+ 生成**动态代理 AOP**

**优点**：可以实现动态创建对象和编译，体现出很大的灵活性

**缺点**：对性能有影响。使用反射**大部分是一种解释操作**，其比直接执行慢得多

**java.lang.Class**：代表一个类

**java.lang.reflect.Method**：代表类的**方法**

**java.lang.reflect.Field**：代表类的**成员变量**

**java.lang.reflect.Constructor**：代表类的**构造器**



# 6. Class 类

+ **Class 本身也是一个类**
+ Class 对象**只能由系统建立**
+ **一个加载的类在 JVM 中只会有一个 Class 实例**
+ 一个 Class 对象**对应的是一个加载到 JVM 中的一个 class 文件**
+ 每个**类的实例**都会记得自己是由哪个 **Class 实例**所生成
+ 通过 Class 对象可以完整地得到一个类中的所有的被加载的结构
+ Class 类是 Reflection 的根源，**针对任何想要动态加载、运行的类，唯有先获得相应的 Class 对象**

**static Class forName(String name)**：==返回指定类名的 Class 对象==

**Object newInstance()**：==调用构造函数，返回 Class 对象的一个实例==

**getName()**：==返回此 Class 对象所表示的实体（类，接口，数组类型，void）的名称==

**Class getSuperClass()**：返回当前 Class 对象的父类的 Class 对象

**Class[] getinterfaces()**：==获取当前 Class 对象的接口==

**ClassLoader getClassLoader()**：==返回该类的类加载器==

**Constructor[] getConstructors()**：==返回一个包含Constructor对象的数组==

**Method getMethod(String name, Class.. T)**：==返回一个Method对象，此对象的形参类型为paramType==

**Field[] getDelcaredFields()**：==返回Field对象的一个数组==

> 获得 Class 类的实例
>
> a）**已知具体的类：**通过类的class属性获取，性能最高
>
> b）**已知实例：**调用getClass()方法获取Class对象
>
> c）**已知类的全类名，且在该类的路径下：**可通过Class类的静态方法forName()获取，**可能抛出ClassNotFoundException**
>
> d）**基本数据类型：**可以直接 类名.Type
>
> e）可以用**类加载器ClassLoader**

```java
package com.isaiah.reflect;

public class Test3 {
    public static void main(String[] args) {
        Person person = new Student();
        System.out.println("this man is a/an " + person.name);

        // get class instance a)
        Class<Student> c3 = Student.class;
        System.out.println(c3.hashCode());
        
        // get class instance b)
        Class<?> c1 = person.getClass();
        System.out.println(c1.hashCode());

        // get class instance c)
        try {
            Class<?> c2 = Class.forName("com.isaiah.reflect.Student");
            System.out.println(c2.hashCode());
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }

        // get class instance d)
        Class<Integer> c4 = Integer.TYPE;
        System.out.println(c4);
        System.out.println(int.class);

        // get super class
        Class<?> c5 = c1.getSuperclass();
        System.out.println(c5);
    }
}

class Person {
    String name;

    public Person() {
    }

    public Person(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                '}';
    }
}

class Student extends Person {
    public Student() {
        super.name = "student";
    }
}

class Teacher extends Person {
    public Teacher() {
        super.name = "teacher";
    }
}
```

有 Class 实例对象的类

```java
package com.isaiah.reflect;

import java.lang.annotation.ElementType;

public class Test4 {
    public static void main(String[] args) {
        Class<?> c1 = Object.class;
        Class<?> c2 = Comparable.class;
        Class<?> c3 = String[].class;
        Class<?> c4 = int[][].class;
        Class<?> c5 = Override.class;
        Class<?> c6 = ElementType.class;
        Class<?> c7 = Integer.class;
        Class<?> c8 = void.class;
        Class<?> c9 = Class.class;

        System.out.println(c1);
        System.out.println(c2);
        System.out.println(c3);
        System.out.println(c4);
        System.out.println(c5);
        System.out.println(c6);
        System.out.println(c7);
        System.out.println(c8);
        System.out.println(c9);
    }
}

```



# 7. 类加载内存分析

==Java 内存由堆、栈、方法区构成==

**堆**：存放 **new 出来的对象和数组**，可以被所有线程共享

**栈**：存放**基本类型**（**会存放基本类型的具体数值**）**对象的引用**（**会存放这个对象在堆中的地址**）

**方法区**：可以被所有线程共享，**包含了所有 class 和 static 变量**



类的加载过程：

==类的加载 ——> 类的链接 ——> 类的初始化==

**类的加载**：==将类的**class文件**读入内存，并为之创建一个**java.lang.Class对象**，此过程由**类加载器**完成==

**类的链接**：==将类的二进制数据**合并到 JRE** 中==

**类的初始化**：==**JVM** 负责对类进行初始化==



类的加载和ClassLoader的理解：

**加载**：将**class文件中的字节码**内容加载到内存中，并将这些**静态数据转化为方法区的运行时数据结构**，最后**生成一个代表这个类的 java.lang.Class 对象**（因此==不能够主动创建class对象，而只能获取==）

**链接**：确保加载的类信息符合 JVM 规范，没有安全方面的问题（验证阶段）正式**为类变量（static）分配内存并设置类变量的默认初始值**，内存将在方法区中分配（准备阶段）虚拟机**常量池内的符号引用（常量名）替换为直接引用（地址）**（解析阶段）

**初始化**：执行**类构造器<clinit>()** 方法的过程。类构造器私有编译器收集的**类变量的赋值动作和静态代码块中的语句**合并而成的（类构造器是构造类的，不是构造类对象的构造器）**当初始化一个类时，若发现其父类还没有进行初始化，就先初始化父类**。虚拟机会保证一个类的<clinit>() 方法在多线程的环境中被正确加锁或同步

```java
package com.isaiah.reflect;

public class Test5 {
    public static void main(String[] args) {
        /*
        * <clinit>() {
        *   System.out.println("A class static block initial");
        *   m = 300;
        *   m = 100;
        * } --> m = 100;
        * */
        A a = new A();
        System.out.println(A.m);
    }
}

class A {
    static {
        System.out.println("A class static block initial");
        m = 300;
    }
    static int m = 100;

    public A() {
        System.out.println("A class non-parameter constructor initial");
    }
}
```



# 8. 分析类初始化

**类的主动引用**（发生类的初始化）：

+ 当虚拟机启动时，==先初始化 main 方法所在的类==
+ ==new 一个类的对象==（为一个类的对象分配空间）
+ ==调用类的静态成员（除了 final 常量）和静态方法==
+ ==使用 java.lang.reflect 包中的方法对类进行反射调用==
+ ==当初始化一个类时，其父类还没有被初始化，则先初始化其父类

**类的被动引用**（不会发生类的初始化）：

+ 当访问一个静态域时，只有声明这个域的那个类会被初始化（==若子类引用父类的静态变量，此举不会导致子类的初始化==）
+ ==通过数组定义类引用，不会触发此类的初始化==
+ ==引用常量不会导致此类的初始化==（常量在链接阶段就存入调用类的常量池中了）

```java
package com.isaiah.reflect;

public class Test6 {
    static {
        System.out.println("main class been loaded");
    }

    public static void main(String[] args) {
        // active reference
        Son son = new Son();    // son class will be loaded
        try {
            Class.forName("com.isaiah.reflect.Son");    // same as above
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
        // passive reference
        System.out.println(Son.b);  // father static var, son class will not be loaded
        Son[] array = new Son[5];   // just array, not class instance
        System.out.println(Son.M);  // constant var
    }
}

class Father {
    static int b = 2;
    static {
        System.out.println("father class been loaded");
    }
}

class Son extends Father {
    static {
        System.out.println("son class been loaded");
        m = 300;
    }
    static int m = 100;
    static final int M = 1;
}

```



# 9. 类加载器

源程序（\*.java文件）——> **Java 编译器** ——> 字节码（\*.class文件）——> **类加载器** ——> **字节码校验器** ——> **解释器** ——> os

**类缓存**：标准的 JavaSE 类加载器可以按要求查找类，类被加载到类加载器中，将维持加载（缓存）一段时间，直到被垃圾回收器（gc）回收（回收 class 对象）



类加载器（classloader）的类型（JVM规范定义）：

**引导类加载器（Bootstrap）**：用 C++ 编写，是 JVM 自带的类加载器，负责 ==Java 平台核心类库==，**该加载器无法直接获取**

**扩展类加载器（Extension）**：负责 ==jre/lib/ext 目录下的 jar 包或 -D java.ext.dirs== 指定目录下的 jar 包

**系统类加载器（app/system）**：负责 ==-classpath 或 -D java.class.path== 所指目录下的类与 jar 包，**是最常用的类加载器**

```java
package com.isaiah.reflect;

public class Test7 {
    public static void main(String[] args) {
        // get system class loader aka application class loader
        ClassLoader systemClassLoader = ClassLoader.getSystemClassLoader();
        System.out.println("systemClassLoader = " + systemClassLoader);

        // get its super class -> extension class loader
        ClassLoader parent = systemClassLoader.getParent();
        System.out.println("parent = " + parent);

        // get its super class -> boot strap class loader (c/c++)
        ClassLoader root = parent.getParent();
        System.out.println("root = " + root);

        try {
            ClassLoader classLoader = Class.forName("com.isaiah.reflect.Test3").getClassLoader();
            System.out.println("present classLoader = " + classLoader);
            classLoader = Class.forName("java.lang.Object").getClassLoader();
            System.out.println("jdk classLoader = " + classLoader);
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }

        // 系统类加载器 加载的路径
        System.out.println(System.getProperty("java.class.path"));
        /*
        D:\Java\jdk1.8.0_201\jre\lib\charsets.jar;
        D:\Java\jdk1.8.0_201\jre\lib\deploy.jar;
        D:\Java\jdk1.8.0_201\jre\lib\ext\access-bridge-64.jar;
        D:\Java\jdk1.8.0_201\jre\lib\ext\cldrdata.jar;
        D:\Java\jdk1.8.0_201\jre\lib\ext\dnsns.jar;
        D:\Java\jdk1.8.0_201\jre\lib\ext\jaccess.jar;
        D:\Java\jdk1.8.0_201\jre\lib\ext\jfxrt.jar;
        D:\Java\jdk1.8.0_201\jre\lib\ext\localedata.jar;
        D:\Java\jdk1.8.0_201\jre\lib\ext\nashorn.jar;
        D:\Java\jdk1.8.0_201\jre\lib\ext\sunec.jar;
        D:\Java\jdk1.8.0_201\jre\lib\ext\sunjce_provider.jar;
        D:\Java\jdk1.8.0_201\jre\lib\ext\sunmscapi.jar;
        D:\Java\jdk1.8.0_201\jre\lib\ext\sunpkcs11.jar;
        D:\Java\jdk1.8.0_201\jre\lib\ext\zipfs.jar;
        D:\Java\jdk1.8.0_201\jre\lib\javaws.jar;
        D:\Java\jdk1.8.0_201\jre\lib\jce.jar;
        D:\Java\jdk1.8.0_201\jre\lib\jfr.jar;
        D:\Java\jdk1.8.0_201\jre\lib\jfxswt.jar;
        D:\Java\jdk1.8.0_201\jre\lib\jsse.jar;
        D:\Java\jdk1.8.0_201\jre\lib\management-agent.jar;
        D:\Java\jdk1.8.0_201\jre\lib\plugin.jar;
        D:\Java\jdk1.8.0_201\jre\lib\resources.jar;
        D:\Java\jdk1.8.0_201\jre\lib\rt.jar;
        
        // 读取到之前自己写完的类，被系统类加载器加载，所以可以用
        D:\IdeaProjects\annotation\target\classes;
        
        D:\IntelliJ IDEA 2019.3.3\lib\idea_rt.jar
         */
    }
}
```

**双亲委派机制**：当一个类加载器收到了类加载的请求的时候，它不会直接加载指定的类，而是把这个请求委托给自己的父加载器去加载。只有当父类加载器无法加载这个类的时候，才会由当前这个加载器加载



# 10. 获得类的运行时结构

+ 获得**类的名字：**getName()	getSimpleName()
+ 获得**类的属性：**getDeclaredFields()     getFields()    getDeclaredField
+ 获得**类的方法：**getMethods()    getDeclaredMethods()    getMethod()
+ 获得**构造方法：**getConstructors()    getDeclaredConstructors()    getDeclaredConstructor

```java
package com.isaiah.reflect;

import java.lang.reflect.Constructor;
import java.lang.reflect.Field;
import java.lang.reflect.Method;

public class Test8 {
    public static void main(String[] args) throws ClassNotFoundException {
        Class<?> c1 = Class.forName("com.isaiah.reflect.User");

        // 获得类的名字
        System.out.println(c1.getName());   // 获得包名 + 类名
        System.out.println(c1.getSimpleName()); // 获得类名

        // 获得类的属性
        System.out.println("-----------------------------------");
        Field[] declaredFields = c1.getDeclaredFields();    // 找到全部的属性
        for (Field declaredField : declaredFields) {
            System.out.println("declaredField = " + declaredField);
        }

        Field[] fields = c1.getFields();    // 只能找到 public 属性
        for (Field field : fields) {
            System.out.println("field = " + field);
        }

        try {
            Field name = c1.getDeclaredField("name");
            System.out.println("name = " + name);
        } catch (NoSuchFieldException e) {
            e.printStackTrace();
        }

        // 获得类的方法
        System.out.println("-------------------------------------");
        Method[] methods = c1.getMethods();     // 获得本类及其父类的所有公共方法
        for (Method method : methods) {
            System.out.println("method = " + method);
        }
        Method[] declaredMethods = c1.getDeclaredMethods();     // 获得本类的所有方法
        for (Method declaredMethod : declaredMethods) {
            System.out.println("declaredMethod = " + declaredMethod);
        }
        try {
            // 因为重载的存在，所以需要指定参数
            Method getName = c1.getMethod("getName", null);
            System.out.println("getName = " + getName);
            Method setName = c1.getMethod("setName", String.class);
            System.out.println("setName = " + setName);
        } catch (NoSuchMethodException e) {
            e.printStackTrace();
        }

        // 获得指定的构造器
        System.out.println("---------------------------------------");
        Constructor<?>[] constructors = c1.getConstructors();
        for (Constructor<?> constructor : constructors) {
            System.out.println("constructor = " + constructor);
        }
        Constructor<?>[] declaredConstructors = c1.getDeclaredConstructors();
        for (Constructor<?> declaredConstructor : declaredConstructors) {
            System.out.println("declaredConstructor = " + declaredConstructor);
        }
        try {
            Constructor<?> declaredConstructor = c1.getDeclaredConstructor(String.class, int.class, int.class);
            System.out.println("specific declaredConstructor = " + declaredConstructor);
        } catch (NoSuchMethodException e) {
            e.printStackTrace();
        }
    }
}

```



# 11. 动态创建对象invoke & set

==Method，Field，Constructor对象都有setAccessible()== 方法

此方法的作用是启用或禁用 ==访问安全检查==

+ true：提高反射的效率（必须有反射的话建议打开）、使得**原来无法访问的私有成员也可以被访问**
+ false：即指示反射的对象应执行Java 访问安全检查（默认）

```java
package com.isaiah.reflect;

import java.lang.reflect.Constructor;
import java.lang.reflect.Field;
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;

public class Test9 {
    public static void main(String[] args) {
        Class<?> c1 = null;
        // 获得 Class 对象
        try {
            c1 = Class.forName("com.isaiah.reflect.User");
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
        // 构造一个对象
        User user = null;
        try {
            assert c1 != null;
            user = (User) c1.newInstance(); // 本质是调用类的无参构造器
        } catch (InstantiationException | IllegalAccessException e) {
            e.printStackTrace();
        }
        System.out.println("user = " + user);

        try {
            Constructor<?> declaredConstructor = c1.getDeclaredConstructor(String.class, int.class, int.class);
            User user1 = (User) declaredConstructor.newInstance("isaiah", 1, 2);    // 调用类的有参构造
            System.out.println("user1 = " + user1);
        } catch (NoSuchMethodException | InstantiationException | IllegalAccessException | InvocationTargetException e) {
            e.printStackTrace();
        }

        // 通过反射调用方法
        try {
            User user1 = (User) c1.newInstance();
            Method setName = c1.getDeclaredMethod("setName", String.class);
            setName.invoke(user1, "isaiah");    // 激活：传递对象，参数值
            System.out.println(user1.getName());
        } catch (InstantiationException | IllegalAccessException | NoSuchMethodException | InvocationTargetException e) {
            e.printStackTrace();
        }

        // 通过反射操作属性
        try {
            User user1 = (User) c1.newInstance();
            Field declaredField = c1.getDeclaredField("name");
            declaredField.setAccessible(true);  // 使得可以操作私有属性
            declaredField.set(user1, "oliver");
            System.out.println(user1.getName());
        } catch (InstantiationException | IllegalAccessException | NoSuchFieldException e) {
            e.printStackTrace();
        }
    }
}

```



# 12. 性能对比分析

```java
package com.isaiah.reflect;

import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;

public class Test10 {
    public static void test1() {
        long start = System.currentTimeMillis();
        User user = new User();
        for (int i = 0; i < 1e9; i++) {
            user.getName();
        }
        long end = System.currentTimeMillis();
        System.out.println("普通方法 ：" + (end - start) + "ms");
    }

    public static void test2() throws ClassNotFoundException, NoSuchMethodException, IllegalAccessException, InstantiationException, InvocationTargetException {
        long start = System.currentTimeMillis();
        Class<?> c1 = Class.forName("com.isaiah.reflect.User");
        User user = (User) c1.newInstance();
        Method getName = c1.getMethod("getName");
        for (int i = 0; i < 1e9; i++) {
            getName.invoke(user);
        }
        long end = System.currentTimeMillis();
        System.out.println("反射方法 ：" + (end - start) + "ms");
    }

    public static void test3() throws ClassNotFoundException, NoSuchMethodException, IllegalAccessException, InstantiationException, InvocationTargetException {
        long start = System.currentTimeMillis();
        Class<?> c1 = Class.forName("com.isaiah.reflect.User");
        User user = (User) c1.newInstance();
        Method getName = c1.getMethod("getName");
        getName.setAccessible(true);
        for (int i = 0; i < 1e9; i++) {
            getName.invoke(user);
        }
        long end = System.currentTimeMillis();
        System.out.println("反射方法 + accessTrue ：" + (end - start) + "ms");
    }

    public static void main(String[] args) throws ClassNotFoundException, NoSuchMethodException, InvocationTargetException, InstantiationException, IllegalAccessException {
        test1();
        test2();
        test3();
    }
}

```

```shell
普通方法 ：1145ms
反射方法 ：2342ms
反射方法 + accessTrue ：1265ms
```

Java 采用**泛型擦除**的机制，即泛型仅仅是给编译器 javac 使用的，确保数据的安全性，免去强制类型转化的问题，==一旦编译完成所有泛型都会被擦除==

为了通过反射操作泛型，Java 新增了几种类型：

**ParameterizedType**：表示一种参数化类型，例如 Collection<String>

**GenericArrayType**：表示一种元素类型是参数化类型 或者 类型变量的数组类型

**TypeVariable**：各种类型变量的公共父接口（local instance static）

**WildcardType**：代表一种通配符类型表达式

```java
package com.isaiah.reflect;

import java.lang.reflect.Method;
import java.lang.reflect.ParameterizedType;
import java.lang.reflect.Type;
import java.util.List;
import java.util.Map;

public class Test11 {
    public void test1(Map<String, User> map, List<User> list) {
        System.out.println("test 1");
    }

    public Map<String, User> test2() {
        System.out.println("test 2");
        return null;
    }

    public static void main(String[] args) throws NoSuchMethodException {
        Method method = Test11.class.getMethod("test1", Map.class, List.class);
        Type[] genericParameterTypes = method.getGenericParameterTypes();
        for (Type genericParameterType : genericParameterTypes) {
            System.out.println("genericParameterType = " + genericParameterType);
            if (genericParameterType instanceof ParameterizedType) {
                Type[] actualTypeArguments = ((ParameterizedType) genericParameterType).getActualTypeArguments();
                for (Type actualTypeArgument : actualTypeArguments) {
                    System.out.println("actualTypeArgument = " + actualTypeArgument);
                }
            }
        }

        method = Test11.class.getMethod("test2");
        Type genericReturnType = method.getGenericReturnType();
        if (genericReturnType instanceof ParameterizedType) {
            Type[] actualTypeArguments = ((ParameterizedType) genericReturnType).getActualTypeArguments();
            for (Type actualTypeArgument : actualTypeArguments) {
                System.out.println(actualTypeArgument);
            }
        }
    }
}

```



# 13. 反射操作注解

getAnnotations()    getAnnotation()

```java
package com.isaiah.reflect;

import java.lang.annotation.*;
import java.lang.reflect.Field;

public class Test12 {
    public static void main(String[] args) throws ClassNotFoundException {
        Class<?> c1 = Class.forName("com.isaiah.reflect.Woman");
        // 获得类的所有注解
        Annotation[] annotations = c1.getAnnotations();
        for (Annotation annotation : annotations) {
            System.out.println("annotation = " + annotation);
        }
        // 获得特定注解的值
        TableAnn tableAnn = c1.getAnnotation(TableAnn.class);
        System.out.println(tableAnn.name());

        Field[] declaredFields = c1.getDeclaredFields();
        for (Field declaredField : declaredFields) {
            FieldAnn fieldAnn = declaredField.getAnnotation(FieldAnn.class);
            System.out.println(fieldAnn.name());
            System.out.println(fieldAnn.length());
        }
    }
}

@TableAnn(name = "woman")
class Woman {
    @FieldAnn(name = "name", length = 10)
    public String name;
    @FieldAnn(name = "id", length = 20)
    private int id;
    @FieldAnn(name = "age", length = 3)
    int age;
}

@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@interface TableAnn {
    String name();
}

@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
@interface FieldAnn {
    String name();
    int length();
}

```



