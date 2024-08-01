#### java集合体系框架

![java集合体系框架](https://s2.loli.net/2024/07/16/Ia2vAMZzJGfYmQk.png)

java集合类主要由两个根接口Collection和Map派生出来的。

##### Collection派生出了三个子接口：

![img](https://s2.loli.net/2024/07/16/teZPyEprU5HsVS1.png)

1)List

List代表了有序可重复集合，可直接根据元素的索引来访问

2)Set

Set代表无序不可重复集合，只能根据元素本身来访问

3)Queue

Queue是队列集合

Map接口派生：

Map代表的是存储key-value对的集合，可根据元素的key来访问value。

![img](https://s2.loli.net/2024/07/16/CBAJiEnRtprjfe3.png)

因此java集合大致也课分为：List、Set、Queue、Map四种接口体系。

#### Java集合List

List代表了有序可重复集合，可直接根据元素的索引来访问。

List接口常用的实现类有：ArrayList、LinkedList、Vector。

List集合的特点

- 集合中的元素允许重复
- 集合中的元素是有顺序的，各元素插入的顺序就是各元素的顺序
- 集合中的元素可以通过索引来访问或者设置

#### ArrayList

ArrayList是一个动态数组，也是我们最常用的集合，是List类的典型实现。

它允许任何符合规则的元素插入甚至包括null，每一个ArrayList都有一个初始容量(10)，该容量代表了数组的大小。

随着容器中的元素不断增加，容器的大小也会随着增加，在每次向容器中增加元素的同时都会进行容量检查，当快要溢出时，就会进行扩容操作。

所以如果我们明确所插入元素的多少，最好指定一个初始容量值，避免过多的进行扩容操作而浪费时间、效率。

ArraryList擅长于随机访问，同时ArrayList时非同步的。

#### Vector

与ArrayList相似，但是Vector是同步的，它的操作与ArrayList几乎一样。

#### LinkedList

LinkedList是采用双向循环链表实现的，LinkedList是List接口的另一个实现，除了可以根据索引访问集合元素外，LinkedList还实现了Deque接口，可以当作双端队列来使用，也就是说，既可以当作”栈“使用，又可以当做队列使用。

#### JavaList总结

##### 1)ArrayList

优点：顶嗯数据结构是素组，查询快，增删慢。

缺点：线程不安全，效率高

##### 2)Vector

优点：顶嗯数据结构是素组，查询快，增删慢。

缺点：线程安全，效率低

##### 3)LinkedList

优点：底层数据结构是链表，查询慢，增删快。

缺点，线程不安全，效率高

#### Java集合Set

Set扩展Collection接口，无序集合，不允许存放重复的元素。

Set接口常用的实现类有：HashSet、LinkedHashSet、TreeSet

![img](https://s2.loli.net/2024/07/16/6uSdPzatLbvk2Jr.png)

#### HashSet

HashSet是Set集合最常用实现类，是其经典实现。

HashSet底层数据结构采用哈希表实现，元素无序且唯一，线程不安全，效率高，可以存储null元素，元素的唯一性是靠所存储元素类型是否重写hashCode()和equals()方法来保证的，如果没有重写这两个方法，则无法保证元素的唯一性。

#### LinkedHashSet

底层数据结构采用链表和哈希表共同实现，链表保证了元素的顺序与存储顺序一致，哈希表保证了元素的唯一性。

#### TreeSet

底层数据结构采用二叉树来实现，元素唯一且已经排好序,唯一性同样需要重写hashCode和equals()方法，二叉树结构保证了元素的有序性。

#### JavaSet总结

##### 1)HashSet

- 底层其实是包装了一个HashMap实现的
- 底层数据结构是数组+链表 + 红黑树
- 具有比较好的读取和查找性能， 可以有null 值
- 通过equals和HashCode来判断两个元素是否相等
- 非线程安全

##### 2)LinkedHashSet

- 继承HashSet，本质是LinkedHashMap实现
- 底层数据结构由哈希表(是一个元素为链表的数组)和双向链表组成。
- 有序的，根据HashCode的值来决定元素的存储位置，同时使用一个链表来维护元素的插入顺序
- 非线程安全，可以有null 值

##### 3)TreeSet

- 是一种排序的Set集合，实现了SortedSet接口，底层是用TreeMap实现的，本质上是一个红黑树原理
- 排序分两种：自然排序（存储元素实现Comparable接口）和定制排序（创建TreeSet时，传递一个自己实现的Comparator对象）
- 正常情况下不能有null值，可以重写Comparable接口 局可以有null值了。

#### Java集合Queue

![img](https://s2.loli.net/2024/07/16/7zKWScjN49vuasB.png)

队列是数据结构中比较重要的一种类型，它支持 FIFO，尾部添加、头部删除（先进队列的元素先出队列），跟我们生活中的排队类似。

##### PriorityQueue

PriorityQueue保存队列元素的顺序并不是按照加入的顺序，而是按照队列元素的大小进行排序的。
PriorityQueue不允许插入null元素。

##### Deque

Deque接口是Queue接口的子接口，它代表一个双端队列,当程序中需要使用“栈”这种数据结构时，推荐使用ArrayDeque。

#### Java集合Map

Map用于保存具有映射关系的数据，Map里保存着两组数据：key和value，它们都可以使任何引用类型的数据，但key不能重复。

![img](https://s2.loli.net/2024/07/16/QZWyXBTuJhos7ln.png)

##### HashMap

Map接口基于哈希表的实现，是使用频率最高的用于键值对处理的数据类型。

它根据键的hashCode值存储数据，大多数情况下可以直接定位到它的值，特点是访问速度快，遍历顺序不确定，线程不安全，最多允许一个key为null，允许多个value为null。

可以用 Collections的synchronizedMap方法使HashMap具有线程安全的能力，或者使用ConcurrentHashMap类。

##### Hashtable

Hashtable和HashMap从存储结构和实现来讲有很多相似之处，不同的是它承自Dictionary类，而且是线程安全的，另外Hashtable不允许key和value为null，并发性不如ConcurrentHashMap。

Hashtable不建议在新代码中使用，不需要线程安全的场合可以用HashMap替换，需要线程安全的场合可以用ConcurrentHashMap替换。

##### LinkedHashMap

LinkedHashMap继承了HashMap，是Map接口的哈希表和链接列表实现，它维护着一个双重链接列表，此链接列表定义了迭代顺序，该迭代顺序可以是插入顺序或者是访问顺序。

#### TreeMap

TreeMap实现SortMap接口，能够把它保存的记录根据键排序，默认是按键值的升序排序（自然顺序），也可以指定排序的比较器，当用Iterator遍历TreeMap时，得到的记录是排过序的。

##### Map总结

![img](https://s2.loli.net/2024/07/16/Jc4U7Ltf3gWmP2C.png)