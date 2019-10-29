# 10 Stream API

在Java 8中得益于Lambda所带来的函数式编程，引入一个全新的Stream概念，用于**解决**已有**集合类库**既有弊端。

Java 8的Lambda让我们可以更加专注于做什么（What），而不是怎么做（How），这点此前已经结合内部类进行 了对比说明。结合集合中遍历操作，可以发现循环遍历的弊端：

* for循环的语法就是“怎么做” 
* for循环的循环体才是“做什么”

> 如果希望对集合中的元素进行筛选过滤：
>
> * 将集合A根据条件一过滤为子集B； 
> * 然后再根据条件二过滤为子集C。 
>
> 使用线性循环就意味着只能遍历一次，那么需多次循环来解决；并且循环是做事情的方式，而不是目的。

```java
//筛选以张开始，长度为3，遍历打印
list.stream()
    .filter(s->s.startsWith("张"))
    .filter(s->s.length()==3)
    .forEach(s -> System.out.println(s));
```



## 10.1 流式思想概述

> 注意：请暂时忘记对传统IO流的固有印象！

整体来看，流式思想类似于工厂车间的“**生产流水线**”。当需要**对多个元素进行操作（特别是多步操作）**的时候，考虑到性能及便利性，我们应该**首先拼好一个“模型”步骤方案**，然后再按照方案去**执行**它。

![](images\拼接流式模型.PNG)

这张图中展示了过滤、映射、跳过、计数等多步操作，这是一种**集合元素的处理方案**，而方案就是一种“**函数模 型**”。图中的**每一个方框都是一个“流”**，调用指定的方法，可以**从一个流模型转换为另一个流模型**。而右侧的数字 3是终结果。

==这里的 filter 、 map 、 skip 都是在**对函数模型进行操作**，**集合元素并没有真正被处理**。**只有当终结方法 count 执行**的时候，**整个模型才会按照指定策略执行操作**。而这得益于**Lambda的延迟执行特性**。==

> 备注：**“Stream流”其实是一个集合元素的函数模型**，它并不是集合，也不是数据结构，**其本身并不存储任何 元素（或其地址值）**。

**Stream（流）是一个来自数据源的元素队列**

* **元素**是**特定类型的对象**，形成一个队列。 Java中的Stream并不会存储元素，而是按需计算。 
* **数据源**：流的来源。 可以是集合，数组 等。 

和以前的Collection操作不同， Stream操作还有两个基础的特征： 

* **Pipelining**：中间操作都会**返回流对象本身**。 这样多个操作可以**串联**成一个管道， 如同流式风格（ﬂuent style）。 这样做可以对操作进行优化， 比如延迟执行(laziness)和短路( short-circuiting)。 
* **内部迭代**：以前对集合遍历都是通过Iterator或者增强for的方式，显式的在集合外部进行迭代， 这叫做外部迭 代。 **Stream提供了内部迭代的方式，流可以直接调用遍历方法**。

当使用一个流的时候，通常包括三个基本步骤：**获取一个数据源（source）**→ **数据转换**→**执行操作**获取想要的结 果，每次转换原有 Stream 对象不改变，返回一个新的 Stream 对象（可以有多次转换），这就允许对其操作可以 像链条一样排列，变成一个管道。 



## 10.2 获取Stream

> `java.util.stream.Stream<T>` 是Java 8新加入的常用的流接口。（这并不是一个函数式接口）。

**获取流的方式**

* `default Stream<E> stream() `：

    返回以此 **`Collection`** 为数据源的流，`Map`可以转为`Set`来使用

* `static <T> Stream<T> of(T... values)`：

    `Stream`接口的静态方法（可变参数），也可以获取**数组**的流

* 获取并发流：`collection.parallelStream()`、`stream.parallel()`

## 10.3 常用方法

> Stream属于管道流，**只能被使用一次**，数据从上一个Stream传到下一个Stream上，上一个Stream已经关闭

**延迟方法**：**返回值类型仍然是Stream接口自身**类型的方法，因此支持链式调用。（除了终结方法外，其余方法均为延迟方法。） 

* `Stream<T> filter(Predicate<? super T> predicate)`：

    返回==**与给定谓词匹配**==的此流的元素组成的流，<a href="#Predicate">接口</a>

* `<R> Stream<R> map(Function<? super T,? extends R> mapper)`：

    将流的元素==**转换(映射)**==到另一流，<a href="#Function">接口</a>

* `Stream<T> limit(long maxSize)`：

    对流进行==**截取前maxSize个**==

* `Stream<T> skip(long n)`：

    截取==**跳过前n个**==元素的流，若流当前长度小于等于n会得到长度为0的空流

* `static <T> Stream<T> concat(Stream<? extends T> a, Stream<? extends T> b)`：

    ==**合并流**==，静态方法

* `<R,A> R collect(Collector<? super T,A,R> collector)`：

    规约操作；转为List，如`Collectors.toList()`

**终结方法**：**返回值类型不再是 Stream 接口自身**类型的方法，因此不再支持类似 StringBuilder 那样的链式调用。本小节中，终结方法包括 `count` 和 `forEach` 方法。[其他查看API](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/stream/Stream.html)。

* `void forEach(Consumer<? super T> action)`：

    对此流的每个元素==**逐一处理**==。<a href="#Consumer">接口</a>

* `long count()`：

    返回此流中元素的==**数量**==

* `Object[] toArray()`：

    ==**转为数组**==，返回包含此流的元素的数组

* `<A> A[] toArray(IntFunction<A[]> generator)`：

    通过==**方法引用指定创建数组**==类型，如`String[]::new`

```java
public static void main(String[] args) {
    /**
         * 1. 第一个队伍只要名字为3个字的成员姓名；存储到一个新集合中。
         * 2. 第一个队伍筛选之后只要前3个人；存储到一个新集合中。
         * 3. 第二个队伍只要姓张的成员姓名；存储到一个新集合中。
         * 4. 第二个队伍筛选之后不要前2个人；存储到一个新集合中。
         * 5. 将两个队伍合并为一个队伍；存储到一个新集合中。
         * 6. 根据姓名创建 Person 对象；存储到一个新集合中。
         * 7. 打印整个队伍的Person对象信息。
         */
    ArrayList<String> one = new ArrayList<>();
    one.add("迪丽热巴");
    one.add("宋远桥");
    one.add("苏星河");
    one.add("石破天");
    one.add("石中玉");
    one.add("老子");
    one.add("庄子");
    one.add("洪七公");

    ArrayList<String> two = new ArrayList<>();
    two.add("古力娜扎");
    two.add("张无忌");
    two.add("赵丽颖");
    two.add("张三丰");
    two.add("尼古拉赵四");
    two.add("张天爱");
    two.add("张二狗");

    Stream<String> one2 = one.stream().filter(name -> name.length() == 3).limit(3);
    Stream<String> two2 = two.stream().filter(name -> name.startsWith("张")).skip(2);
    Stream.concat(one2, two2).map(name -> new Person(name)).forEach(name -> System.out.println(name));

}
```



