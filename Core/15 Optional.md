# 15 Optional

到目前为止，臭名昭著的空指针异常是导致Java应用程序失败的最常见原因。 以前，为了解决空指针异常，Google公司著名的Guava项目引入了`Optional`类， Guava通过使用检查空值的方式来防止代码污染，它鼓励程序员写更干净的代 码。受到Google Guava的启发，`Optional`类已经成为Java 8类库的一部分。

*   `java.util.Optional<T>` 是一个容器类，它可以保存类型T的值，代表这个值存在。或者仅仅保存null，表示这个值不存在。原来用 null 表示一个值不存在，现在 `Optional` 可以更好的表达这个概念。并且可以避免空指针异常。
*   `Optional`类的Javadoc描述如下:这是一个可以为`null`的容器对象。如果值存在 则`isPresent()`方法会返回`true`，调用`get()`方法会返回该对象。

**创建`Optional`类对象的方法**

*   `Optional.of(T t)`

    创建一个 `Optional` 实例，`t`必须**非空**; 

*   `Optional.ofNullable(T t)`

    `t`可以为`null`

*   `Optional.empty()`

    创建一个空的 `Optional` 实例

**判断`Optional`容器中是否包含对象**

*   `boolean isPresent()`

    判断是否包含对象

*   `void ifPresent(Consumer<? super T> consumer)`

    如果有值，就执行Consumer接口的实现代码，并且该值会作为参数传给它。

**获取`Optional`容器的对象**

*   `T get()`

    如果调用对象包含值，返回该值，否则抛异常

*   `T orElse(T other)`

    如果有值则将其返回，否则返回指定的other对象。

*   `T orElseGet(Supplier<? extends T> other)`

    如果有值则将其返回，否则返回由`Supplier`接口实现提供的对象。

*   `T orElseThrow(Supplier<? extends X> exceptionSupplier)`

    如果有值则将其返回，否则抛出由`Supplier`接口实现提供的异常。



```java
public class Girl {

  private String name;
  
  public Girl() {}

  public Girl(String name) {
    this.name = name;
  }

  public String getName() {
    return name;
  }

  public void setName(String name) {
    this.name = name;
  }

  @Override
  public String toString() {
    return "Girl{" +
      "name='" + name + '\'' +
      '}';
  }
}
// -------------------------------
public class Boy {

  private Girl girl;

  public Boy() {}

  public Boy(Girl girl) {
    this.girl = girl;
  }

  public Girl getGirl() {
    return girl;
  }

  public void setGirl(Girl girl) {
    this.girl = girl;
  }

  @Override
  public String toString() {
    return "Boy{" +
      "girl=" + girl +
      '}';
  }
}
```

```java
@Test
public void test1(){
  Girl girl = new Girl();
  // girl = null;
  
  //of(T t):保证t是非空的，对于 null 执行会报 NPE 
  Optional<Girl> optionalGirl = Optional.of(girl);
  
  //ofNullable(T t)：t可以为null，执行输出为Optional.empty
  Optional<Girl> optionalGirl = Optional.ofNullable(girl);
  System.out.println(optionalGirl);
  
  //orElse(T t1):如果当前的Optional内部封装的t是非空的，则返回内部的t；如果内部的t是空的，则返回orElse()方法中的参数t1.
  Girl girl1 = optionalGirl.orElse(new Girl("赵丽颖"));
  System.out.println(girl1);
}

// --------------没有 Optional 时--------------------
@Test
public void test3(){
  Boy boy = new Boy();
  boy = null;
  String girlName = getGirlName(boy);
  System.out.println(girlName);

}

//优化之前的getGirlName():
public String getGirlName(Boy boy){
  return boy.getGirl().getName();
}


//优化以后的getGirlName():
public String getGirlName1(Boy boy){
  if(boy != null){
    Girl girl = boy.getGirl();
    if(girl != null){
      return girl.getName();
    }
  }
  return null;
}

// -----------有 Optional 时------------

//使用Optional类的getGirlName():
public String getGirlName2(Boy boy){

  Optional<Boy> boyOptional = Optional.ofNullable(boy);
  //此时的boy1一定非空
  Boy boy1 = boyOptional.orElse(new Boy(new Girl("迪丽热巴")));

  Girl girl = boy1.getGirl();

  // 实际中不会多写如下一层
  Optional<Girl> girlOptional = Optional.ofNullable(girl);
  //girl1一定非空
  Girl girl1 = girlOptional.orElse(new Girl("古力娜扎"));

  return girl1.getName();
}

@Test
public void test5(){
  Boy boy = null;
  boy = new Boy();
  boy = new Boy(new Girl("苍老师"));
  String girlName = getGirlName2(boy);
  System.out.println(girlName);

}
```

