# 基本语法

## 数据

数字字面量加下划线，如用1_000_00（0或0b1111_0100_0010_0100_0000）表示一百万。这些下划线只是为了让人更易读。Java编译器会去除这些下划线。

Java没有任何无符号（unsigned）形式的`int`、`long`、`short`或`byte`类型。

float类型的数值有一个后缀F或f（例如，3.14F）。没有后缀F的浮点数值（如3.14）默认为double类型。

利用关键字`final`指示常量。`static final`指示类常量。  
```java
final PI = 3.14
```

## 运算

整数被0除将会产生一个异常，而浮点数被0除将会得到无穷大或NaN结果。

 - 如果两个操作数中有一个是`double`类型，另一个操作数就会转换为`double`类型。
 - 否则，如果其中一个操作数是`float`类型，另一个操作数将会转换为`float`类型。
 - 否则，如果其中一个操作数是`long`类型，另一个操作数将会转换为`long`类型。
 - 否则，两个操作数都将被转换为`int`类型。

 ## 字符串

` equals`方法检测两个字符串是否相等。  
 `s.equal(t)` s与t可以是字符串变量，也可以是字符串字面量。  
 `"hello".equal("hello")`

 如果虚拟机始终将相同的字符串共享，就可以使用`==`运算符检测是否相等。但实际上只有字符串常量是共享的，而`+`或`substring`等操作产生的结果并不是共享的。

 空串和null串是不同的。  
 空串""，null串是没有分配内存。

 ## 输入与输出

 构造输出对象，`print`方法输出，要close文件。

```java
PrintWriter out = new PrintWriter("OutFile.txt", "UTF-8");
out.print("123");
out.close();
```

# 面向对象

## 构造器

请注意，不要在构造器中定义与实例域重名的局部变量。例如，下面的构造器将无法设置salary。  

```java
public Employee(String n, double s, ...) {
    String name = n; // Error
    double salary = s; // Error
}
```

如果没有初始化类中的域，将会被自动初始化为默认值（0、false或null），可以理解为内含了一个默认的无参构造器。如果类中提供了至少一个构造器，但是没有提供无参数的构造器，则在构造对象时如果没有提供参数就会被视为不合法。  
可以显示给出以下构造器
```java
public ClassName() {

}
```

可以通过静态方法，在构造器中对实例域进行赋值变量

```java
class Employee {
    private static int nextID:
    private int id = assignID();

    private static int assignID() {
        int r = nextID:
        nextID++;
        return r;
    }
}
```

构造器调用构造器，必须在第一行。

```java
public Employee(double s) {
    this("Employee #" + nextID, s); // call the other constructer
    nextID++;
}
```

初始化块，在静态域中运用较好

```java
class Employee {
    private staic int nextID;

    private int id;

    // object initialization block
    {
        id = nextID;
        nextID++;
    }
}
```

```java
static
{
    // 对静态域进行赋值
}
```

构造器具体执行步骤  
1. 所有数据域被初始化为默认值（0、false或null）。
2. 按照在类声明中出现的次序，依次执行所有域初始化语句和初始化块。
3. 如果构造器第一行调用了第二个构造器，则执行第二个构造器主体。
4. 执行这个构造器的主体

## 包

### 导入静态域和静态方法

```java
import static java.lang.System.*;
out.println("");
```

### 将类放入包

如果没有在源文件中放置package语句，这个源文件中的类就被放置在一个默认包（defaulf package）中。默认包是一个没有名字的包。

## 继承

子类不可以访问超类的私有域。  
超类不可以调用子类特有的方法。

```java
Employee aManager = new manager();
aManager.setBonus() // error
```

返回类型不是方法签名的一部分。在覆盖方法时，一定要保证返回类型的兼容性。允许子类将覆盖方法的返回类型定义为原返回类型的子类型。

`Manager boss = (Manager) aEmployee;`
1. 只能在继承层次内进行类型转换。
1. 在将超类转换成子类之前，应该使用`instanceof`进行检查。

一般情况下，应该尽量少用类型转换。如果超类要调用子类的特殊方法，那么说明类的设计不合理。这个方法需要添加到超类中去。

### 抽象类

抽象类不可以被实例化。类中不包含抽象方法，也可以声明为抽象类。（作用，不让此类实例化）    
抽象类可以包含具体的数据和方法。  
在子类中实现抽象方法，或者继续保留`abstract`关键字。

关键字`protected`对本包和所有子类可见，域和方法
1. 仅对本类可见——`private`
2. 对所有类可见——`public`
3. 对本包和所有子类可见——`protected`
4. 对本包可见——默认（很遗憾），不需要修饰符。

## 参数数量可变方法

```java
/**
* printf方法定义
*/
public class PrintStream {
    public PrintStream printf(String fmt, Object... args) {return format(fmt, args);}
}

// 用for(:)进行遍历
for (Object i : args) {
    
}
```

## 枚举类

比较两个枚举类的值时，直接使用`==`

## 反射

`Class`类中的`getFields`、`getMethods`和`getConstructors`方法将分别返回类提供的public域、方法和构造器数组，其中包括超类的公有成员。`Class`类的`getDeclareFields`、`getDeclareMethods`和`getDeclaredConstructors`方法将分别返回类中声明的全部域、方法和构造器，其中包括私有和受保护成员，但不包括超类的成员。  
`AccessibleObject`类，它是`Field`、`Method`和`Constructor`类的公共超类。
`void setAccessible(boolean flag)`是其中一个方法， 他为反射对象设置可访问标志。`flag`为`true`表明屏蔽Java语言的访问检查，使得对象的私有属性也可以被查询和设置。

1. `Object get(Object obj)`返回obj对象中用Field对象表示的域值。
1. `void set(Object obj, Object newValue)`用一个新值设置Obj对象中Field对象表示的域。
1. `public Object invoke(Object implicitParameter, Object[ ]explicitParamenters)`调用任意方法

# 接口

接口是一个规范，别人或者库函数要调用自己写的代码。  
实现接口有下面两个步骤：
1. 将类声明为实现给定的接口。
2. 对接口中的所有方法进行定义。

接口中的方法都自动地被设置为`public`，接口中的域将被自动设为`public static final`。java规范中不建议写多余的修饰关键字。
```java
class Employee implements Comparable<Employee> {
    public int compareTo(Employee other) {
        return Double.compare(salary, other.salary);
    }
}
```

接口不是类，不能被实例化。但是可以声明接口变量。  
用`instanceof`检查是否实现了某个接口。  
接口可以像继承一样扩展。
```java
Comparable x = new Employee();
if (anObject instanceof Comparable) {};
```

# lambda表达式

可以理解为匿名函数。将函数抽象为表达式传入方法。  
`(`参数`) ->` 函数主体  

```java
(String first, String second) -> first.length() - second.length()
// 无参写法
() -> { return 1 }
```
其中返回类型由上下文得出，无需指定。实际上，参数类型若能推导出也无需指定，一个参数时甚至可以省略`()`

```java
Comparator<String> comp = (first, second) -> first.length() - second.length();
```

## 函数式接口

对于只有一个抽象方法的接口，需要这种接口的对象时，就可以提供一个lambda表达式。这种接口称为函数式接口（functional interface）。

## 方法引用

已经有现成的方法可以完成你想要传递到其他代码的某个动作。（想要传递到接口的函数是库函数）

```java
Timer t  = new Timer(1000, System.out::println);
Arrays.sort(strings, String::compareToIgnoreCase);
```

`::`分隔方法名与对象或类名：
1. object::instanceMethod
1. Class::staticMethod
1. Class::instanceMethod

前两种等价与提供方法参数的lambda表达式，`System.out::println`等价于`x -> System.out.println(x)`。  
`this::instanceMethod`和`super::instanceMethod`也是合法的

## 构造器引用

具体调用那个构造器取决于上下文，根据参数。
```java
Person::new
//int[]::new等价于x -> new int[x]
```

## 闭包

lambda表达式三个部分
1. 代码块
1. 参数
1. 自由变量的值，这是指非参数而且不在代码中定义的变量。

lambda会捕获自由变量的值。  
lambda表达式中捕获的变量必须实际上是最终变量（effectively final）。

## lambda的延迟执行

- 在一个单独的线程中运行代码；
- 多次运行代码；
- 在算法的适当位置运行代码（例如，排序中的比较操作）；
- 发生某种情况时执行代码（如，点击了一个按钮，数据到达，等等）；
- 只在必要时才运行代码。

# 内部类

使用内部类原因
- 内部类方法可以访问该类定义所在的作用域中的数据，包括私有的数据。
- 内部类可以对同一个包中的其他类隐藏起来。
- 当想要定义一个回调函数且不想编写大量代码时，使用匿名（anonymous）内部类比较便捷。

# 代理

# 异常

## 异常分类
- Error
- Exception：RuntimeException

在方法后声明可能会抛出的异常  
如果方法没有声明所有可能发生的受查异常，编译器就会发出一个错误消息。

## 处理异常

### 抛出异常

1. 找到或创建一个合适的异常类
1. 创建这个类的一个对象
1. 将对象抛出

### 捕获异常

捕获异常必须设置try/catch子句

- 如果在try语句块中的任何代码抛出了一个在catch子句中说明的异常类
    1. 程序将跳过try语句块的其余代码。
    2. 程序将执行catch子句中的处理器代码。
- 如果在try语句块中的代码没有抛出任何异常，那么程序将跳过catch子句。  
- 如果方法中的任何代码抛出了一个在catch子句中没有声明的异常类型，那么这个方法就会立刻退出（希望调用者为这种类型的异常设计了catch子句）。


```java
try {
    code
}
catch (ExceptionType e1) {
    handle e1
}
catch (ExceptionType e2) {
    handle e2
}
catch (ExceptionType1 | ExceptionType2 e3) {
    handle e3
}
```
多个catch子句捕获多个异常，同时一个catch子句捕获多个不同继承树上的异常。

```java
try { 
    code
}
catch (SQLException e) {
    Throwable se = new ServletException("database error");
    se.initCause(e)
    throw se;
}
```
通常，应该捕获那些知道如何处理的异常，而将那些不知道怎样处理的异常继续进行传递。可以包装后再抛出，使用`Throwable e = se.getCause()`重新得到原始异常。

### finally子句

不管是否有异常被捕获，finally子句中的代码都被执行。可用于关闭方法中所分配的资源，不需要在正常代码和异常代码中都写一遍。

try语句可以只有finally子句，而没有catch子句。且建议解耦合try/catch和try/finally语句块。
```java
InputStream in = ...;
try {
    try {
        code
    }
    finally {
        in.close();
    }
}
catch (IOException e) {
    handle
}
```

try中抛出异常，catch也可能抛出异常，finaly也可能抛出异常。嵌套加嵌套，子子孙孙无穷尽也。

带资源的try子句，自动关闭资源。当close方法抛出异常时会被抑制，try中的异常会被重新抛出。
```java
try (Resource res = ...) {
    code
}
```

早抛出，晚捕获。

# 断言

```java
assert 条件;
assert 条件:表达式;
```

# 泛型

Generic programming 编写的代码可以被不同类的对象所使用，类似于数组 `[]`，声明后可以有各种不同数据类型的数组。不需要为每个类设计一个数据结构。

## 类型参数 type parameters

没有泛型类之前，用继承实现泛型。数据结构用一个Object类作为所有操作对象的类。
```java
public class ArrayList {
    private Object[] elementData;
    ...
    public Object get(int i) {}
    public void add(Object o) {}
}
```
导致两个问题：
1. 方法输出的对象类型为Object，必须进行强制类型转换。
```java
ArrayList files = new ArrayList();
string filename = (String) files.get(0);
```
2. 方法输入的对象类型也为Object，可以添加任何类（不同于当前处理的类）而没有错误检查。
```java
files.add(new File("..."));
```

类型参数是所要操作对象的类。
```java
ArrayList<String> files =  new ArrayList<String> ();
ArrayList<String> files =  new ArrayList<> (); //两行代码等价，后者会自行推断变量类型
```

## 类型变量

类型变量是类型参数的抽象，指代所操作对象的类。用`<T>`尖括号加变量名表示。  
编写代码时，所有的用到此类名的地方全部用变量名代替。
```java
public classs Pair<T> {
    private T first;
    public Pair(T first) {}
    public T getFirst() {}
}
```

### 类型变量的限定

有时候需要对类型变量加以约束，规定其必须继承某个类或实现某个接口。  
在Java的继承中，可以根据需要拥有多个接口超类型，但限定中至多有一个类。如果用一个类作为限定，它必须是限定列表中的第一个。

```java
public static <T extends Comparable & Serializable> T min(T[] a)
```
使用关键字`extends`和`&`约束。

## 泛型方法

泛型方法可以定义在普通类中，也可以定义在泛型类中。
```java
class ArrayAlg {
    public static <T> T getMiddle(T... a) {}
}

String middle  = ArrayAlg.<String>getMiddle("...");
// 使用时要指定具体类型，当然编译器会尝试推断，有时可以不写。
```
类型变量放在修饰符（这里是public static）的后面，返回类型的前面。

## 泛型代码和虚拟机

虚拟机中没有泛型类型对象，所有对象都属于普通类。

### 类型擦除

泛型类型会自动替换为相应的原始类型（raw type)。
原始类型的名字就是删去类型参数后的泛型类型名。
擦除（erased）类型变量，并替换为限定类型（无限定的变量用Object）。  
原始类型用第一个限定的类型变量来替换，如果没有给定限定就用Object替换。  
为了提高效率，应该将标签（tagging）接口（即没有方法的接口）放在边界列表的末尾。

### 翻译泛型表达式

擦除方法返回的泛型类型后将返回原始类型，但编译器会强制类型转换为泛型规定的类型。

### 翻译泛型方法

当继承一个泛型类，并且重写包含泛型参数的方法会失败，因为泛型类中泛型参数实质为`object`类。这样一来就有两个名字相同，参数不同的方法。

```java
class DateInterval extends Pair<LocalDate> {
    public void setSecond(LocalDate second) {

    }
}

// after erasure
class DateInterval extends Pair {
    public void setSecond(LocalDate second) {

    }
}

//存在一个从Pair继承的setSecond方法
public void setSecond(Object second) {}

// bridge method 覆盖继承的setSecond方法
public void setSecond(Object second) {
    setSecond((Date) second);
}
```

- 虚拟机中没有泛型，只有普通的类和方法。
- 所有的类型参数都用它们的限定类型替换。
- 桥方法被合成来保持多态。
- 为保持类型安全性，必要时插入强制类型转换。 

## 泛型的约束和局限性

由于类型擦除的原因，

- 不能用基本类型实例化类型参数
- 运行时类型查询只适用于原始类型
- 不能创建参数化类型的数组  
`Pair<String>`和`Pair<Integer>`将都被视为Pair，放进同一个数组。
- Varargs警告  
可是用`@SafeVarargs`取消警告。
- 不能实例化类型变量
- 不能构造泛型数组

## 泛型类型的继承

类型变量没有继承，类型有继承。`Pair<Manager>`和`Pair<Employee>`没有关系，`ArrayList`继承`List`，那么`ArrayList<Manger>`继承`List<Manger>`。

## 通配符

# 集合

基于泛型的集合雷数据结构，其接口和实现分离。

## 迭代器

是一个对象接口，用于依次访问集合中的元素。通过`next`方法访问元素，和文件流类似。应该将java迭代器的位置认为是位于两个元素之间。当调用`next`时，迭代器就越过下一个元素，并返回刚刚越过的那个元素的引用。

## 继承结构

`Collection`接口->`AbstractCollection`抽象类，将基础方法`size`和`iterator`抽象化，将`contain`方法实现了。