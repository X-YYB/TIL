# 复习规划
## 1.JAVA基础
### 1.1、JAVA基本数据类型
答：分四类，公8种。
1. 整型：byte(1),short(2),int（4），Long(8);
2. 浮点型：float(4),double(8);
3. 布尔型：boolean：true,false;
4. 字符型：char(2)
### 1.2、Java类和对象的概念
JAVA是面向对象的编程语言，类是对象的抽象表示，对象是类的实例。
#### 1.2.1、类与类之间的关系
关联、聚合、组合、依赖、继承、实现
### 1.3、Java面向对象的三个特征？
封装、继承、多态。
#### 1.3.1、封装
通过访问修饰符public、缺省、project、private来实现。可以限制性的访问类和对象的方法、属性。
#### 1.3.2、继承
子类可以继承父类所有的东西，但是子类看不到父类private的属性和方法。
#### 1.3.3、多态
将子类的引用赋值给一个父类对象，这样子类可以重写父类的方法，也可以扩展拥有自己的属性和方法。
#### 1.3.4、面向对象编程（OOP）
面向对象编程将现实世界中的概念和事务，以及相应的状态和行为，抽象成计算机语言中的类和对象，以及相应的属性和方法，这是一个抽象的过程。类和对象是面向对象编程的核心概念，它包含有封装、继承、多态三大特征，也有人把抽象作为它的第四大特征。由于面向对象的使用，使得程序的可扩展性大大增强，面向对象编程在大型项目中得到了广泛的使用。面向对象的思想更符合于我们人类对现实世界的认知，可以使我们设计出更加灵活，维护性更好的程序。
### 1.4、抽象方法、抽象类、接口
#### 1.4.1、抽象方法、抽象类
抽象方法、抽象类通过abstract关键字修饰。
含有抽象方法的类必须是抽象类，因为如果某个拥有抽象方法的类被其子类继承，并去掉这个父类的抽象方法，而抽象方法是没有方法体的，这时候应该输出什么呢？
答：编译器肯定会报错，所以这个时候就只能将这个父类也声名为抽象类，这样其子类就必须去重写它的抽象方法，这样就这个问题就解决了。当然如果这个子类不需要重写这个抽象方法，那也可以将自己也定义为抽象类即可。
一个类只能继承一个父类，即单继承。
#### 1.4.2、接口
通过interface关键字修饰。接口里面有常量和抽象方法，就是通过实现该接口的类去重写抽象方法，从而实现多态。
一个类可以实现多个接口。
万事万物皆对象，面向对象编程，面向接口编程。
JDK8以后，接口里面可以有静态方法（static）和默认方法（default）。  
### 1.5、封箱和拆箱

### 1.6、异常处理
#### 1.6.1、 JAVA处理异常的思路：
一是自己捕获处理，即加try(){}catch(){};
二是处理以后再向上抛出；
三是直接向上抛出(throw e)异常,对应的方法上也要加上throws e，甚至可以将异常抛给main方法，最终交由JVM去处理，当然不推荐这样做。
#### 1.6.2、JAVA程序的错误：
1. 代码错误，一般编译器就会帮我们指出。
2. 逻辑错误，需要测试，才能排查出来。
3. 程序运行期发生的错误。这种错误也分三种，一种是error，比如内存溢出等，这种不用出来，JVM会报错，直接终止程序；第二种是运行时异常（RuntimeException），也叫非受检查异常，也不用处理，JVM会处理，打出堆栈错误，终止程序等等；第三种是除前两种意外的异常（空指针等），也叫受检查异常或非运行时异常，需要我们去处理，必须捕获处理，或向上抛出。
4. 自定义异常
#### 1.6.3、therow和throws的区别？
throw用于在方法内部抛出异常，throws用于方法之上，两者一般是成对出现。
### 1.7、IO流
### 1.8、集合
JAVA中的集合又称容器，是对数据结构的一种实现，通过集合，我们可以完成对数据的增删改查，方便程序员管理数据。
JAVA中集合中的数据是存储在内存当中的，是一种临时的数据存储方式，当服务器重启或出现故障的情况下，集合中的数据就会消失。
#### 1.8.1、JAVA集合体系
![JAVA集合体系](vx_images/168244017220562.png =998x)
#### 1.8.2、Collection接口
1. List和Set集合的父接口，当然还有其它的实现类或子接口。
#### 1.8.3、List接口
Collection接口的子接口，存储有序，可重复的数据。
#### 1.8.4、ArrayList
数据结构：数组，相对于链表，查询快，增删慢。
常用方法：
```
 List<String> arrayList=new ArrayList<>();
        arrayList.add("尾");//尾加
        arrayList.add(1,"一");//插入
        arrayList.add(1,"二");//插入一个新值，原先在这个位置上的将往后移一位
        arrayList.set(1,"3");//覆盖掉原先在这个位置上的值
        arrayList.remove(0);
        arrayList.remove("一");
        //遍历方式：1、for循环、2、增强for、3、迭代器iterator
        Iterator<String> iterator = arrayList.iterator();
        while (iterator.hasNext()){
            System.out.println(iterator.next().toString());
        }
```
扩容机制：1.5N,生成一个新的数组，将原来的数组复制到新的数组中。删除同理。

`
//默认长度10
private static final int DEFAULT_CAPACITY = 10;
`

不足点：线程不安全，方法无synchronized修饰等。与之对应的Vector是线程安全的。
#### 1.8.5、LinedkList
数据结构：双向非循环链表，相对于数组，增删快，查询慢。
常用方法：
```
LinkedList<String> linkedList=new LinkedList<>();
        linkedList.add("一");
        linkedList.add("二");
        linkedList.addLast("tail");
        linkedList.addFirst("head");
        System.out.println(linkedList.toString());//[head, 一, 二, tail]
        linkedList.remove("一");
        System.out.println(linkedList.toString());//[head, 二, tail]
```
不足点：线程不安全，方法无synchronized修饰等。
#### 1.8.6、Set
Set继承了Collection接口，里面没有额外的方法。
实现无序（这里的无序是指和元素的添加顺序不一致而已，其实里面有排序规则），不可重复的数据集合。
#### 1.8.7、HashSet
数据结构：基于HashMap实现。
特点：存储无序，不可重复的数据。没有根据索引操作数据的方法。
常用方法：
```
Set<String> hashSet=new HashSet<>();
        hashSet.add("a");
        hashSet.add("b");
        hashSet.add("c");
        hashSet.add("a");//有重复则覆盖
        hashSet.remove("c");
        for (String s:hashSet
             ) {
            System.out.println(s.toString());
        }
```
#### 1.8.10、TreeSet
数据结构：基于TreeMap实现。
常用方法：
```
TreeSet<String> treeSet= new TreeSet<>();
       treeSet.add("a");
       treeSet.add("b");
       treeSet.add("c");
       treeSet.add("a");//覆盖掉相同的值
       treeSet.remove("c");
        for (String s:treeSet
             ) {
            System.out.println(s.toString());
        }
```
#### 1.8.11、Map接口
Map接口是独立的接口，和Collection没关系。
Map中的每一个元素都是一个entry类型，每个元素包含key(键)和value(值)。
Map存储无序（指和添加元素时的顺序不一致），不可重复的元素。
#### 1.8.12、HashMap
HashMap是对散列表的具体实现。
数据结构：数组+链表。
```
Map<String,Object> hashMap=new HashMap<>();
        hashMap.put("1","张三");
        hashMap.put("2","张三");
        hashMap.put("3","李四");
        hashMap.put("3","李四四");//key相同则覆盖掉原来的值
        hashMap.put(null,null);//只允许一个key为null
        hashMap.put("4",null);//可允许多个value为null
        //遍历：
        //keySet遍历：
        for (String key:
                hashMap.keySet()) {
            System.out.println(key);
        }
        //entry遍历
        for (Map.Entry<String,Object> entry:
             hashMap.entrySet()) {
            System.out.println("key:"+entry.getKey().toString()+";"+"value:"+entry.getValue());
        }
```
不足：线程不安全。
特点：只允许一个key为null,多个value为null，但遍历取值的时候有可能报空指针异常。
#### 1.8.13、TreeMap
数据结构：红黑树。
特点：和HashMap类似，不允许key为null。
常用方法：
```
Map<String,Integer> treeMap=new TreeMap<>();
        treeMap.put("张三",20);
        treeMap.put("李四",30);
        treeMap.put("王麻子",40);
        treeMap.put("王麻子",43);//key重复，则覆盖掉原来的value
        treeMap.put(null,43);//不允许key为null的情况,若为key为空，则会报空指针的错误
        treeMap.remove("李四");
        //遍历 1、keySet()
        for (String key:treeMap.keySet()) {
            System.out.println(key);
        }
        //遍历 2、entrySet() 遍历
        for (Map.Entry<String,Integer> entry:treeMap.entrySet()) {
            System.out.println(entry.getKey()+":"+entry.getValue());
        }
```
#### 1.8.14、Iterator
迭代器，是一个接口，每一个集合的实现类的内部基本都有对Iterator的实现，通过Iterator可以实现对集合的遍历。
存在的意义：隐藏集合内部的实现细节，无论那种集合都是通过Iterator操作，而不是直接操作该集合。
实例化：每个集合都有.iterator()方法，该方法的返回值就是一个迭代器对象。
示例：
```
 List<String> arrayList=new ArrayList<>();
        arrayList.add("尾");//尾加
        arrayList.add(1,"一");//插入
        arrayList.add(1,"二");//插入一个新值，原先在这个位置上的将往后移一位
        arrayList.set(1,"3");//覆盖掉原先在这个位置上的值
        arrayList.remove(0);
        arrayList.remove("一");
        //遍历方式：1、for循环、2、增强for、3、迭代器iterator
        Iterator<String> iterator = arrayList.iterator();
        while (iterator.hasNext()){
            System.out.println(iterator.next().toString());
            iterator.remove();//删除当前值
        }
```
#### 1.8.15、Collections
Collections是一个工具类型，一个专门操作集合的工具类
常用方法：
```
List<Integer> arrayList=new ArrayList<>();
      arrayList.add(5);
      arrayList.add(3);
      arrayList.add(1);
      arrayList.add(2);
      arrayList.add(4);
      System.out.println(arrayList.toString());//[5, 3, 1, 2, 4]
      Collections.sort(arrayList);//排序
      System.out.println(arrayList.toString());//[1, 2, 3, 4, 5]
       //添加多个值到集合当中
        Collections.addAll(arrayList,7,8,9,67,68,69);
        System.out.println(arrayList.toString());//[1, 2, 3, 4, 5, 7, 8, 9, 67, 68, 69]
        //取最大值，最小值
        Integer max = Collections.max(arrayList);
        System.out.println(max);//69
        Integer min = Collections.min(arrayList);
        System.out.println(min);//1
```
#### 1.8.16、哈希表
1. 数组在查找时效率很高，但是插入和删除效率却很低，而链表刚好相反。
2. 哈希表可以综合链表和数组的优势，在查找、插入、删除时实现更好的效率。
3. 散列表的存储结构使用的也是数组加链表。
4. 但是极端情况下，散列表也会呈现数组和链表的劣势。
### 1.9、泛型(Generics)
1. JDK1.5引入。
2. 参数化类型。即”先泛泛定义要使用的一个类型，使用的时候再具体指定这个类型“。
3. 在定义类、接口或方法的时候，使用<类型参数>来声名，也称参数化类型，泛型。类型参数将被使用在类、接口或方法的定义中，比如属性定义，方法参数，返回值；在今后具体使用这个类或方法的时候，用具体的一个类型来替代类型参数，作为类型实参。
4. 使用了泛型的类、接口、方法，简称为泛型类、泛型接口、泛型方法。
5. 集合框架里面使用了泛型，这样可以在使用集合的时候，再具体指定使用的元素类型；泛型给集合元素提供了类型检查功能。注意，数组有天然的类型检查功能。
`public class GenericsTest {

    public static void main(String[] args){

       /* Cat<String> cat = new Cat<String>();
        cat.setT("大貓");
        cat.say();*/

        Cat<Integer> cat = new Cat<Integer>(3);
        cat.setT(3);
        cat.say();
    }
}

class Cat<T>{
    T t;

    public T getT(){
        return t;
    }

    public Cat(T t){
        this.t=t;
    }
    public void setT(T t){
        this.t=t;
    }
    public void say(){
        System.out.println(t);
    }
}`
#### 1.9.1、泛型类
1. <>里可以声名多个类型参数，用逗号隔开，比如HashMap。
2. 泛型类在声名构造器的时候直接使用类名，不能加<>,比如：Cat(){}
3. 泛型实参可以是类，数组；基本数据类型不能用于泛型的实参，需要使用包装类。
4. 静态方法不能使用类的泛型。
5. 泛型类作为父类，可以是实参化，则子类不再声名此泛型，不再是泛型类；如果不实参化，则子类需要声明此泛型，依然是泛型类。

`public class GenericsTest {

    public static void main(String[] args){

        Cat<String>  cat = new Cat<>();
    }
}

abstract class PCat<T>{
   T t;
}

class Cat<T> extends PCat<T>{

}`
6. 泛型接口被实现的时候，可以实参化，则实现类不用再声明此泛型，不再是泛型类；如果不实参化，则实现类需要声明此泛型，依然是泛型类。

`public class GenericsTest {

    public static void main(String[] args){

        Cat<String,String>  cat = new Cat<>();
    }
}

interface ICat<T>{
    void eat();
}

abstract class PCat<T>{
   T t;
}

class Cat<T,I> extends PCat<T> implements ICat<I>{

    @Override
    public void eat() {

    }
}`
7. 泛型可以嵌套。
8. 泛型类相同，实参类型不一样的对象不能相互赋值。
9. 创建泛型的数组不能new T[10],可以写为(T[])new Object[10]。
10. 从JDK1.7开始，在创建泛型类对象的时候，可以使用类型推断，省略泛型实参，比如：new Cat<>();
11. 定义了泛型，就应该使用泛型；创建泛型类对象时，如果不使用泛型，则将这个类称为原始类型（raw type），应该避免使用原始类型。
12. 异常类不能定义为泛型类。
#### 1.9.2、泛型方法
1. 用<类型参数>放在方法的返回值前来声明泛型方法。
2. 声明的泛型可以用在方法参数，返回值，以及方法内部。
3. 泛型方法可以是静态方法也可以是非静态方法。
4. 泛型方法可以在泛型类中，也可以在普通类中。如果放在泛型类里，声明的泛型符号尽量和泛型类的泛型符号不同。
5. 注意区分，泛型方法和泛型类使用了类的泛型的方法。
##### 1.9.1.2、为何要使用泛型
1. 强制类型检查。
2. 帮助完成了强制类型转换。
3. 健壮性、可读性增强。
##### 1.9.1.3、类型擦除
1. 泛型只存在于编译期中，泛型只会在编译期使用，然后被擦除，Java不会保留泛型，到运行时并没有泛型。
2. 编译期内部永远把所有类型T视为Object处理，但是，在需要转型的时候，编译期会根据T的类型自动实行安全的强制转型。也就是说，编译器执行类型检查的类型推断，然后在生成字节码之前将泛型清除掉。
3. 类型擦除是Java特有的，是历史遗留的妥协问题，是为了和JDK1.5以前没有泛型的类库兼容。
4. 泛型被擦除，则泛型会被转译成Object类型，如果指定了上限则转译成上限，如：<T extends String>则泛型就被替换成String。
5. 类型擦除，使得泛型能够与之前的JDK版本代码兼容，但却抹掉了泛型、泛型之间的特征，这是它带来的问题。
### 1.10、枚举类
1. 使用enum声名，是一种受限制的类。
2. 枚举类用于定义，列举一组同一种类型的常量，这些常量也可以称为枚举值，枚举成员，枚举实例。枚举成员用常量的方式命名。
3. 枚举成员是有顺序的，序号从0开始。
4. 枚举类的父类为java.lang.Enum。
5. 可以添加属性，方法，private或者”无“的构造器。
6. 因为枚举类已经有父类，所以不能再继承自其他类，但是可以实现接口。
7. 每个枚举成员，可以看做枚举类的常量，在外部使用中代表枚举值。
8. 每个枚举成员，可以看做枚举类的子类，可以构造器传参，可以定义自己的属性，甚至可以重写枚举类的方法。
9. Thread.State为枚举类，java8中新增了DayOfWeek,Month枚举类。
#### 1.10.1、Enum类的方法
values(),valuesOf(),toString(),ordinal(),compareTo(),equals()

```
public class EnumTest {

    public static void main(String[] args){
       Day myDay=Day.TODAY;

       switch (myDay){
           case TODAY:
               System.out.println(Day.TODAY);
               break;
           case TOMORROW:
               System.out.println(Day.TOMORROW);
               break;
           case YESTERDAY:
               System.out.println(Day.YESTERDAY);
               break;
       }
    }

    static enum Day{
        YESTERDAY,TODAY,TOMORROW
    }
}

```
#### 1.11、JAVA创建对象的几种方式
4种创建对象的方法：
使用 new 关键字调用对象的构造器；
使用 Java 反射的 newInstance() 方法；
使用 Object 类的 clone() 方法；
使用对象流 ObjectInputStream的readObject()方法读取序列化对象；

