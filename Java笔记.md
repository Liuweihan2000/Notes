## 泛型

是一种未知的数据类型。当我们不知道使用什么数据类型的时候，可以使用泛型。

泛型也可以看作是一个变量，用来接收数据类型

E e: Element

T t: Type

ArrayList集合在定义的时候，不知道集合中都会存储什么类型的数据

```Java
public class ArrayList<E>{
    public boolean add(E e) {}
    public E get(int index) {}
}
```

E: 未知的数据类型

在创建集合对象的时候就会确定泛型的数据类型

```java
ArrayList<String> list = new ArrayList<String>()
```

会把数据类型作为参数传递，把String赋值给泛型E

```java
// 不使用泛型
// 可以存储任意类型的数据
private static void show1() {
    ArrayList list = new ArrayList();
	list.add("abc");
	list.add(1);
    Iterator it = list.iterator();
    while (it.hasNext()) {
        // 取出元素也是Object类型
        Object obj = it.next();
        Sout(obj);
        // 想要使用String类特有的方法，length获取字符串的长度，不能使用
        // 需要向下转型
        String s = (String)obj;
        Sout(s.length());
        // 但会出现ClassCassException  Integer !--> String
        // 不使用泛型容易引发异常
    }
}

// 使用泛型
// 避免了类型转换的麻烦，存储的是什么类型，取出的就是什么类型
// 把运行期异常提前到了编译期
private static void show2() {
    ArrayList<String> list = new ArrayList<>();
    list.add("abc");
    list.add(1); // 想要添加整数的话会报错
    // 使用迭代器遍历list集合
    Iterator<String> it = list.iterator();
    while (it.hasNext) {
        String s = it.next();
        Sout(s + "->" + s.length());
    }
}
```

### 定义和使用含有泛型的类

```java
// 定义一个含有泛型的类，可以模拟ArrayList集合
// 泛型是一个未知的数据类型，可以接收任意的数据类型
public class GenericClass<E> {
    private E name;
    
    public E getName() {
        return name;
    }
    
    public void setName(E name) {
        this.name = name;
    }
}

public class test {
    psvm() {
        // 不写泛型的话默认为Object类型
        GenericClass gc = new GenericClass();
        gc.setName("This is a string");
        // 所以返回的也默认是Obejct类型
        Object obj = gc,getName();
        GenericClass<Integer> gc2 = new GenericClass<>();
        gc.setName(1);
        Integer name = gc2.getName();
        sout(name);
    }
}
```

### 含有泛型的方法

```java
// 泛型定义在方法的修饰符和返回值类型之间
// 格式： 修饰符 <泛型> 返回值类型 方法名(参数列表(使用泛型))
// 在调用方法的时候确定泛型的数据类型
public class GenericMethod {
    public <M> void method01(M m) {
        Sout(m);
    }
}

public class test {
    psvm() {
        GenericMethod gm = new GenericMethod();
        // 调用含有泛型的方法
        gm.method01(10);
        gm.method01("abc");
    }
}
```

### 含有泛型的接口

```java
public interface GenericInterface<I> {
    public abstract void method(I i);
}
// 第一种方式：定义接口的实现类，实现接口，指定接口的泛型
public class GenericInterfaceImpl1 implements GenericInterface<String> {
    @Override
    public void method(String s) {
        Sout(s);
	}
}
// 第二种方式：接口使用什么泛型，实现类就使用什么泛型，类跟着接口走
// 就相当于定义了一个含有泛型的类，创建对象的时候确定泛型的类型
public class GenericInterfaceImpl2<I> implements GenericInterface<I> { // 用的都是<I>
    @Override
    public void method(I i) {
        sout(i);
    }
}
// 测试含有泛型的接口
public class test {
    psvm() {
        GenericInterfaceImpl1 gi1 = new GenericInterfaceImpl1();
        gi1.method("This is a string");
        
        GenericInterfaceImpl1<Integer> gi2 = new GenericInterfaceImpl2<>();
        gi2.method(10);
    }
}
```

第一种方法的一个实例：

```java
public interface Iterator<E> {
    E next();
}
// Scanner类实现了Iterator接口并指定接口的泛型为字符串
// 所以重写的next方法默认为String类型
public final class Scanner implements Iterator<String> {
    public String next();
}
```

第二种方法的一个实例：

```java
public interface List<E> extends Collection<E> {
    boolean add(E e);
    E get(int index);
}
// 用的都是E
public class ArrayList<E> implements List<E> {
    public boolean add(E e) {
        // ...
    }
    public E get(int index) {
        // ...
    }
}
```

### 泛型通配符

在使用泛型类或者接口时，传递的数据中，泛型类型不确定，可以通过通配符<?>表示。但是一旦使用泛型的通配符后，只能使用Object类中的共性方法，集合中元素自身的方法无法使用。

```java
// 不能创建对象使用，只能作为方法的参数使用
public class Generic {
    psvm() {
        ArrayList<Integer> list1 = new ArrayList<>();
        list1.add(1);
        list1.add(2);
        
        ArrayList<String> list2 = new ArrayList<>();
        list2.add("a");
        list2.add("b");
        
        // 定义一个方法，能遍历所有类型的ArrayList集合
        // 这时候我们不知道ArrayList集合使用什么数据类型，可以用泛型通配符?来接收数据类型
        // 泛型是没有继承概念的，所以<>里不能写Object，只能写'?'
        public static void printArray(ArrayList<?> list) {
            // 使用迭代器遍历集合
            Iterator<?> it = list.iterator();
            while (it.hasNext()) {
                // it.Next()方法，取出的元素是Object，可以接收任意的数据类型
                Object o = it.hasNext();
                Sout(o);
            }
        }
    }
}
```

#### 通配符的高级使用——受限泛型

之前设置泛型的时候，实际上是可以任意设置的，只要是类就可以设置。但是在java的泛型中可以指定一个泛型的上限和下限。

泛型的上限：

格式：类型名称 <? extends E> E对象名称

意义：只能接受E类型及其子类

泛型的下限：

格式：类型名称 <? super E> 对象名称

意义：只能接收E类型及其父类

```java
// 现已知类Object, String, Number, Integer, 其中Number是Integer的父类
psvm() {
    Collection<Integer> list1 = new ArrayList<Integer>();
    Collection<String> list2 = new ArrayList<String>();
    Collection<Number> list3 = new ArrayList<Number>();
    Collection<Object> list4 = new ArrayList<Object>();
    
    getElement1(list1);
    getElement1(list2); // 报错
    getElement1(list3);
    getElement1(list4); // 报错
    
    getElement2(list1); // 报错
    getElement2(list2); // 报错
    getElement2(list3);
    getElement2(list4);
}
// 上限：此时的泛型?必须是Number类型或者Number的子类
public static void getElement1(Collection<? extends Number> coll) {}
// 下限：此时的泛型?必须是Number类型或者Number的父类
public static void getElement2(Collection<? super Number> coll) {}
```

## List集合

java.util.List接口继承自Collection接口，是单列集合的一个重要分支，习惯性地会像实现了List接口的对象称为LIst集合。在List集合中允许出现重复的元素，所有的元素是以一种线性的方式进行存储的，在程序中可以通过索引来访问集合中的指定元素。另外，List集合还有一个特点就是元素有序，即元素的存入顺序和取出顺序一致。

### ArrayList

基于动态数组，查询快，增删慢。此实现不是同步的，可以多线程，速度快。

```java
transient Object[] elementData;
```

### LinkedList

基于双向链表，查询慢，增删快。此实现不是同步的。

找到头和尾非常方便，有大量操作头尾元素的方法。

```java
public void addFirst(E e) {}
public void addLast(E e) {}
public E getFirst() {}
public E getLast() {}
public E removeFirst() {}
public E removeLast() {}
```

### Vector

基于动态数组。此实现是同步的，不支持多线程。安全，但是慢。

## Set接口

Set接口和List接口一样继承自Collection接口，是Collection接口比较重要的两个子接口。

不包含重复元素，没有带索引的方法，也不能用for循环遍历。

两个主要的实现类：HashSet

### HashSet

此类实现Set接口，由哈希表（实际上是一个HashMap）支持。它不保证set迭代顺序，特别是它不保证该顺序恒久不变。此类允许使用null元素。

是一个无序的集合，存储元素和取出元素的顺序有可能不一致。

底层是一个哈希表结构，查询的速度非常的快。

实现不是同步的。

```java
psvm() {
    Set<Integer> set = new HashSet<>();
    set.add(1);
    set.add(2);
    set.add(3);
    set.add(1);
    // 使用迭代器遍历set集合
    Iterator<Integer> it = set.Iterator();
    while (it.hasNext()) {
        Integer n = it.next();
        sout(n);
    }
    // 1 2 3
    // 使用增强for循环遍历set集合
    for (Integer i : set)
        sout(i);
}
```

#### 哈希表

哈希值：是一个十进制的整数，由系统随机给出（就是对象的地址值，是一个逻辑地址，是模拟出来的地址，不是数据实际存储的物理地址）

##### Object的hashCode()方法

```java
public class Person 
```

### HashMap

HashMap存储自定义类型必须重写hashCode和equals方法，以确定键值唯一。

key不允许重复，无序的集合。

### LinkedHashMap

LinkedHashMap extends HashMap

由哈希表＋链表实现，具有可预知的迭代顺序

链表用来记录元素的顺序

输入的顺序和输出的顺序一致

key不允许顺序，有序的集合。

### HashTable

Map接口的一个实现类，底层也是一个哈希表，和HashMap一样。

HashTable不能存储null值或键(NullPointerException)，是一个早期的Map实现

null和null也是键值冲突，之后的null会把之前的取代

HashTable是线程安全的，速度慢。

HashTable和Vector一样，在JDK1.2之后被取代了。

取代Vector的是ArrayList

取代HashTable的是HashMap

但HashTable的子类Properties仍然活跃

Properties集合是一个唯一个I/O流相关的集合

### JDK9对集合添加的优化

List接口，Set接口，Map接口：里面增加了一个静态方法of，可以给集合一次性添加多个元素

```java
static <E> List<E> of(E... elements)
```

使用前提：当集合中存储的元素的个数已经确定，不再改变

注意：

1.  of方法只适用于这三个接口本身，不适用于这三个接口的实现类

2.  of方法的返回值是一个不能改变的集合，这个集合不能再使用add，put方法添加元素
3.  Set接口和Map接口在调用of方法的时候，不能含有重复的元素，否则会抛出异常

```java
psvm {
    List<String> list = List.of("beijing", "shanghai", "shenzhen");
    Sout(list);
    // ["beijing", "shanghai", "shenzhen"]
    list.add("Guangzhou"); // UnsupportOperationException:不支持操作异常
    // Set集合不能包含重复元素，否则会报非法参数异常:IlligalArgumentException
    Set<String> set = Set.of("beijing", "shanghai", "shenzhen");
    sout(set);
    // ["beijing", "shanghai", "shenzhen"]
    // 同样不能再添加元素了
    Map<String, Integer> map = Map.of("张三", 18, "李四", 19, "王五", 20);
    sout(map);
    // 张三=18, 李四=19, 王五=20
    // Map同样不支持存储重复的键，如果这样：
    Map<String, Integer> map1 = Map.of("张三", 18, "李四", 19, "王五", 20, "张三", 19);
    // IlligalArgumentException
    // 同样不能再添加元素了
}
```

## 异常

异常指的是程序在执行过程中产生的非正常的情况，最终会导致JVM的停止。

在Java等面向对象编程语言中，异常本身是一个类，产生异常就是创建异常对象并抛出了一个异常对象，Java处理异常的方式是中断处理。

异常的根类是java.lang.Throwable

其下有两个子类: java.lang.Error(非常严重的问题)与java.lang.Exception

平常所说的异常指后者

把异常处理掉，程序可以继续执行。

Error则必须修改源代码，程序才能继续执行。

### 异常的处理方式

#### 第一种

一种是在类的后面添加throws xxxException，交给JVM处理

JVM处理的方式就是中断处理，把异常打印在控制台并停止执行程序

比如date字符串转换为日期的时候会抛出的ParseException

```java
public static void main(String args[]) throws ParseException {
    // ...
}
```

#### 第二种

try-catch方法，程序可以在这个语句后正常执行。

```java
// 编译期异常，进行编译java程序出现的问题
try {
    date = sdf.parse("1999-0910");
} catch (ParseException e) {
    e.printStackTrace
}
sout("a"); // 这句话可以正常执行

// 运行期异常
int[] arr = {1, 2, 3};
try { // try语句块里写可能会出现错误的代码
    sout(arr[3]); // IndexOutOfBound
} catch(Exception e) { // 异常的处理
    sout(e); // java.lang.ArrayIndexOutOfBoundException
}   
```

### JVM对异常的处理

访问了数组中的越界索引3，这时候，JVM就会检测出程序出现异常，JVM会做两件事情：

1.  JVM会根据异常产生的原因创建一个异常对象，这个异常对象包含了异常产生的内容、原因、位置

   ```java
   new ArrayIndexOutOfBoundsException("3");
   ```

2.  在getElement方法中没有异常的处理逻辑(try...catch)，JVM就会把异常对象抛出给方法的调用者main方法来处理这个异常。

main方法接收到了接收到了这个异常对象，main方法也没有异常的处理逻辑，继续把对象抛出给main方法的调用者JVM

JVM接收到了这个异常对象，又做了两件事情

1. 把异常对象（内容、原因、位置）以红色的字体打印在控制台
2.  JVM会终止当前正在执行的java程序 --> So called interrupt

### throw关键字

作用：在指定的方法中抛出指定的异常

使用格式:

throw new xxxException

注意:

1. throw关键字必须写在方法内部
2. throw关键字后面new的对象必须是Exception或者Exception的子类对象
3.  throw关键字抛出指定的异常对象，我们就必须处理这个异常对象

throw关键字后面创建的是RuntimeException或者是RuntimeException的子类对象，我们可以不处理，默认交给JVM处理

throw关键字后面创建的是编译异常，我们就必须处理这个异常，throws/try...catch

```java
public static int getElement(int[] arr, int index) {
    // 参数合法性校验
    if (arr == null) {
        // 是一个运行期异常，不用处理，交给JVM
        throw new NullPointerException("传递的数组为空");
    }
    if (index < 0 || index > arr.length - 1) {
        // 也是运行期异常
        throw new ArrayIndexOutOfBoundsException("索引越界");
    }
    int e = arr[index];
    return e;
}
```

### Objects非空判断

Objects类提供了很多静态方法，其中方法requireNonNull可以判断对象是否为空，在写代码的时候直接调用这个方法即可，不用自己判断，requireNonNull的源码如下：

```java
public static <T> T requireNonNull(T obj) {
    if (obj == null) {
        throw new NullPointerException();
    }
    return obj;
}
// 调用上面的方法
pubic static void method(Object obj) {
    Objects.requireNonNull(obj);
    Objects.requireNonNull(obj, "传递的对象的值为空");
}
```

###  thorws关键字

异常处理的第一种方式，交给别人处理

作用：当方法内部抛出异常对象的时候，那么我们就必须处理这个异常对象

可以使用throws关键字处理异常对象，会把异常对象声明抛出给方法的调用者，自己不处理，别人处理，最终交给JVM处理 --> 中断处理

使用格式：在方法声明时使用

修饰符 返回值类型 方法名(参数列表)  throws AAAException, BBBException {

​	throw new AAAException("reason");

​	throw new BBBException("reason");

​	// 抛出多个异常

}

注意：

throws关键字必须写在方法声明处

throws关键字后边声明的异常必须是Exception或者是Exception的子类

方法内部如果抛出了多个异常对象，throws后面必须也声明多个异常

如果抛出的多个异常对象有子父类关系，直接声明父类异常即可

调用了一个声明抛出异常的方法，我们就必须处理声明的异常

处理方式：

要么继续使用throws，把异常交给方法的调用者，最终交给JVM

要么try...catch，自己处理异

```java
// 异常处理的第一种方式
// 定义一个方法，对传递的文件路径进行合法性判断，如果路径不是"c:\\a.txt"，那么我们就抛出文件找不到异常
// 注意：文件找不到异常是编译异常，抛出了编译异常就必须处理这个异常
// 可以使用throws继续声明抛出FileNotFoundException这个异常对象，让方法的调用者处理
// 如果传递的路径不是.txt结尾，那么我们就抛出IO异常对象，告知方法的调用者后缀名不对
// FileNotFoundException是IOException的子类，无需声明
psvm() {
    public static void readFile(String fileName) throws IOException {
        if (fileName.equals("c:\\a.txt")) {
            throw new FileNotFoundException("Wrong Path");
        }
        if (!filename.endsWith(".txt")) {
            throw new IOException("Wrong Suffix");
		}
        sout("Reading file..."); // 这句话不会被打印，因为JVM会中断处理
    }
}
```

### 捕获异常try...catch

异常处理的第二种方式，自己处理异常

格式：

```java
try {
    // code that might throw Exception
} catch(异常类名 变量名) { // 定义一个异常变量，用来接收try中抛出的异常对象
    // 异常的处理逻辑，产生异常对象之后怎么处理异常对象
    // 一般会把异常的信息记录到日志中
}
```

注意：

1. try中可能会抛出多个异常对象，那么就可以使用多个catch来处理这些异常对象zhi
2. 如果try中产生了异常，那么就会执行catc中的异常处理逻辑，执行完毕catch中的处理逻辑，继续执行try...catch之后的代码。如果try中没有产生异常，那么就不会执行catch中异常的处理逻辑，执行完try中的代码，就行执行try...catch之后的代码

```java
psvm() {
	try {
        readFile("c:\\a.jpg");
    } catch (IOException e) { 
        // try中抛出什么异常对象，catch就定义什么异常变量，用来接收这个异常对象
        sout("传递的文件后缀不是.txt");
    }
    sout("后续代码");
}
```

### 获取异常信息

Throwable类中定义了一些查看方法：

- public String getMessage(): 获取异常的描述信息，原因（提示给用户的时候，就提示错误原因）
- public String toString(): 获取异常的类型和异常描述信息（不用）
- public void printStackTrace(): 打印异常的跟踪栈信息并输出到console，打印的异常信息是最全面的

```java
psvm() {
	try {
        readFile("c:\\a.jpg");
    } catch (IOException e) { 
        sout(e.getMessage()); // Wrong Suffix
        sout(e.toString()); // java.io.IOException: 文件的后缀名不对
        e.printStackTrace(); // 还会打印异常产生的位置、原因
    }
    sout("后续代码");
}
```

### finally代码块

```java
psvm() {
	try {
        readFile("c:\\a.jpg");
        sout("资源释放"); // 如果抛出了异常，这句话不会被执行
    } catch (IOException e) { 
        sout(e.getMessage()); // Wrong Suffix
        sout(e.toString()); // java.io.IOException: 文件的后缀名不对
        e.printStackTrace(); // 还会打印异常产生的位置、原因
    }
    sout("后续代码");
}
```

格式：

```java
try {
    // code that might throw Exception
} catch(异常类名 变量名) { // 定义一个异常变量，用来接收try中抛出的异常对象
    // 异常的处理逻辑，产生异常对象之后怎么处理异常对象
    // 一般会把异常的信息记录到日志中
} finally {
    // 无论是否出现异常都要执行
}
```

注意：

1. finally不能单独使用，必须和try一起使用
2. finally一般用于资源释放/回收，无论程序是否出现异常，最后都要释放资源(IO)

```java
psvm() {
    try {
    	readFile("c:\\a.jpg");    
    } catch (IOException e) {
        e.printStackTrace();
    } finally { // 无论是否出现异常都会执行
		sout("资源释放");
    }
}
```

### 异常注意事项

#### 多个异常使用捕获如何处理

1. 多个异常分别处理
2. 多个异常一次捕获，多次处理
3. 多个异常一次捕获，一次处理

一般我们是使用一次捕获多次处理方式，格式如下：

```java
try {
    // 可能会出现异常的代码
} catch (异常类型A e) { // 当try中出现A类型异常，就用该catch来捕获
    // 处理异常的代码
} catch (异常类型B e) { // 当try中出现B类型异常，就用该catch来捕获
    // 处理异常的代码
}
```

一个try多个catch注意事项：

catch里边定义的异常变量，如果有父子类关系，那么子类的异常必须写在上边，否则就会报错。

```java
// 这么写会报错，因为两个异常有父子类关系
try {
    int[] arr = {1, 2, 3};
    sout(arr[3]);
    List<Integer> list = List.of(1, 2, 3);
    sout(list.get(3));
} catch (IndexOutOfBoundsException e) {
    sout(e);
} catch (ArrayIndexOutOfBoundsException e) {
    sout(e);
}
sout("后续代码");
```

为什么会报错？

try中如果出现了异常对象，会把异常对象抛出给catch处理

抛出的异常对象，会从上到下依次赋值给catch中定义的异常变量

正常情况：

ArrayIndexOutOfBoundsException e = new ArrayIndexOutOfBoundsException();

IndexOutOfBoundsException e = new IndexOutOfBoundsException();

错误情况：

IndexOutOfBoundsException e = new ArrayIndexOutOfBoundsException(); // 多态，使得下面的catch语句块不会被用到

IndexOutOfBoundsException e = new IndexOutOfBoundsException();

一次捕获一次处理：就是把下面的catch删除，如果我们的异常可以用一个异常对象处理就这么用

不管什么异常都是Exception，无论什么异常都可以用catch Exception来处理



运行时异常被抛出可以不处理。即不捕获也不声明抛出。

默认会给虚拟机处理，终止程序。

什么时候不抛出运行时异常了，再来继续执行程序。

#### finally中有return语句

如果finally中return语句，永远返回finally中的结果，避免该情况

```java
public static int getA() {
    int a = 10;
    try {
        return a;
    } cacth (Exception e) {
        sout(e);
    } finally {
        a = 100;
        return a;
    }
}

psvm() {
    int a = getA();
    sout(a); // 100
}
```

#### 子父类的异常

1. 如果父类抛出了多个异常，子类重写父类方法时，抛出和父类相同的异常或者是父类异常的子类，或者不抛出异常
2.  父类方法没有抛出异常，子类重写父类方法时，此时子类产生该异常，只能捕获处理，不能声明抛出。

父类异常什么样，子类异常就什么样就行了

```java
public class Fu {
    public void show01() throws NullPointerException, ClassCastException{}
    public void show02() throws IndexOutOfBoundsException{}
    public void show03() throws IndexOutOfBoundsException{}
    public void show04() throws IndexOutOfBoundsException{}
}

class Zi extends Fu {
    // 抛出相同异常
    public void show01() throws NullPointerException, ClassCastException{}
    // 抛出父类异常的子类
    public void show02() throws ArrayIndexOutOfBoundsException{}
    // 不抛出异常
    public void show03() {}
    // 子类不能抛出异常，只能捕获
    public void show04() { // 不能声明throws exception
        throw new Exception("编译期异常"); // usr try catch
    }
}
```

### 自定义异常类

java提供的异常类不够我们使用，需要自己定义一些异常类

格式：public class XXXException exteneds Exception/RuntimeException {

​	添加一个空参数的构造方法

​	添加一个带异常信息的构造方法

}

注意：

1. 自定义异常类一般都是以Exception结尾
2. 自定义异常类必须继承Exception(编译异常)或者RuntimeException(运行期异常)

```java
public class RegisterException extends Exception {
    public RegisterException{
        super();
    }
    
    // 所有的异常类都会有一个带异常信息的构造方法，方法内部会调用父类带异常信息的构造方法，让父类来处理这个信息
    public RegisterException(String s) {
        super(s);
    }
}
```

## 多线程

### 并发与并行

并发：两个或多个事件在同一时间段内发生

并行：两个或多个时间在同一时刻发生（同时发生）

### 主线程

主线程：执行main方法的线程

单线程程序：java程序中只有一个线程

执行从main方法开始，从上到下依次执行

```java
public class Person {
    private String name;
    // 构造函数
    public void run() {
        for (int i = 0; i < 20; i++) {
            sout("name" + "-->" + i);
        }
    }
}

public class Main {
    psvm() {
        Person p1 = new Person("Alice");
        p1.run();
        Person p2 = new Person("Bob");
        p2.run();
    }
}

// p1执行20次，p2执行20次
```

### 创建线程类

#### 第一种方式：创建Thread的子类

java.lang.Thread类：是描述线程的类，想要实现多线程的程序，必须实现Thread类

实现步骤：

1. 创建一个Thread类的子类
2. 在Thread的子类中重写Thread中的run方法，设置线程任务（这个线程要做什么）
3. 创建对象
4. 调用Thread类中的start方法，开启新的线程，执行run方法

多次启动一个线程是非法的，特别是一个线程已经执行完毕后，不能再重新启动

java程序属于抢占式调度，哪个线程的优先级高就优先执行，相同优先级则随机选择一个执行

```java
public class MyThread extends Thread {
    public void run() {
        for (int i = 0; i < 20; i++) {
			sout("MyThread:" + i);
        }
    }
}

public class Main {
    psvm() {
        MyThread mt = new MyThread();
        mt.start();
        for (int i = 0; i < 20; i++) {
            sout("Main:" + i);
        }
    } // 每次执行的结果不一样
}
```

#### 第二种方式：实现Runnable接口的实现类

然后实现run方法，然后可以分配该类的实例，再创建Thread时作为一个参数来传递并启动。

Thread有构造函数传递一个Runnable的实现类作为参数

Runnable接口应该由那些打算通过某一线程执行其实例的类来实现。类必须定义一个称为run的无参数方法

实现步骤：

1. 创建一个Runnable接口的实现类
2. 再实现类中重写Runnable接口的run方法，设置线程任务
3. 创建一个Runnable接口的实现类对象
4. 创建Thread类对象，构造方法中传递Runnable接口的实现对象
5. 调用Thread类中的start方法，开启新的线程执行run方法

```java
// implement Runnable interface
public RunnableImpl implements Runnable {
    @Override
    public void run() {
        for (int i = 0; i <= 60; i++) {
            sout(Thread.CurrentThread().getName() + "-->" + i);
        }
    }
}

psvm() {
    RunnableImpl run = new RunnableImpl();
    Thread t = new Thread(run);
    t.start();
	for (int i = 0; i <= 60; i++) {
            sout(Thread.CurrentThread().getName() + "-->" + i);
    }
}
// 交替打印结果
```

#### Thread和Runnable的区别

实现Runnable接口创建多线程程序有什么好处？

1.避免了单继承的局限性：

一个类只能继承一个类，类继承了Thread类，就不能继承其他的类

实现了Runnable接口，仍然可以继承其他的类和接口

2.增强了程序的拓展性，降低了程序的耦合性（解耦）

实现Runnable接口的方式，把设置线程任务和开启新线程进行了分离（解耦）

实现类中，重写了run方法：用来设置线程任务

创建Thread类对象，调用start方法：用来开启新线程

写多线程程序的时候，尽量用Runnable方法

#### 匿名内部类方式实现线程创建

使用线程的内匿名内部类方式，可以方便的实现每个线程执行不同的线程任务操作

使用匿名内部类的方式实现Runnable接口，重写Runnable接口中的run方法

匿名内部类的作用：简化代码

把子类继承父类，重写父类的方法，创建子类对象合一步完成

把实现类实现接口，重写接口中的方法，创建实现类对象合一步完成

匿名内部类最终产物：子类/实现类对象，而这个类没有名字

格式：

new 父类/接口() {

​	重写父类/接口中的方法

}

```java
psvm() {
    // 继承Thread类的方式
    new Thread() {
        @Override
        public void run() {
            for (int i = 0; i < 20; i++) {
                sout(Thread.currentThread.getName() + 'i');
            }
        }
    }.start();
    
    // 实现Runnable接口的方式
    Runnable r = new Runnable() {
        @Override
        for (int i = 0; i < 20; i++) {
                sout(Thread.currentThread.getName() + 'i');
        }
	}
    new Thread(r).start();
    
    // 简化
    new Thread(new Runnable(){...}).start();
}
```



### Thread.start()和Thread.run()的内存映像

![屏幕截图(5)](C:\Users\liuwe\Pictures\Screenshots\屏幕截图(5).png)

只有当执行线程的start()方法时，JVM才会为其分配独立的栈空间，从而达到多线程的执行效果。

执行start()方法，JVM会调用此线程的run()方法。

而如果只是执行线程的run()方法时，JVM会在主线程的栈空间中为线程的run()方法分配空间，仍然是顺序执行。

### Thread类中的常用方法

#### 获取线程的名称：

1. getName()
2. 可以先获取到当前正在执行的线程，再使用线程线程中的方法getName()获取线程的名称

static Thread currentThread() 是一个静态方法，返回对当前正在执行的线程对象的引用

sout(Thread.currentThread().getName())

#### 设置线程的名称

1. setName()
2. 创建一个带参数的构造方法，参数传递线程的名称

### sleep()

param: t

t毫秒过后线程停止执行

```java
// 一秒钟打印一次for循环的内容
psvm() {
    for (int i = 0; i <= 60; i++) {
        sout(i);
        try {
            Thread.sleep(1000); // This is a static method
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

## 线程安全

解决线程安全问题的三种方案

### 同步代码块

```java
synchronized(lock object) {
    // code here(访问了共享对象)
}
```

注意：

1. 同步代码块中的锁对象，可以使用任意的对象
2. 但是必须保证多个线程使用的锁对象是同一个

创建一个锁对象：

Object obj = new Object() // 可以是任意对象，创建在run方法的外面

同步技术的原理：

使用了一个锁对象，这个锁对象叫做同步锁，也叫做对象监视器

3个线程一起抢夺cpu的执行权，谁抢到了谁执行run方法

t0抢到了cpu的执行权，执行run方法，遇到synchronized代码块，这时t0会检查synchronized代码块是否有锁对象

发现有，就会获取到锁对象，线程执行

t1抢到了cpu的执行权，执行run方法，发现没有obj锁对象，就会进入到阻塞态，会一直等待t0线程归还锁对象

一直到t0线程执行完同步中的代码块，会把锁对象归还

同步代码块t1才能获取到锁对象进入到同步中执行

总结：同步中的线程，没有执行完毕不会释放锁，同步外的锁没有锁进不去同步

出了同步，会把锁对象归还

问题：程序频繁的判断锁，获取锁，释放锁，使得效率降低

### 同步方法

使用步骤：

1. 把访问了共享数据的代码块抽取出来，放到一个方法中
2. 在方法上添加synchronized关键字

```java
// 定义一个同步方法
public synchronized void payTicket() {
    // ...
}
```

同步方法也会把方法内部的代码锁住，锁住之后只让一个线程执行

同步方法的锁对象默认就是实现类对象，也就是new RunnableImpl，也就是this

相当于

```java
synchronized(this) {
    // code here
}
```

也可以定义一个静态的同步方法

这时候锁对象就不能是this了

this的创建对象之后产生的，静态方法优于对象，在对象进入内存之前就进入内存了

静态方法的锁对象是本类的class属性-->class文件对象（反射）

相当于

```java
synchronized(RunnableImpl.class) {
    // code here
}
```

### Lock锁

在java1.5之后提供了一个Lock接口

在java.util.concurrent.locks.Lock中

Lock机制提供了比synchronized代码块和synchronized方法更广泛的锁定操作，同步代码块/同步方法具有的功能Lock都有，除此之外更强大，更体现面向对象

#### lock和unlock

lock接口的一个实现类：ReenrantLock

使用步骤：

1. 在成员位置创建一个ReentrantLock对象
2. 在可能会出现安全问题的代码前调用Lock接口中的方法lock获取锁
3. 在可能会出现安全问题的代码后调用Lock接口中的方法unlock释放锁

```java
public RunnableImpl implements Runnable {
    private int tickets = 100;
    Lock l = new ReentrantLock();
    
    public void run() {
        while (true) {
            l.lock;
        }
        if (tickets > 0) {
            try {
                Thread.sleep(10);
                sout(Thread.currentThread().getName() + tickets);
                tickets--;
            } catch (InterruptException e) {
                e.printStackTrace();
            } finally { // 在finally代码块中归还锁
                l.unlock(); // 这样无论程序是否出现异常，锁对象都会释放掉
            }
        }
    }
}
```

## 线程状态

线程的状态是Thread类中的一个内部类，描述了线程的状态，共六种状态。

![屏幕截图(7)](C:\Users\liuwe\Pictures\Screenshots\屏幕截图(7).png)

NEW新建状态

RUNNABLE运行状态

BLOCKED阻塞状态

TERMINATED死亡状态

TIMED_WAITING计时等待

WAITING无限等待状态

休眠状态可以自己醒，但无限等待状态需要外部唤醒，这两种状态都主动放弃了CPU的执行权

### TIMED_WAITING

就是计时等待，示例代码：每秒打印for循环

进入到TIMED_WAITING有两种方式：

1. 使用sleep(long ms) 方法，在毫秒值结束之后，线程睡醒进入到Runnable/Blocked状态
2. 使用wait(long ms)方法，wait方法如果在毫秒值结束之后还没有被notify，它就会自动醒来，无参就必须被唤醒



### BLOCKED

可以运行，但是要和其他线程争夺CPU的使用权

### WAITING

一个正在无限期等待另一个线程执行一个特别的（唤醒）动作的线程处于这一状态

等待唤醒案例：线程之间的通信

同步使用的锁对象必须唯一

只有锁对象可以调用wait和notify方法

### notify和notifyAll

notify唤醒此对象监视器上等待的单个线程

notifyAll唤醒此对象监视器上等待的所有线程

### 线程间通信

wait(): 线程不再活动，不再参与调度，进入wait set中，因此不会浪费CPU资源，也不会去竞争锁了，这是的线程状态即是WAITING。它还要等着别的线程执行一个特别的动作，也就是notify在这个对象上等待的线程从wait set中释放出来，重新进入到调度队列当中。

notify(): 则选取所通知对象的wait set中的一个线程释放；例如，餐馆有空位置后，等候就餐最久的顾客最先入座

notifyAll(): 则释放所通知对象的wait set上的全部线程

注意：哪怕只通知了一个等待的线程，被通知线程也不能立即恢复执行，因为它当初中断的地方是在同步块内，而此刻它已经不持有锁（wait方法会让当前线程归还它获得的锁），所以它需要再次尝试去获得锁（很可能面临其它线程的竞争），成功后才能在当初调用wait方法之后的地方恢复执行。

总结如下：

1. 如果能获取锁，线程就从WAITING状态变为RUNNABLE状态
2. 否则，从wait set出来，又进入entry set，线程就从WAITING状态又变成BLOCKED状态

## 线程池

如果并发的线程数量很多，并且每个线程都是执行一个时间很短的任务就结束了，这样频繁创建线程就会大大降低系统的效率，因为频繁创建线程和销毁线程需要时间。

那么有没有一种办法可以使得线程可以复用，就是执行完一个任务，并不被销毁，而是可以继续执行其他任务？

在Java中可以通过线程池来达到这样的效果。

线程池：容器-->集合(ArrayList, HashSet, LinkedList, HashMap)使用泛型：Thread

add(new Thread(xxx));

...

add(new Thread(xxx));

当程序第一次启动的时候，创建多个线程，保存到一个集合中

当我们想要使用线程的时候，就可以从集合中取出来线程使用

list.remove(0)：返回被移除的元素

线程只能被一个任务使用，返回了一个线程

linked.removeFirst(); 返回第一个线程

当我们使用完毕线程，需要把线程归还给线程池

list.add(t);   /  linked.addLast(t);

这是一种队列的现象

在JDK1.5之后，JDK内置了线程池，可以直接使用，不用自己创建集合了

线程池就是一个容纳线程的容器



在java.util.concurrent中有一个类Executors，这个类是生产线程池的工厂类

这个类中有一个静态方法: static ExecutorService newFixedThreadPool(int nThreads)创建一个线程池

返回值：ExecutorService是一个接口，返回的是这个接口的实现类对象，我们可以使用这个接口接收

接口ExecutorService中有一个方法submit

提交一个Runnable任务用于执行，并返回一个表示该任务的Future



关闭/销毁线程池的方法：shutdown()



使用步骤：

1. 使用线程池的工厂类提供的静态方法newFieldThreadPool生产一个指定数量的线程池
2. 创建一个类，实现Runnable接口，重写run方法，设置线程任务
3. 调用ExcutorService中的方法submit，传递线程任务(实现类)，开启线程，执行run方法
4. 调用ExcutorService中的方法shutdown，销毁线程池（不建议执行）

```java
public RunnableImpl implements Runnable {
    @Override
    public void run() {
		Sout(Thread.currentThread().getName());
    }
}

psvm() {
    ExecutorService es = Executors.newFixedThreadPool(2);
    es.submit(new RunnableImpl());
    es.submit(new RunnableImpl());
    es.submit(new RunnableImpl());
}
```

## Lambda表达式

### 函数式编程思想概述

在数学中，函数就是有输入量、输出量的一套计算方案，也就是“拿什么东西做什么事情”。相对而言，面向对象过分强调“必须通过对象的形式来做事情”，而函数式思想则尽量忽略面向对象的复杂语法——强调做什么，而不是以什么形式做。

面向对象的思想：做一件事情，找一个能解决这个事情的对象，调用对象的方法，完成事情

函数式编程思想：只要能获取到结果就可以，谁去做的，怎么做的都不关心，重视的是结果，不重视过程



### 冗余的Runnable代码

- Thread类需要Runnable接口作为参数，其中抽象run方法是用来指定线程任务的核心
- 为了指定run方法体，不得不需要Runnable方法的实现类
- 为了省去定义一个RunnableImpl实现类的麻烦，不得不使用匿名内部类
- 必须覆盖重写抽象run方法，所以方法名称，方法参数，方法返回值不得不再写一遍，且不能写错
- 而实际上，似乎只有方法体才是关键所在



### 编程思想转换

做什么，而不是怎么做

我们真的希望创建一个匿名内部类吗？不。我们只是为了做这件事情而不得不创建一个对象。我们真正希望做的事情是：将run方法体内的代码传递给Thread类知晓。

传递一段代码——才是我们真正的目的。而创建对象只是受限于面向对象语法而不得不采取的一种手段方式。那，有没有更加简单的方法？



### 体验Lambda表达式

```java
// 使用lambda表达式实现多线程
psvm() {
    new Thread(()->{ // 括号里面的是参数列表，括号里的参数传递给箭头右边的方法体
        sout(Thread.currentThread().getName());
    	}
    ).start();
}
```



### Lambda标准格式

格式由3个部分组成：

- 一些参数
- 一个箭头
- 一段代码

(参数类型 参数名称) -> { 一些重写方法的代码语句 }



### 有参数无返回值的代码案例

```java
public class Person {
    String name;
    int age;
    // Getters, Setters and Constructors
}

public class Test {
    psvm() {
        Person[] arr = {
            new Person("Alice", 38);
            new Person("Bob", 18);
            new Person("Carol", 19);
        };
        
        Arrays.sort(arr, new Comparator<Person>() {
            @Override
            public int compare(Person o1, Person o2) {
                return o1.getAge() - o2.getAge();
            }
        });
        
        for (Person p : arr)
            sout(p);
        
        // 使用lambda表达式简化匿名内部类
        Arrays.sort(arr, (Person o1, Person o2)->{
            return o1.getAge() - o2.getAge();
        })
    }
}
```



### 有参数无返回值的代码案例

```java
public interface Calculator {
    // 定义一个计算两个int和的方法并返回结果
    int calc(int a, int b);
}

public class Test {
    psvm() {
        invokeCalc(10, 20, new Calculator(){
            @Override
            public int calc(int a, int b) {
                return a + b;
            }
        });
        
        // 使用lambda表达式简化匿名内部类书写
        invokeCalc(10, 20, (a, b)->{
            return a + b;
        });
    }
    
    // 定义一个方法，参数传递两个intL，参数传递Calculator接口，方法内部调用Calculator中的方法calc
    public static void invokeCalc(int a, int b, Calculator c) {
        int sum = c.calc(a, b);
        sout(sum);
    }
}
```



Lambda表达式：可推到，可省略

凡是根据上下文推到出来的内容，都可以省略书写

可以省略的内容：

1. （参数列表）：括号中参数列表的数据类型可以不写
2. （参数列表）：括号中的参数如果只有一个，那么类型和类型和()都可以省略
3.  {一些代码}：如果{}中的代码只有一行，无论是否有返回值，都可以省略({}, return, 分号)
4. 注意：这三个东西必须一起省略

优化省略：

```java
new Thread(()->{ // 括号里面的是参数列表，括号里的参数传递给箭头右边的方法体
        sout(Thread.currentThread().getName());
    	}
).start();

// 优化：省略{}, 分号, return(本来就没有)
new Thread(()->sout(Thread.currentThread().getName())).start();

Arrays.sort(arr, (Person o1, Person o2)->{
            return o1.getAge() - o2.getAge();
})
    
// 优化：
Arrays.sort(arr, (Person o1, Person o2)->o1.getAge()-o2.getAge());
```



### Lambda的使用前提

1. 使用Lambda必须有接口，且要求接口中有且仅有一个抽象方法。

   无论是JDK内置的Runnable、Comparator接口还是自己定义的接口，只有当接口中的抽象方法存在且唯一时，才可以使用Lambda

2. 使用Lambda必须具有上下文推断

   也就是方法的参数或局部变量类型必须为Lambda对应的接口类型，才能使用Lambda作为该接口的实例

备注：有且仅有一个抽象方法的接口，称为“函数式接口”



## File类

java.io.File类是文件和目录路径名的抽象表示，主要用于文件和目录的创建、查找和删除等操作

java把电脑中的文件和文件夹封装成了一个File类，我们可以使用File类对文件和文件夹进行操作

我们可以：

创建一个文件/文件夹

删除文件/文件夹

获取文件

判断文件是否存在

对文件夹进行遍历

获取文件大小

File类是一个与系统无关的类，任何的操作系统都可以使用这个类中的方法

file: 文件 directory: 文件夹/目录 path:路径

```java
static String pathSeparator；
// 与系统有关的路径分隔符，为了方便，它被表示为一个字符串
// 在windows系统中是分号；在linux系统中是冒号：
static String separator;
// 与系统有关的默认名称分隔符
// 在windows系统中的反斜杠\ 在linux系统中的正斜杠/

// 操作路径不能写死了，因为不知道服务器用的是什么系统
// "C:" + File.separator + "develop" + File.separator + "a" + File.separator + "a.txt"
```



### 路径

绝对路径：是一个完整的路径

以盘符（C: D:）开始的路径

C:\\\Users\\\itcast\\\IdeaProjects\\\shuangyuan



相对路径：是一个简化的路径

相对指的是相对于当前项目的根目录

如果使用当前项目的根目录，路径可以简化书写

C:\\\Users\\\itcast\\\IdeaProjects\\\shuangyuan\\\123.txt -->简化为123.txt

可以省略项目的根目录



注意：

1. 路径不区分大小写
2. 路径中的文件名称分隔符windows使用反斜杠，反斜杠是转义字符，两个反斜杠表示一个普通的反斜杠



### File类的构造方法

#### 第一个构造方法

用到的情况最多

File(String pathname) 通过将给定路径名字符串转换为抽象路径名来创建一个新File实例

参数：

String pathname：字符串的路径名称

路径可以以文件结尾，也可以以文件夹结尾

路径可以是相对路径或绝对路径

路径可以存在，也可以是不存在的

创建File对象只是把字符串的路径封装为File对象，不考虑路径的真假情况

```java
private static void show() {
    File f1 = new File("C:\\Users\\itcast\\IdeaProjects\\shuangyuan\\a.txt");
    sout(f1); // 打印的就是这个路径，说明File类重写了Object的toString方法
    
    File f2 = new File("C:\\Users\\itcast\\IdeaProjects\\shuangyuan");
    
    File f3 = new File("b.txt"); // 相对于当前项目的根目录
    sout(f3); // b.txt
}
```



#### 第二个构造方法

File(String parent, String child) 根据parent路径名字符串和child路径名字符串创建一个新File实例

参数：把路径分成了两部分

String parent: 父路径

String child: 子路径

好处：父路径和子路径可以单独书写，使用起来非常灵活，父路径和子路径都可以变化

```java
private static void show(String parent, String name) { 
    // 传参parent = "C:\\" child = "a.txt"
    File file = new File(parent, child);
    sout(file); // C:\a.txt
}
```



#### 第三个构造方法

File(File parent, String child)

父路径是File类型，可以使用File类的方法对路径进行一些操作，再使用路径创建对象

```java
private static void show() {
	File parent = new File("c:\\");
    File file = new File(parent, "hello.java");
    sout(file); // c:\hello.java
}
```



#### 第四个构造方法

URL以后再写



### 常用方法

#### 获取功能的方法

public String getAbsolutePath() 返回此File的绝对路径名字符串

public String getPath() 将此File转换为路径名字符串

public String name() 返回由此File表示的文件或目录的名称

public long length() 返回由此File表示的文件的长度

```java
private static void show01() {
    // getAbsolutePath  获取的都是绝对路径
    File f1 = new File("C:\\Users\\itcast\\IdeaProjects\\shuangyuan\\a.txt");
    sout(f1.getAbsolutePath()); // C:\Users\itcast\IdeaProjects\shuangyuan\a.txt
    
    File f2 = new File("a.txt");
    sout(f2.getAbsolutePath()); // C:\Users\itcast\IdeaProjects\shuangyuan\a.txt
    
    // getPath输出构造函数里的路径
    // File的toString方法调用的就是getPath方法
    
    // getName()获取的就是构造方法传递路径的结尾部分(文件/文件夹)
    
    // length()获取构造方法指向文件的大小，单位是字节
    // 文件夹是没有大小概念的，不能获得文件夹大小，会返回0
    // 如果构造方法中给出的路径不存在，返回0
    // 文件夹的属性和length是两个不同的概念
}
```

### 判断功能的方法

public boolean exists()方法：此File表示的文件或目录是否存在

file.exists(String path)



public boolean isDirectory：此File表示的是否为目录



public boolean isFile

注意：

电脑中只有文件/文件夹，所以这两个方法是互斥的

这两个方法的使用前提：路径必须存在，否则都返回false

### 创建删除功能的方法

public boolean createNewFile() iff. 具有该名称的文件尚不存在时，创建一个新的文件

此方法只能创建文件，不能创建文件夹，且创建的文件路径必须存在，否则会抛出IO异常

我们调用这个方法必须处理这个可能的异常

public boolean delete() 删除由此File表示的文件或目录

delete方法直接从硬盘删除文件或文件夹，不走回收站，使用需谨慎

public boolean mkdir() 创建由此File表示的目录

public boolean mkdirs() 创建由此File表示的目录，包括任何必须但不存在的父目录

### 遍历目录功能

public String[] list()

public File[] listFiles()



## Stream

OutputStream是一个抽象类，不可以直接创建对象

#### FileOutputStream

FileOutputStream是这个抽象类的一个子类

构造：

FileOutputStream(File file) // 目的地是一个文件

FileOutputStream(String name) // 目的地是一个路径

1. 创建一个FileOutputStream对象
2. 根据构造方法中的文件/文件路径，创建一个空的文件
3. 会把FileOutputStream对象指向创建好的文件

写入数据的原理（内存到硬盘）

java程序 --> JVM --> OS --> OS调用写数据的系统调用

步骤：

1. 创建一个FileOutputStream对象，构造方法中传递写入数据的目的地
2. 调用FileOutputStream对象中的write方法，把数据写到文件中
3. 释放资源，把使用的内存资源释放掉

```java
psvm throws IOException{
    // 这三个方法都可能抛出异常
    // 可能会抛出文件找不到异常，是IO异常的子类
    FileOutputStream fos = new FileOutputStream("xxx\\a.txt");
    
    fos.write(97);
    
    fos.close();
}
```

