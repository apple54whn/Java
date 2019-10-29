# 5 Generic

## 5.1 为什么要有泛型

*   泛型：类似标签。例如：中药店，每个抽屉外面贴着标签；垃圾分类！

*   泛型的**设计背景**：

    集合容器类在设计阶段、声明阶段不能确定这个容器到底实际存的是什么类型的对象，所以在JDK1.5之前只能把元素类型设计为`Object`，JDK1.5之后使用泛型来解决。因为这个时候除了元素的类型不确定，其他的部分是确定的，例如关于这个元素如何保存，如何管理等是确定的，因此此时把**元素的类型设计成一个参数**，这个**类型参数叫做泛型**。 `Collection<E>`，`List<E>`，`ArrayList<E>` 这个`<E>`就是类型参数，即泛型。

*   泛型的概念：所谓泛型，就是允许在**定义类、接口时通过一个标识表示类中某个属性的类型或者是某个方法的返回值及参数类型**。这个类型参数将在**使用时**（例如， 继承或实现这个接口，用这个类型声明变量、创建对象时）**确定**（即传入实际的类型参数，也称为类型实参）。

    从JDK1.5以后，Java引入了“参数化类型(Parameterized type)”的概念， 允许我们在创建集合时再指定集合元素的类型，正如：`List<String>`，这表明该`List`只能保存字符串类型的对象。JDK1.5改写了集合框架中的全部接口和类，为这些接口、类增加了泛型支持， 从而可以在声明集合变量、创建集合对象时传入类型实参。

*   那么为什么要有泛型？泛型的好处？

    *   解决元素**存储**的**类型不安全**问题，好比商品、药品标签，不会弄错。在设计API时可以指定类或方法支持泛型，这样使用API的时候也变得更为简洁，并得到了**编译时期的语法检查**。
    *   解决**获取**数据元素时，需要**类型强制转换**的问题，有可能强转引发**运行时**`ClassCastException`。不用每回拿商品、药品都要辨别。

>   TIPS：泛型是数据类型的一部分，我们将类名与泛型合并一起看做数据类型。



## 5.2 集合中使用泛型

略👻



## 5.3 自定义泛型

注意：

*   泛型类可能有多个参数，此时应将多个参数一起放在尖括号内。比如`<E1,E2,E3>`

*   `E`或其他只能是类，不能用基本数据类型填充，但可以使用包装类填充。

*   构造器不用带`E`

*   **泛型不同的引用不能相互赋值**

*   在类/接口上声明的泛型，在本类或本接口中即代表某种类型，可以作为**非静态属性的类型、非静态方法的参数类型、非静态方法的返回值类型**。但**在静态方法中不能使用（使用！）类的泛型**。因为泛型是在创建对象时确定，晚于 static

*   异常类不能是泛型的；不能在try-catch中使用泛型定义 

*   不能使用`new E[]`。但是可以`E[] elements = (E[])new Object[capacity]`，参考`ArrayList`源码中声明`Object[] elementData`，而非泛型参数类型数组

*   **父类有泛型，子类可以选择保留泛型也可以选择指定泛型类型**

    *   子类不保留父类的泛型：按需实现
        *   没有类型擦除
        *   具体类型
    *   子类保留父类的泛型：泛型子类
        *   全部保留
        *   部分保留

    结论：子类必须是“富二代”，**子类除了指定或保留父类的泛型，还可以增加自己的泛型**

    ```java
    class Father<T1, T2> {}
    
    // 子类不保留父类的泛型
    // 1)没有类型 擦除
    class Son1 extends Father {/*等价于class Son extends Father<Object,Object>{}*/}
    // 2)具体类型
    class Son2 extends Father<Integer, String> { }
    
    // 子类保留父类的泛型
    // 1)全部保留
    class Son3<T1, T2> extends Father<T1, T2> { }
    // 2)部分保留
    class Son4<T2> extends Father<Integer, T2> { }
    ```

    ```java
    class Father<T1, T2> {}
    // 子类不保留父类的泛型
    // 1)没有类型 擦除
    class Son<A, B> extends Father{/*等价于class Son extends Father<Object,Object>{ }*/}
    // 2)具体类型
    class Son2<A, B> extends Father<Integer, String> { }
    
    // 子类保留父类的泛型
    // 1)全部保留
    class Son3<T1, T2, A, B> extends Father<T1, T2> { }
    // 2)部分保留
    class Son4<T2, A, B> extends Father<Integer, T2> { }
    ```

    

### 5.3.1 自定义泛型类

*   在**创建对象时**就确定泛型的类型。

```java
public class MyGenericClass<E> {//没有E类型，在这里代表 未知的一种数据类型 未来传递什么就是什么类型
    private E e;

    public MyGenericClass() {
    }

    public MyGenericClass(E e) {
        this.e = e;
    }

    public void setE(E e) {
        this.e = e;
    }
  
    public E getE() {
        return e;
    }
}
```

```java
MyGenericClass<String> my = new MyGenericClass<>(); //创建对象时就确定泛型的类型
```





### 5.3.2 自定义泛型接口

*   **实现接口时**确定泛型的类型；
*   始终不确定泛型，直到**创建对象时**确定泛型的类型

```java
public interface MyGenericInterface<E>{
	
  public abstract void add(E e);
	
	public abstract E getE();  
}
```

```java
public class MyImp1 implements MyGenericInterface<String> { //实现接口时确定泛型的类型
    @Override
    public void add(String e) { /*省略...*/ }

    @Override
    public String getE() { /*省略...*/ }
}
```

```java
public class MyImp2<E> implements MyGenericInterface<E> { //始终不确定泛型
    @Override
    public void add(E e) { /*省略...*/ }
    
	@Override
    public E getE() { /*省略...*/ }
}
MyImp2<String>  my = new MyImp2<>();  //直到创建对象时确定泛型的类型
my.add("aa"); 
```



### 5.3.3 自定义泛型方法

*   方法也可以被泛型化，**不管此时定义在其中的类是不是泛型类**。在泛型方法中可以定义泛型参数，此时参数的类型就是传入数据的类型。

*   **调用方法传递数据时**确定泛型的类型
*   **泛型方法可以声明为静态的（不同于使用类的泛型）**。原因：泛型参数是在调用方法时确定的。并非在实例化类时确定。

```java
修饰符 <代表泛型的变量> 返回值类型 方法名(参数){  } //格式
public class MyGenericMethod {	  
    // 权限修饰符后的<E>可以写上，让编译器知道这是泛型，不是具体类。也可以不写，在 IDEA2019中没报错
    public <E>  List<E> show(E[] e) {
        System.out.println(e.getClass());
        return null;
    }

    public static <E> E show2(E e) {
        return e;
    }
}
```

```java
MyGenericMethod mm = new MyGenericMethod();
mm.show("aaa"); //调用方法传递数据时确定泛型的类型
mm.show(1);
```



## 5.4 泛型在继承方面的体现

*   如果 `B` 是 `A` 的一个子类型（子类或者子接口），而 `G` 是具有泛型声明的类或接口，`G<B>`并不是`G<A>`的子类型

    类`A`是类`B`的父类，`A<G>`是`B<G>`的父类

    ```java
    List<Object> list1 = null;
    List<String> list2 = null;
    list1 = list2; // 编译失败。add 时数据不安全
    
    Object a = null;
    String b = null;
    a = b;// 成功
    ```

    

## 5.5 泛型通配符

-   当**使用泛型类或者接口**时，**传递的数据中**，**泛型类型不确定**，可以通过通配符`?`表示。但是一旦使用泛型的通配符后，只能使用`Object`类中的共性方法。集合中元素自身方法无法使用，此时**只能读取数据，不能添加数据（除了 null ）**。

-   类`A`是类`B`的父类，`G<A>`和`G<B>`是没有关系的，二者共同的父类

-   使用方式：不能创建对象时使用，只能**作为方法的参数使用**

    ```java
    public static void main(String[] args) {
      Collection<Intger> list1 = new ArrayList<Integer>();
      getElement(list1);
      Collection<String> list2 = new ArrayList<String>();
      getElement(list2);
    }
    public static void getElement(Collection<?> coll){}//？代表可以接收任意类型
    ```

-   注意

    ```java
    //注意点1:编译错误:不能用在泛型方法声明上，返回值类型前面<>不能使用? 
    public static <?> void test(ArrayList<?> list){
    }
    ```

    ```java
    //注意点2:编译错误:不能用在泛型类的声明上 
    class GenericTypeClass<?>{
    }
    ```

    ```java
    //注意点3:编译错误:不能用在创建对象上，右边属于创建集合对象 
    ArrayList<?> list2 = new ArrayList<?>();
    ```

    

-   **通配符高级使用——受限泛型**

    -   之前设置泛型的时候，实际上是可以任意设置的，只要是类就可以设置。但是在JAVA的泛型中可以指定一个泛型的**上限**和**下限**。

    -   **泛型的上限**：

        -   **格式**： `类型名称 <? extends 类 > 对象名称`
        -   **意义**：只能接收该类型及其子类

        **泛型的下限**：

        -   **格式**： `类型名称 <? super 类 > 对象名称`
        -   **意义**：只能接收该类型及其父类型

        ```java
        //现已知Object类，String 类，Number类，Integer类，其中Number是Integer的父类
        public static void main(String[] args) {
            Collection<Integer> list1 = new ArrayList<Integer>();
            Collection<String> list2 = new ArrayList<String>();
            Collection<Number> list3 = new ArrayList<Number>();
            Collection<Object> list4 = new ArrayList<Object>();
        
            getElement1(list1);
            getElement1(list2);//报错
            getElement1(list3);
            getElement1(list4);//报错
        
            getElement2(list1);//报错
            getElement2(list2);//报错
            getElement2(list3);
            getElement2(list4);
        
        }
        public static void getElement1(Collection<? extends Number> coll){}
        public static void getElement2(Collection<? super Number> coll){}
        ```







## 用处

#### Mybatis-Plus 的通用 Mapper

```java
public interface BaseMapper<T> extends Mapper<T> {

    /**
     * 插入一条记录
     *
     * @param entity 实体对象
     */
    int insert(T entity);
  
  	//......
}
```

```java
public class UserDao implements BaseMapper<User> {
}
```

