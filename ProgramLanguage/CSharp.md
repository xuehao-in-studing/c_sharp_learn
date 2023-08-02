## 一、变量和类型  
### 1.1 基本数据类型
  数据类型有以下几类
- 值类型
  - 结构体(int32,int64,double等)
  - 枚举  
- 引用类型
  - 类  
  - 接口
  - 委托

### 1.2 变量
变量表示存储位置，其用途也是存储数据  
广义变量的分类：  
- 静态变量
  静态变量的特点是:  
  1. 与类相关联,而不是与类的实例相关联,因此可以在整个程序内访问.
  2. 会带来线程安全问题,因为可能会有多个线程同时访问和修改静态变量
  3. 节约内存,因为不是每个实例都分配一个静态变量,所以多个实例还会共享同一个静态变量
  4. 可能导致程序耦合
- 实例变量
- 数组元素
- 值参数
- 引用参数
- 输出形参
- 局部变量

### 1.3 修饰符  
- 访问修饰符：public、private、protected、internal等
- 继承修饰符：sealed(禁止重写)、abstract(必须被重写)、virtual(可写可不写)等
- 静态修饰符：static(静态，表示成员属于类而不是对象，例如MATH.PI)、const(定义常量，不能修改)、readonly(成员只能被构造函数或者定义的时候初始化)等
- 其他修饰符：unsafe(用于指针)、async、new(创建对象)等

### 1.4 命名空间  
命名空间限定你所编写程序的变量的作用域，引用命名空间时就可以使用命名空间中的类和方法，相当于python导入包。  


### 1.5 方法参数  
方法参数有以下几种：  
- 值参数，只传值，不传地址。
  - 传入值类型参数时，相当于创建了一个副本，外部变量的值不会被方法修改。  
  - 传入引用类型参数时，外部变量的值会被方法修改(副作用，要避免)。  
- 引用参数，传入的是地址。
  - 传入值类型参数时，外部变量的值会被方法修改。
  - 传入引用类型参数时，相对于c语言当中的指向指针的指针，指向一个地址，该地址指向该引用变量。  
  - 传入参数时，需要加ref，定义时也需要。
- 输出参数 (out)，目的是为了输出
  - 该参数传入的时候可以没有初始化，也就是没有赋值。

## 二、 类
### 2.1 字段和属性  
字段和属性都属于类的成员变量，  
- 可以通过属性来访问和设置字段，相当于对类的字段进行保护，以避免一些异常值。
- 字段和属性都可以用修饰符来修饰，也即可以限定为只读。
  - **字段限定只读的方式：readonly关键字**
  - 属性限制只读的方式：
    - public get，private set，只能在类的内部通过方法来改变属性的值，外部不可以进行修改，但并不是完全只读。
    - public get，没有set，但是有类内的方法来更改属性对应的字段，也不是完全只读。
    - 只有get没有set，也没有方法来设置属性，那么就是完全的只读。
  
### 2.2 计算属性：
- **属性也可以通过动态计算得到**，例如知道一个人的年龄(属性)，想要知道一个人是否可以上班(bool 属性)，就可以通过年龄计算得到属性。
- 设置构造器，**直接在构造器的setter和getter中对属性进行计算**，
  - 优点：每次调用的时候就会计算该属性
  - 缺点：如果调用比较频繁就会浪费计算资源
- 设置构造器，**仅仅有getter，但通过方法对计算属性进行赋值(成为了只读属性)**
  - 优点：仅仅在初始化或者发生改变的时候就进行计算，相比于上面的方法节省了计算资源
  - 缺点：方法会占用内存，浪费内存资源
  
### 2.3 类的声明
- **类的声明默认为internal**，只能在该名称空间内访问。
- 类的修饰符：public、protected、internal、private、sealed、abstract、static

### 2.4类的继承与派生

#### 2.4.1 类的继承
- **继承就是让一个类在基类的基础上，使类的成员进行横向和纵向的扩展**。
- 子类一定是一个父类，而父类不一定是一个子类，就如骑车一定是一个交通工具，但是交通工具不一定是一个汽车。
  - 子类可以调用父类一切可调用的属性和方法(非priavte)，这就是一种继承
  - 此外，父类的变量可以指向子类的实例对象，例如 Object obj = new Car();但是在项目中比较少见。
  - 因为car一定是一个object，所以obj可以指向Car的实例。但是obj变量只能调用Object 的方法和属性，除非子类重写才可以调用被重写的属性和方法。
  - 实例化子类时，一定会先调用父类的构造器。如果父类的构造器含参数，那么可以在子类上使用base(args)让子类构造器给父类传参。
  - 但是**父类的实例构造器不能够被子类所继承**。
- 子类只能有一个一个基类，也就是只能有一个父类，但是可以有多个基接口。

#### 2.4.2 类的访问
- **子类的访问级别不能够超过父类的访问级别**，例如父类是internal，子类就不能是public。
- 限制类的继承问级别：
  - sealed：隐藏的，不可以被继承
  - abstract ：抽象类，不能被实例化
  
#### 2.4.2 类成员的访问
- 限制类的成员访问级别的一些关键字,限制级别由低到高：
  - public：**项目文件都可以访问**
  - internal：**仅限于本项目文件中可以访问**，其他项目就不可以访问(引用了名称空间也不行)。
  - protected：**限制在继承链上，子类才能访问**。但是可以跨程序集，也就是说不是internal的。**更多应用在方法上**
  - private：**仅限于这个类中可以访问**，但是不能其他类中访问。
举一个例子说明：人在开车时肯定不能够直接接触到油箱，只能通过加速、减速、行驶、加油等操作改变油的量。字段设置为private，属性设置为protected，然后可以让子类(宝马A6)也继承父类(机动车)的方法，但是实例对象不能直接改变属性或者字段。

#### 2.4.3 重写和多态
重写就是类在深度上的扩展，用子类的方法去覆盖父类的方法
- 重写语法：父类方法采用virtual修饰符，子类方法采用override修饰符
- 属性也可以进行重写
- 重写的条件：函数成员、可见性、签名保持一致
多态，指的是同一类型的对象在不同情况下表现出不同的状态。
- 多态只能在继承链中实现
- c#当中可以体现多态，是因为变量和对象都是有类型的，object类型的变量可以指向任何子类的对象，从而调用方法时，可以调用被子类重写的方法
- 而python在语言层面不可以实现多态，因为变量没有类型，声明之后就指向特定对象

#### 2.4.4 子类实例化
在**实例化子类时，会先实例化父类**，另外，子类的构造函数要继承父类的构造函数。例如：  
![image](https://github.com/xuehao-in-studing/c_sharp_learn/assets/102791379/a783cf70-5520-4465-9d9f-3ea49586558c)  
这是在父类有无参数构造器的情况下进行实例化的(也可以不调用base，因为系统会默认调用父类的构造器)，如果需要继承有参数的构造器，那么，就需要在base中加入父类的参数。

### 2.5 构造器
每个类都有一个构造器,如果你没有声明构造器,系统会自动定义一个public的无参数构造器.  
静态构造器和静态变量很类似,都是与类本身相关联,而不是与类的实例相关联,**当类出现在程序中时,静态构造器都会被调用,无论是否实例化**.  
构造器分类:
1. 静态构造器
2. 无参构造器
3. 带参构造器

## 三、 委托类型
委托类型相当于c语言当中的函数指针，指向函数的指针，当调用该委托时，也就调用了委托对应的函数。  
**委托类型是一种类，声明时要和类定义在同一个级别**  

### 3.1 c#中的委托类型：  
- Action类型的委托
  - **声明方法：Action name = new Action(void() FunctionName)**
  - 要求委托函数返回空，且参数列表也为空。
  - 调用和下面的差不多，就是没有参数列表和返回值。
- Func类型的委托
  - **声明方法：Func<inType,inType,outType> name = new Func(func)**
  - 其中func要有返回值，也要有两个参数类型。
  - 调用：采用name.invoke(para1,para2)方法，或者像c函数指针一样直接调用name(para1,para2),返回值赋给一个变量。
- 自定义的委托：
  - **定义方法：修饰符 delegate 返回值 函数名(参数列表)**
  - 例如： public delegate int Cal(int x,int y);
  - 委托与所封装的方法必须类型兼容，(参数类型和返回值类型)
  - 委托的**声明位置**要注意，一般是**名称空间内，类体外部**

注：委托的功能很强大，但是要正确使用。**不正确使用会带来很严重的后果！**
使用途径：
- 模板方法：委托有返回值，相当于填空题
- 回调方法：委托无返回值，相当于流水线
缺点：
- 使用不当会导致内存泄漏，程序性能下降
- 紧耦合方法，现实工作要慎重
- 可读性下降
- 把委托、异步调用、多线程缠在一起会导致代码难以维护

## 四、事件
### 4.1 事件的组成部分：
1. 事件源(source)
2. 事件成员(event),一般来说是属于事件源的
3. 事件订阅者(subscriber)
4. 事件处理器(hanlder)，一般来说，就是对象的方法，来处理事件,包含了事件触发逻辑
5. 事件订阅  
可以从一个简单的例子来描述，就拿狗发疯后它周边的人会跑这个事件来说，狗就是事件源，人就是事件订阅者，发生的事件就是狗叫人跑，但这看起来好像只有三部分，但还有一点，人必须订阅这个事件，为什么北京的狗发疯了，纽约的人不会跑，因为这个对他们没有影响，也就是说，他们没有订阅这个事件，必须北京的人会跟狗发疯这件事相关，也就是北京人订阅了狗发疯。

### 4.2 如何定义一个事件
c#语法中事件是委托的一种，定义事件必须通过委托定义。  
1. 首先定义事件委托类型(实际也是一个类)，语法如下：delete type ***EventHandler(obj,args);  
![image](https://github.com/xuehao-in-studing/c_sharp_learn/assets/102791379/c21bed81-479f-4571-95ed-3e1d5111e459)
2. 定义事件委托参数(实际是一个类)，语法：修饰符 class ***eventargs:EventArgs;  
 ![image](https://github.com/xuehao-in-studing/c_sharp_learn/assets/102791379/42021de0-73c7-4551-aeae-83c343179e85)
3. 定义事件发布者(类) 语法：修饰符 class PublisherName;  
4. 定义事件成员(在事件发布者内部)  语法：event ***EventHandler EventHandlerName{add;remove}(详细声明);
5. 定义事件处理器(是个方法,在事件订阅者内部)
 ![image](https://github.com/xuehao-in-studing/c_sharp_learn/assets/102791379/6e1214b5-0ffe-49bb-be94-6d4f13705133)
7. 定义事件订阅者 语法： 修饰符 class SubscriberName;
8. 定义事件逻辑  
![image](https://github.com/xuehao-in-studing/c_sharp_learn/assets/102791379/dc0a3890-44fd-40e4-9cc8-4f877d582aac)



## 四、接口和抽象类
从具体类到抽象类再到接口，抽象程度越来越高，实现的东西越来越少。

### 3.1 抽象类
抽象类是多个类方法和属性的抽象，不涉及逻辑和具体实现，两个用处：  
- 作为基类，让子类去实现
- 作为变量类型，实现多态
抽象类的语法：  
- **抽象的属性和抽象的方法都用abstract来进行修饰**，并且方法不能够实现，得让子类实现。
- 所有的成员不能够被private修饰。

#### 3.1.1开闭原则 Open access
对于一个类，我们应当把那些不改变的，功能固定的成员封装好，而不再改变其具体实现。把一些不确定的推迟给子类去实现。

### 3.2 接口
接口，是一种集大成的抽象类，里面所有涉及到的类成员都是抽象的，语法要求**所有的类成员都是public**  
- 接口和抽象类都不可以实例化。
- **接口是为了解耦合而生，高内聚，低耦合**
- 接口是完全未实现逻辑的类
 
### 3.3依赖反转
依赖：服务的提供者和服务的使用者有着较紧的依赖关系，例如汽车一定需要引擎，而引擎坏了，汽车也就没用了。  
可以通过以下的这张图进行理解：  
一、本来是驾驶员对象与车对象进行强依赖关系，如果改变驾驶员与车的关系，需要改变内部的类的声明以及引用逻辑。  
二、采用右图的接口之后，驾驶员对象与车对象没有了强依赖，转而改变成了两个接口之间的依赖关系，驾驶员与车对象之间可以来回切换。
![image](https://github.com/xuehao-in-studing/c_sharp_learn/assets/102791379/72bef90f-24fe-43dc-a8bc-263543d339c2)





## 四、项目文件基本知识
- 一个解决方案可以由多个项目组成，每一个项目又可以被多个解决方案同时引用。
- 项目右键点击可以选择该项目生成库还是生成可执行文件，也可以配置路径，一般是项目名\bin\debug。
- 
### 4.1 快捷操作(visual studio)
注意有的时候快捷键会发生冲突，这时候就需要重新设置。  
- 代码重构：ctrl+r 然后ctrl+r(rename),+i(interface),+e(封装字段)
- 代码快捷生成：prop、propfull生成类属性，indexer生成索引器
- tab键可以快捷生成和补全代码
- 注释：ctrl+k 然后 ctrl+/
- 全局代码格式化：ctrl+k 然后 ctrl+d

