# 1.  序列化与反序列化 serialization & deserialization

**Serialization** is a mechanism of converting the state of an object into a byte stream. 

**Deserialization** is the reverse process where the byte stream is used to recreate the actual Java object in memory. 

This mechanism is used to

+ save/persist state of an object.
+ travel an object across a network.

The **byte stream is platform independent**. So, the object serialized on one platform can be deserialized on a different platform.

![serialize-deserialize-java](./serial.assets/serialize-deserialize-java.png)



# 2. 具体实现 implement

To make a Java object serializable we implement the **java.io.Serializable** interface.

> Serializable is a **marker interface** (has no data member and method). It is used to “mark” java classes so that objects of these classes may get certain capability.

The **ObjectOutputStream** class contains **writeObject()** method for serializing an Object.

```java
public final void writeObject(Object obj)
                       throws IOException
```

The **ObjectInputStream** class contains **readObject()** method for deserializing an object.

```java
public final Object readObject()
                  throws IOException,
               ClassNotFoundException
```



```java
package com.isaiah.serial;

import java.io.*;

class Demo implements Serializable {
    public int a;
    public String b;

    public Demo(int a, String b) {
        this.a = a;
        this.b = b;
    }
}

public class TestSerialization {
    public static void main(String[] args) {
        Demo demo = new Demo(11, "abc");
        String fileName = "demoInstance.ser";

        // serialization
        try {
            FileOutputStream fileOutputStream = new FileOutputStream(fileName);
            ObjectOutputStream objectOutputStream = new ObjectOutputStream(fileOutputStream);
            objectOutputStream.writeObject(demo);
            objectOutputStream.close();
            fileOutputStream.close();
            System.out.println("Object has been serialized");
        } catch (IOException e) {
            e.printStackTrace();
        }

        // deserialization
        try {
            FileInputStream fileInputStream = new FileInputStream(fileName);
            ObjectInputStream objectInputStream = new ObjectInputStream(fileInputStream);
            Demo demo1 = (Demo) objectInputStream.readObject();
            System.out.println("a : " + demo1.a + " b : " + demo1.b);
            objectInputStream.close();
            fileInputStream.close();
        } catch (IOException | ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}

```



# 3. 注意事项 careful

+ If a parent class has implemented Serializable interface then child class doesn’t need to implement it.
+ Only **non-static** data members are saved via Serialization process.
+ **Static data members and transient data members** are not saved via Serialization process.So, if you don’t want to save value of a non-static data member then make it transient.
+ Constructor of object is never called when an object is deserialized.



**SerialVersionUID**

The Serialization runtime associates a version number with each Serializable class called a SerialVersionUID, which is used during Deserialization to verify that sender and receiver of a serialized object have loaded classes for that object which are compatible with respect to serialization. If the receiver has loaded a class for the object that has different UID than that of corresponding sender’s class, the Deserialization will result in an **InvalidClassException**. A Serializable class can declare its own UID explicitly by declaring a field name.
It must be static, final and of type long.
i.e- ANY-ACCESS-MODIFIER static final long serialVersionUID=42L;

If a serializable class doesn’t explicitly declare a serialVersionUID, then the serialization runtime will calculate a default one for that class based on various aspects of class, as described in Java Object Serialization Specification. However it is strongly recommended that all serializable classes explicitly declare serialVersionUID value, since its computation is highly sensitive to class details that may vary depending on compiler implementations, any change in class or using different id may affect the serialized data.

It is also recommended to use private modifier for UID since it is not useful as inherited member.



```java
package com.isaiah.serial;

import org.omg.CORBA.portable.ObjectImpl;

import java.io.*;

class Demo1 implements Serializable {
    private static final long serialVersionUID = 129348938L;
    transient int a;
    static int b;
    String name;
    private int age;

    public Demo1(String name, int age, int a, int b) {
        this.a = a;
        Demo1.b = b;
        this.name = name;
        this.age = age;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}

public class TestSerialization1 {
    private static void printData(Demo1 demo1) {
        System.out.println("name = " + demo1.name);
        System.out.println("age = " + demo1.getAge());
        System.out.println("a = " + demo1.a);
        System.out.println("b = " + Demo1.b);
    }

    public static void main(String[] args) {
        Demo1 demo = new Demo1("isaiah", 11, 1, 2);
        String fileName = "demo1Instance.txt";

        // serialization
        try {
            FileOutputStream fileOutputStream = new FileOutputStream(fileName);
            ObjectOutputStream objectOutputStream = new ObjectOutputStream(fileOutputStream);
            objectOutputStream.writeObject(demo);
            // value of static variable changed 2 -> 3
            Demo1.b = 3;
            objectOutputStream.close();
            fileOutputStream.close();
        } catch (IOException e) {
            e.printStackTrace();
        }

        // deserialization
        try {
            FileInputStream fileInputStream = new FileInputStream(fileName);
            ObjectInputStream objectInputStream = new ObjectInputStream(fileInputStream);
            Demo1 demo1 = (Demo1) objectInputStream.readObject();
            TestSerialization1.printData(demo1);
            objectInputStream.close();
            fileInputStream.close();
        } catch (IOException | ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}

```

