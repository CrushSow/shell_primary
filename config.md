![截屏2022-04-21 下午10.41.23](/Users/luzoo/Study/shell/pictures/截屏2022-04-21 下午10.41.23.png)

![截屏2022-04-21 下午10.59.10](/Users/luzoo/Study/shell/pictures/截屏2022-04-21 下午10.59.10.png)

```java
public class TestDemo2 {
    public static void main(String[] args) {
        TestDemo demo1 = new TestDemo();
        TestDemo demo2 = new TestDemo();
        System.out.println("TestDemo final field: " + demo1.aDouble );
        System.out.println("TestDemo final field: " + demo2.aDouble );
        System.out.println("TestDemo static field: " + demo1.bDouble );
        System.out.println("TestDemo static field: " + demo1.bDouble );
        // first update static field
        demo1.bDouble =Math.random();
        System.out.println("first final field: " + demo1.aDouble );
        System.out.println("first final field: " + demo2.aDouble );
        System.out.println("first static field: " + demo1.bDouble );
        System.out.println("first static field: " + demo1.bDouble );
        // second update static field
        demo2.bDouble = Math.random();
        System.out.println("first static field: " + demo1.bDouble );
        System.out.println("first static field: " + demo1.bDouble );

    }

}


public class TestDemo {
    public final double aDouble = Math.random();
    public static double bDouble = Math.random();
}


```

1、static修饰的变量和方法，在类加载的时候被初始化，可直接通过类名.变量名和类名.方法名来自进行调用

2、static修饰的变量，在类加载时候会被分配到数据区的方法区。类的实例可共享方法区中的变量。如果static修饰的变量发生变化，那么所有类实例引用的变量都会一起发生变化。

3、static修饰的方法中不能使用this或super，static修饰的方法属于类的方法，而this或supter是对象的方法



4、当这个属性被修饰为final，而不是static的时候，它属于类的实例对象的资源，当类被加载进内存的事哦呼这个属性并没有给分配内存，而是只是定义了一个变量，只有当这个类被实例化的时候这个属性才被分配内存空间，而实例化的时候同时执行了构造函数，所以这个属性被实例化，也就符合了当它被分配内存空间的时候就需要初始化，以后都不能被改变。对象属性



5、当类的属性同时被修饰static和final的时候，它就是属于类的资源，那么就是类在被加载进内存的时候（也就是应用程序启动的时候）就要已经为这个属性分配内存，所以此时的属性已经存在，它又是被final修饰，所以必须在属性的定义了以后就给其初始化值，而构造函数是在当类被实例化的时候才会被执行，所以用构造函数，这时候这个属性没有被初始化，程序就会被报错。而static块是类被加载的时候执行的，且只执行一次，所以在static块中可以被初始化。



final static 修饰的属性 ，在类被加载的时候属性被分配内存完成初始化放到方法区中，赋值之后不可以被修改的一块内存空间。



### 类的加载执行与初始化

- 加载：查找并加载类的二进制数据
- 链接
  - 验证：确保被加载的类的正确性
  - 准备：为类的静态变量分配内存，并将其初始化为默认值
  - 解析：把类中的符号引用转换为直接引用
- 初始化：为类的静态变量赋予正确的初始值
- 值得注意的是：准备阶段即使我们为静态变量赋值为任意的数值，但是该静态变量还是会被初始化为他的默认值，最后的初始化时才会把我们赋予的值设为该静态变量的值。

### Java程序对类的使用可以分为两种

1. 主动使用
   - 创建类的实例
   - 访问某个类或接口的静态变量，或者对该静态变量赋值
   - 调用该类的静态方法
   - 反射
   - 初始化一个类的子类
   - Java虚拟机启动时被标为启动类的类（Java Test）
2. 被动使用
3. 所有的Java虚拟机实现必须在每个类或接口被Java程序“首次主动使用”时才初始化他们

------

- 类的加载:指的是将类的。class文件中的二进制数据读入到内存中，将其放在运行时数据区的方法区内，然后在内存中创建一个`Java.lang.Class`对象（规范并没有说明Class对象位于哪里，HotSpot虚拟机将其放在了方法区中）用来封装类在方法区的数据结构
- 加载类的方式
  - 从本地系统中直接加载
  - 通过网络下载.class文件
  - 从zip，jar等归档文件中加载.class文件
  - 从专有数据库中提取.class文件
  - 将java源文件动态编译为.class文件（将JAVA源文件动态编译这种情况会在动态代理和web开发中jsp转换成Servlet）



使用加载器对类进行加载 



```java
public class TestDemo4 extends ClassLoader{
    public static void main(String[] args) throws ClassNotFoundException, InstantiationException, IllegalAccessException {
        System.out.println(1);
        ClassLoader classLoader = ClassLoader.getSystemClassLoader();

        byte[] cLassBytes = null;
        Path path = null;
        try{
            path = Paths.get(new URI("file:///Users/luzoo/Study/JavaProject/NettyStudy/target/classes/jvmStudy/TestDemo3.class"));
            cLassBytes= Files.readAllBytes(path);
        }catch (IOException | URISyntaxException e){
            e.printStackTrace();
        }
        TestDemo4 testDemo4 = new TestDemo4();
        Class<?> aClass = testDemo4.defineClass("jvmStudy.TestDemo3", cLassBytes, 0, cLassBytes.length);
        aClass.newInstance();
        Class<?> loadClass = classLoader.loadClass("TestDemo3.class");
        System.out.println("----------------");
        loadClass.newInstance();
        System.out.println("---------");
        //Object.class.getClassLoader().loadClass("src.main.java.jvmStudy.class");
    }



}

```





