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