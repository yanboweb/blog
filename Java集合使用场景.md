# List概括

![List框架图]( https://raw.githubusercontent.com/yanboweb/blog-images-store/master/20170715/List%E6%A1%86%E6%9E%B6%E5%9B%BE.jpg)

1. List 是一个接口，它继承于Collection的接口。它代表着有序的队列。
2.  AbstractList 是一个抽象类，它继承于AbstractCollection。AbstractList实现List接口中除size()、get(int location)之外的函数。
3. AbstractSequentialList 是一个抽象类，它继承于AbstractList。AbstractSequentialList 实现了“链表中，根据index索引值操作链表的全部函数”。
4. **ArrayList**, LinkedList, Vector, Stack是List的4个实现类。
  
  

> ArrayList 是一个数组队列，相当于动态数组。它由数组实现，随机访问效率高，随机插入、随机删除效率低。
LinkedList 是一个双向链表。它也可以被当作堆栈、队列或双端队列进行操作。LinkedList随机访问效率低，但随机插入、随机删除效率低。
Vector 是矢量队列，和ArrayList一样，它也是一个动态数组，由数组实现。但是ArrayList是非线程安全的，而Vector是线程安全的。
Stack 是栈，它继承于Vector。它的特性是：先进后出(FILO, First In Last Out)。

  # List使用场景

**1.  对于需要快速插入，删除元素，应该使用LinkedList。**
**2.  对于需要快速随机访问元素，应该使用ArrayList。**
3.  对于“单线程环境” 或者 “多线程环境，但List仅仅只会被单个线程操作”，此时应该使用非同步的类(如ArrayList)。
4. 对于“多线程环境，且List可能同时被多个线程操作”，此时，应该使用同步的类(如Vector)。

# Map概括
![Map框架图]( https://raw.githubusercontent.com/yanboweb/blog-images-store/master/20170715/Map%E6%9E%B6%E6%9E%84%E5%9B%BE.jpg)

**1. Map 是映射接口，Map中存储的内容是键值对(key-value)。**
2. AbstractMap 是继承于Map的抽象类，它实现了Map中的大部分API。其它Map的实现类可以通过继承AbstractMap来减少重复编码。
3. SortedMap 是继承于Map的接口。SortedMap中的内容是排序的键值对，排序的方法是通过比较器(Comparator)。
4. NavigableMap 是继承于SortedMap的接口。相比于SortedMap，NavigableMap有一系列的导航方法；如"获取大于/等于某对象的键值对"、“获取小于/等于某对象的键值对”等等。
5. TreeMap 继承于AbstractMap，且实现了NavigableMap接口；因此，TreeMap中的内容是“有序的键值对”！
**6. HashMap 继承于AbstractMap，但没实现NavigableMap接口；因此，HashMap的内容是“键值对，但不保证次序”！**
7. Hashtable 虽然不是继承于AbstractMap，但它继承于Dictionary(Dictionary也是键值对的接口)，而且也实现Map接口；因此，Hashtable的内容也是“键值对，也不保证次序”。但和HashMap相比，Hashtable是线程安全的，而且它支持通过Enumeration去遍历。
8. WeakHashMap 继承于AbstractMap。它和HashMap的键类型不同，WeakHashMap的键是“弱键”。
	



