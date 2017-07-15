

## 一、为什么需要泛型
### 示例代码1：

``` java
public class GenericTest {

    public static void main(String[] args) {
        List list = new ArrayList();
        list.add("yanbo");
        list.add("yin");
        list.add(100);

        for (int i = 0; i < list.size(); i++) {
            String name = (String) list.get(i); // 1注释
            System.out.println("name:" + name);
        }
    }
}
```

控制台打印结果

> Exception in thread "main" name:yanbo
name:yin
java.lang.ClassCastException: java.lang.Integer cannot be cast to java.lang.String
	at com.ys.test.GenericTest.main(GenericTest.java:31)


定义了一个List类型的集合，先向其中加入了两个字符串类型的值，随后加入一个Integer类型的值。这是完全允许的，因为此时list默认的类型为Object类型。在之后的循环中，由于忘记了之前在list中也加入了Integer类型的值或其他编码原因，很容易出现类似于//1注释中的错误。因为编译阶段正常，而运行时会出现“java.lang.ClassCastException”异常。因此，导致此类错误编码过程中不易发现。

 在如上的编码过程中，我们发现主要存在两个问题：

1.当我们将一个对象放入集合中，集合不会记住此对象的类型，当再次从集合中取出此对象时，改对象的编译类型变成了Object类型，但其运行时类型任然为其本身类型。

2.因此，//1注释处取出集合元素时需要人为的强制类型转化到具体的目标类型，且很容易出现“java.lang.ClassCastException”异常。

怎么解决这种问题呢？


----------


## 二、什么是泛型？
泛型，即“参数化类型”。一提到参数，最熟悉的就是定义方法时有形参，然后调用此方法时传递实参。那么参数化类型怎么理解呢？顾名思义，就是将类型由原来的具体的类型参数化，类似于方法中的变量参数，此时类型也定义成参数形式（可以称之为类型形参），然后在使用/调用时传入具体的类型（类型实参）。

### 示例代码2：

``` java
public class GenericTest {

    public static void main(String[] args) {
        List<String> list = new ArrayList<String>();
        list.add("yanbo");
        list.add("yin");
        list.add(100);   // 1注释  提示编译错误

        for (int i = 0; i < list.size(); i++) {
            String name = list.get(i); // 2注释
            System.out.println("name:" + name);
        }
    }
}

```

控制台打印结果

> Exception in thread "main" java.lang.Error: Unresolved compilation problem: 
	The method add(int, String) in the type List<String> is not applicable for the arguments (int)	at com.ys.test.GenericTest.main(GenericTest.java:41)

采用泛型写法后，在//1注释处想加入一个Integer类型的对象时会出现编译错误，通过List<String>，直接限定了list集合中只能含有String类型的元素，从而在//2注释处无须进行强制类型转换，因为此时，集合能够记住元素的类型信息，编译器已经能够确认它是String类型了。

结合上面的泛型定义，我们知道在List<String>中，String是类型实参，也就是说，相应的List接口中肯定含有类型形参。且get()方法的返回结果也直接是此形参类型（也就是对应的传入的类型实参）。下面就来看看List接口的的具体定义：

``` java
public interface List<E> extends Collection<E> {

    int size();

    boolean isEmpty();

    boolean contains(Object o);

    Iterator<E> iterator();

    Object[] toArray();

    <T> T[] toArray(T[] a);

    boolean add(E e);

    boolean remove(Object o);

    boolean containsAll(Collection<?> c);

    boolean addAll(Collection<? extends E> c);

    boolean addAll(int index, Collection<? extends E> c);

    boolean removeAll(Collection<?> c);

    boolean retainAll(Collection<?> c);

    void clear();

    boolean equals(Object o);

    int hashCode();

    E get(int index);

    E set(int index, E element);

    void add(int index, E element);

    E remove(int index);

    int indexOf(Object o);

    int lastIndexOf(Object o);

    ListIterator<E> listIterator();

    ListIterator<E> listIterator(int index);

    List<E> subList(int fromIndex, int toIndex);
}
```
我们可以看到，在List接口中采用泛型化定义之后，<E>中的E表示类型形参，可以接收具体的类型实参，并且此接口定义中，凡是出现E的地方均表示相同的接受自外部的类型实参。

自然的，ArrayList作为List接口的实现类，其定义形式是：

``` java
public class ArrayList<E> extends AbstractList<E> 
        implements List<E>, RandomAccess, Cloneable, java.io.Serializable {
    
    public boolean add(E e) {
        ensureCapacityInternal(size + 1);  // Increments modCount!!
        elementData[size++] = e;
        return true;
    }
    
    public E get(int index) {
        rangeCheck(index);
        checkForComodification();
        return ArrayList.this.elementData(offset + index);
    }
    
    //...省略掉其他具体的定义过程

}
```
由此，我们从源代码角度明白了为什么//1注释处加入Integer类型对象编译错误，且//2注释处get()到的类型直接就是String类型了。


----------

## 三、自定义泛型接口、泛型类和泛型方法
从上面的内容中，大家已经明白了泛型的具体运作过程。也知道了接口、类和方法也都可以使用泛型去定义，以及相应的使用。是的，在具体使用时，可以分为泛型接口、泛型类和泛型方法。

自定义泛型接口、泛型类和泛型方法与上述Java源码中的List、ArrayList类似。如下，我们看一个最简单的泛型类和方法定义：
### 示例代码3：

``` java
public class GenericTest {

    public static void main(String[] args) {

        Box<String> name = new Box<String>("corn");
        System.out.println("name:" + name.getData());
    }

}

class Box<T> {

    private T data;

    public Box() {

    }

    public Box(T data) {
        this.data = data;
    }

    public T getData() {
        return data;
    }

}
```
在泛型接口、泛型类和泛型方法的定义过程中，我们常见的如T、E、K、V等形式的参数常用于表示泛型形参，由于接收来自外部使用时候传入的类型实参。那么对于不同传入的类型实参，生成的相应对象实例的类型是不是一样的呢？
### 示例代码4：

``` java
public class GenericTest {

    public static void main(String[] args) {

        Box<String> name = new Box<String>("yanbo");
        Box<Integer> age = new Box<Integer>(1);

        System.out.println("name class:" + name.getClass());      // name class:class com.ys.test.Box
        System.out.println("age class:" + age.getClass());        // age class:class com.ys.test.Box
        System.out.println(name.getClass() == age.getClass());    // true

        
    }

}
```
由此，我们发现，在使用泛型类时，虽然传入了不同的泛型实参，但并没有真正意义上生成不同的类型，传入不同泛型实参的泛型类在内存上只有一个，即还是原来的最基本的类型（本实例中为Box），当然，在逻辑上我们可以理解成多个不同的泛型类型。

究其原因，在于Java中的泛型这一概念提出的目的，导致其只是作用于代码编译阶段，在编译过程中，对于正确检验泛型结果后，会将泛型的相关信息擦出，也就是说，成功编译过后的class文件中是不包含任何泛型信息的。泛型信息不会进入到运行时阶段。

对此总结成一句话：泛型类型在逻辑上看以看成是多个不同的类型，实际上都是相同的基本类型。


----------


## 四、类型通配符

接着上面的结论，我们知道，Box<Number>和Box<Integer>实际上都是Box类型，现在需要继续探讨一个问题，那么在逻辑上，类似于Box<Number>和Box<Integer>是否可以看成具有父子关系的泛型类型呢？

为了弄清这个问题，我们继续看下下面这个例子:
### 示例代码5：

``` java
public class GenericTest {

    public static void main(String[] args) {

        Box<Number> name = new Box<Number>(1);
        Box<Integer> age = new Box<Integer>(2);

        getData(name);
        
        //The method getData(Box<Number>) in the type GenericTest is 
        //not applicable for the arguments (Box<Integer>)
        getData(age);   // 1注释

    }
    
    public static void getData(Box<Number> data){
        System.out.println("data :" + data.getData());
    }

}
```

控制台打印结果

> Exception in thread "main" java.lang.Error: Unresolved compilation problem: 
	The method getData(Box<Number>) in the type GenericTest is not applicable for the arguments (Box<Integer>)	at com.ys.test.GenericTest.main(GenericTest.java:72)

我们发现，在代码//1处出现了错误提示信息：The method getData(Box<Number>) in the t ype GenericTest is not applicable for the arguments (Box<Integer>)。显然，通过提示信息，我们知道Box<Number>在逻辑上不能视为Box<Integer>的父类。那么，原因何在呢？
### 示例代码6：
``` java
public class GenericTest {

    public static void main(String[] args) {

        Box<Integer> a = new Box<Integer>(712);
        Box<Number> b = a;  // 1注释
        Box<Float> f = new Box<Float>(3.14f);
        b.setData(f);        // 2注释

    }

    public static void getData(Box<Number> data) {
        System.out.println("data :" + data.getData());
    }

}

class Box<T> {

    private T data;

    public Box() {

    }

    public Box(T data) {
        setData(data);
    }

    public T getData() {
        return data;
    }

    public void setData(T data) {
        this.data = data;
    }

}
```

控制台打印结果

> Exception in thread "main" java.lang.Error: Unresolved compilation problems: 
	Type mismatch: cannot convert from Box<Integer> to Box<Number>
	The method setData(Number) in the type Box<Number> is not applicable for the arguments (Box<Float>)	at com.ys.test.GenericTest.main(GenericTest.java:84)


这个例子中，显然//1注释和//2注释处肯定会出现错误提示的。在此我们可以使用反证法来进行说明。

假设Box<Number>在逻辑上可以视为Box<Integer>的父类，那么//1注释和//2注释处将不会有错误提示了，那么问题就出来了，通过getData()方法取出数据时到底是什么类型呢？Integer? Float? 还是Number？且由于在编程过程中的顺序不可控性，导致在必要的时候必须要进行类型判断，且进行强制类型转换。显然，这与泛型的理念矛盾，因此，在逻辑上Box<Number>不能视为Box<Integer>的父类。

好，那我们回过头来继续看“类型通配符”中的第一个例子，我们知道其具体的错误提示的深层次原因了。那么如何解决呢？总部能再定义一个新的函数吧。这和Java中的多态理念显然是违背的，因此，我们需要一个在逻辑上可以用来表示同时是Box<Integer>和Box<Number>的父类的一个引用类型，由此，类型通配符应运而生。

类型通配符一般是使用 ? 代替具体的类型实参。注意了，此处是类型实参，而不是类型形参！且Box<?>在逻辑上是Box<Integer>、Box<Number>...等所有Box<具体类型实参>的父类。由此，我们依然可以定义泛型方法，来完成此类需求。
### 示例代码7：
``` java
public class GenericTest {

    public static void main(String[] args) {

        Box<String> name = new Box<String>("yanbo");
        Box<Integer> age = new Box<Integer>(1);
        Box<Number> number = new Box<Number>(2);

        getData(name);
        getData(age);
        getData(number);
    }

    public static void getData(Box<?> data) {
        System.out.println("data :" + data.getData());
    }

}
```

控制台打印结果

> data :yanbo
data :1
data :2

有时候，我们还可能听到类型通配符上限和类型通配符下限。具体有是怎么样的呢？

在上面的例子中，如果需要定义一个功能类似于getData()的方法，但对类型实参又有进一步的限制：只能是Number类及其子类。此时，需要用到类型通配符上限。
### 示例代码8：
``` java
public class GenericTest {

    public static void main(String[] args) {

        Box<String> name = new Box<String>("yanbo");
        Box<Integer> age = new Box<Integer>(712);
        Box<Number> number = new Box<Number>(314);

        getData(name);
        getData(age);
        getData(number);
        
        getUpperNumberData(name); // 1注释
        getUpperNumberData(age);    // 2注释
        getUpperNumberData(number); // 3注释
    }

    public static void getData(Box<?> data) {
        System.out.println("data :" + data.getData());
    }
    
    public static void getUpperNumberData(Box<? extends Number> data){
        System.out.println("data :" + data.getData());
    }

}
```

控制台打印结果

> Exception in thread "main" java.lang.Error: Unresolved compilation problem: 
	The method getUpperNumberData(Box<? extends Number>) in the type GenericTest is not applicable for the arguments (Box<String>)	at com.ys.test.GenericTest.main(GenericTest.java:121)

此时，显然，在代码//1注释处调用将出现错误提示，而//2注释 //3注释处调用正常。

类型通配符上限通过形如Box<? extends Number>形式定义，相对应的，类型通配符下限为Box<? super Number>形式，其含义与类型通配符上限正好相反



