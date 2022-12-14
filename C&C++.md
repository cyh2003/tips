语言风格
--------

### 函数

1.  可以采用指针参数实现一个函数"返回"多个值的效果

算法
----

### 预处理

1.  二分前先使数组有序
2.  注意隐藏边界（长度为`0`,`1`等）
3.  先排序再计算往往可以简化计算过程
4.  有可能样例输入有序，测试点输入无序
5.  注意图的输入中的重边和自环，以及有向输入转化为无向图

### 算法执行

1.  二分区间的开闭由具体问题决定
2.  递归算法需要数组记录答案时可以不用"触底"时全部修改，然后利用一个全局的`bool`变量连续退出，而是可以回溯时逐步修改，从而减小代码复杂度
3.  注意浮点数计算的上下浮动
4.  尝试将$n!$（排列）转化为$2^n$（组合）
5.  注意在寻找最高位非`0`数（常见于高精度或多项式等问题）时要考虑全为`0`导致循环变量等于`-1`这一`corner case`

### 算法评估

1.  计算递归算法复杂度可先计算递归实例的数量

实用函数
--------

### 输入输出

1.  可以使用`freopen`函数进行输入输出重定向（但可能不如命令行加重定向符号好用）

### 排序

1.  `qsort`在数据量较小时采用插入排序，数据量中等时采用归并排序，在数据量较大时采用快速排序，且快速排序采用三数取中法，时间上较为稳定

### 随机数

1. `rand`函数可以用来生成伪随机数，使用`srand`函数设置种子
2. `rand`函数不能跨平台，具体而言，`RAND_MAX`在`Windows`上为`32767`，在`Linux`（`WSL`）上为`2147483647`

语法特性
--------

### 面向过程

1.  尝试使用`cassert`头文件，用其中的`assert`宏进行运行时断言
2.  `C/C++`中位运算的优先级很低，低于四则运算和比较运算，需要注意括号的使用
3.  注意循环嵌套中，循环变量`i`、`j`、`k`等不要重复使用
4.  循环体中的变量地址不变
5.  使用`getchar`前注意去除`cin`等留下的回车等干扰字符
6.  注意数组下标越界有可能完全无异常（越在其他变量内部）
7.  `switch`分支结构注意用`break`
8.  在`C++`中，不同的函数、结构体、类可以声明同名的静态变量，彼此独立
9.  只有第一次进入函数时静态变量会初始化，之后进入会忽略初始化语句，故静态函数调用计数可利用函数体中的局部静态变量
10.  在`C`中，结构体和联合不会引入新的变量作用域，不能声明静态成员变量
11.  可以使用位域直接操作内存中的位

### 面向对象

1.  对象内部局部变量需要初始化
2.  注意写`public`（默认为`private`）
3.  友元函数函数不是成员函数，不能加作用域符号
4.  引用本质只是别名，其创建时不会产生任何构造过程
5.  注意避免自身赋值
6.  当一个内部类或内部对象需要访问外部对象时，尽量通过外部对象成员变量的指针来访问，否则有可能出现构造顺序或访问权限的问题
7.  尽量不要创建野指针，如果不可避免要创建野指针，一定要初始化为`nullptr`
8.  移动构造、赋值前注意删除当前指针的内容，避免当前指针赋新值后内存泄漏
9.  `delete`前对象最好指针最好不是`nullptr`，`delete`后对象指针最好置为`nullptr`
10.  在返回值和参数均可被析构时，先析构返回值，再析构参数（符合栈的顺序）
11.  静态成员变量要在`main`函数前进行初始化
12.  虚函数/常量函数不能为静态函数，因为其调用/参数中需要/含有`this`指针
13.  模板函数将成员函数作为形参时，成员函数应设为静态函数，非静态成员函数因为有`this`指针形参，参数数量不一致，可能导致错误（`sort`）
14.  `std::move()`对常引用无效
15.  派生类新定义的非虚函数和新定义的变量会在函数形参为值/引用/指针（所有情况）时被切片
16.  重写函数调用时，与所有当前形式类中的函数同名且参数不同的函数会被隐藏，然后按虚函数机制调用
17.  在派生类没有直接写出新函数的情况下，派生类不会自动生成新的虚函数继承版本，而是在虚函数表中沿用旧版本（注意与重写隐藏的关系）
18.  基类指针指向派生类对象时，调用被基类声明、派生类继承的虚函数不需要`dynamic_cast`，调用派生类声明的函数需要`dynamic_cast`
19.  模板的声明与实现需要在同一文件中（模板实例化在编译期确定）

底层机制
--------

### 多进程/多线程

1.  `volatile`（易变）关键字影响编译器的编译，使得即使进行了优化，每次访问此变量时都会重新从内存取值，从而避免信号处理/其他线程在不经意间修改此变量造成的数据不一致的问题

Qt
--

### 技巧与特性

1.  `Qt`画圆的坐标原点为外界矩形的左上角点
2.  在防止程序僵死时，直接使用信号槽实现异步相比使用多线程要更为简单