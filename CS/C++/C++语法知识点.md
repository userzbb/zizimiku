# C++语法知识点详解

## 目录
1. [C++基础语法](#基础语法)
2. [面向对象编程](#面向对象编程)
3. [模板编程](#模板编程)
4. [STL标准模板库](#STL标准模板库)
5. [内存管理](#内存管理)
6. [异常处理](#异常处理)
7. [文件操作](#文件操作)
8. [高级特性](#高级特性)

---

## 基础语法

### 1. 输入输出流

#### C风格 vs C++风格
```cpp
// C风格
#include <stdio.h>
scanf("%d", &num);
printf("数字是: %d\n", num);

// C++风格
#include <iostream>
using namespace std;
cin >> num;
cout << "数字是: " << num << endl;
```

#### iostream库详解
```cpp
#include <iostream>
#include <iomanip>  // 格式化输出

int main() {
    int a = 42;
    double b = 3.14159;
    
    // 基本输入输出
    cout << "请输入一个数字: ";
    cin >> a;
    
    // 格式化输出
    cout << setw(10) << a << endl;           // 设置宽度
    cout << setprecision(2) << b << endl;    // 设置精度
    cout << hex << a << endl;                // 十六进制输出
    cout << oct << a << endl;                // 八进制输出
    
    return 0;
}
```

### 2. 引用类型详解

#### 引用的基本概念
引用（Reference）是C++中的一个重要特性，它为已存在的变量提供一个别名。

```cpp
int main() {
    // 基本引用语法：类型& 引用名 = 被引用的变量;
    int x = 10;
    int& ref = x;  // ref是x的引用（别名）
    
    cout << "x = " << x << endl;        // 输出: 10
    cout << "ref = " << ref << endl;    // 输出: 10
    
    // 引用和原变量共享同一内存地址
    cout << "x的地址: " << &x << endl;
    cout << "ref的地址: " << &ref << endl;  // 与x相同
    
    ref = 20;  // 修改引用，x也会改变
    cout << "修改ref后，x = " << x << endl;        // 输出: 20
    
    return 0;
}
```

#### 引用的语法规则和特性

**1. 声明语法**
```cpp
// 基本语法: 数据类型& 引用名 = 初始化变量;
int a = 100;
int& ref_a = a;        // 正确：引用必须在声明时初始化

// 错误示例
// int& ref_b;         // 错误：引用必须初始化
// int& ref_c = 50;    // 错误：不能引用字面值（常量）
```

**2. 引用的不可变性**
```cpp
void reference_immutability() {
    int x = 10, y = 20;
    int& ref = x;      // ref引用x
    
    cout << "ref = " << ref << endl;  // 输出: 10
    
    ref = y;           // 注意：这不是让ref引用y，而是将y的值赋给x！
    cout << "x = " << x << endl;      // 输出: 20（x的值变了）
    cout << "y = " << y << endl;      // 输出: 20（y的值没变）
    
    // ref仍然引用x，不能改变引用关系
    cout << "ref仍然引用x，地址相同: " << (&ref == &x) << endl;  // 输出: 1
}
```

**3. 不同数据类型的引用**
```cpp
void different_type_references() {
    // 整数引用
    int num = 42;
    int& int_ref = num;
    
    // 浮点数引用
    double pi = 3.14159;
    double& double_ref = pi;
    
    // 字符引用
    char ch = 'A';
    char& char_ref = ch;
    
    // 字符串引用
    string str = "Hello";
    string& str_ref = str;
    
    // 数组元素引用
    int arr[5] = {1, 2, 3, 4, 5};
    int& first_element = arr[0];  // 引用数组的第一个元素
    
    first_element = 100;  // 修改数组第一个元素
    cout << "arr[0] = " << arr[0] << endl;  // 输出: 100
}
```

**4. 常量引用（const引用）**
```cpp
void const_reference_demo() {
    int x = 10;
    
    // 普通引用
    int& normal_ref = x;
    normal_ref = 20;  // 可以修改
    
    // 常量引用：不能通过引用修改原变量
    const int& const_ref = x;
    // const_ref = 30;  // 错误：不能修改const引用
    
    cout << "通过const引用读取: " << const_ref << endl;  // 可以读取
    
    // 常量引用可以绑定到临时对象
    const int& temp_ref = 42;     // 正确：可以引用字面值
    const string& str_ref = "Hello";  // 正确：可以引用临时字符串
    
    // 常量引用延长临时对象的生命周期
    const string& extended_life = string("Temporary") + " Object";
    cout << extended_life << endl;  // 临时对象被延长生命周期
}
```

#### 引用的高级语法特性

**5. 引用作为函数参数**
```cpp
// 引用参数的各种用法
void swap_by_reference(int& a, int& b) {
    int temp = a;
    a = b;
    b = temp;
}

void print_info(const string& name, const int& age) {
    // const引用参数，避免拷贝且不能修改
    cout << "姓名: " << name << ", 年龄: " << age << endl;
}

void modify_array(int (&arr)[5]) {
    // 数组引用参数，必须指定大小
    for (int i = 0; i < 5; i++) {
        arr[i] *= 2;
    }
}

void reference_parameter_demo() {
    int x = 10, y = 20;
    cout << "交换前: x=" << x << ", y=" << y << endl;
    swap_by_reference(x, y);
    cout << "交换后: x=" << x << ", y=" << y << endl;
    
    print_info("张三", 25);  // 传递字面值，会自动创建临时对象
    
    int arr[5] = {1, 2, 3, 4, 5};
    modify_array(arr);
    for (int val : arr) {
        cout << val << " ";  // 输出: 2 4 6 8 10
    }
    cout << endl;
}
```

**6. 引用作为函数返回值**
```cpp
class IntArray {
private:
    int data[10];
    int size;

public:
    IntArray() : size(0) {
        for (int i = 0; i < 10; i++) data[i] = 0;
    }
    
    // 返回引用，允许修改
    int& operator[](int index) {
        if (index < 0 || index >= 10) {
            throw out_of_range("索引超出范围");
        }
        return data[index];  // 返回引用
    }
    
    // 返回const引用，只读访问
    const int& operator[](int index) const {
        if (index < 0 || index >= 10) {
            throw out_of_range("索引超出范围");
        }
        return data[index];  // 返回const引用
    }
    
    // 返回引用的链式调用示例
    IntArray& set(int index, int value) {
        data[index] = value;
        return *this;  // 返回自身引用，支持链式调用
    }
};

void reference_return_demo() {
    IntArray arr;
    
    // 通过引用修改数组元素
    arr[0] = 100;
    arr[1] = 200;
    
    // 链式调用
    arr.set(2, 300).set(3, 400).set(4, 500);
    
    cout << "arr[0] = " << arr[0] << endl;  // 输出: 100
}
```

**7. 引用的类型转换和兼容性**
```cpp
void reference_compatibility() {
    // 基本类型之间的引用
    int a = 42;
    int& ref_a = a;           // 直接引用
    
    // 不能直接引用不同类型
    // double& ref_d = a;     // 错误：类型不匹配
    
    // 可以通过强制转换（危险）
    double& ref_d = reinterpret_cast<double&>(a);  // 危险操作
    
    // 继承关系中的引用
    class Base { public: virtual ~Base() {} };
    class Derived : public Base {};
    
    Derived d;
    Base& base_ref = d;       // 正确：派生类可以引用为基类
    // Derived& derived_ref = base_ref;  // 错误：需要显式转换
    
    // 使用dynamic_cast安全转换
    Derived& derived_ref = dynamic_cast<Derived&>(base_ref);
}
```

**8. 引用与指针的详细对比**
```cpp
void detailed_reference_vs_pointer() {
    int x = 10, y = 20;
    
    // === 1. 初始化差异 ===
    int* ptr;              // 指针可以不初始化
    // int& ref;           // 错误：引用必须初始化
    int& ref = x;          // 引用必须在声明时初始化
    
    // === 2. 重新赋值差异 ===
    ptr = &x;              // 指针可以指向x
    ptr = &y;              // 指针可以重新指向y
    
    ref = y;               // 这不是重新引用，而是赋值操作！
    cout << "x = " << x << endl;  // x的值变成了20
    
    // === 3. 空值差异 ===
    ptr = nullptr;         // 指针可以为空
    // ref = nullptr;      // 错误：引用不能为空
    
    // === 4. 内存占用 ===
    cout << "指针大小: " << sizeof(ptr) << " 字节" << endl;      // 通常8字节（64位）
    cout << "引用大小: " << sizeof(ref) << " 字节" << endl;      // 与被引用对象相同
    
    // === 5. 语法差异 ===
    int* ptr2 = &x;
    cout << "指针值: " << ptr2 << endl;       // 打印地址
    cout << "指针指向的值: " << *ptr2 << endl; // 需要解引用
    
    int& ref2 = x;
    cout << "引用值: " << ref2 << endl;       // 直接打印值
    cout << "引用地址: " << &ref2 << endl;    // 打印地址（与x相同）
    
    // === 6. 数组访问 ===
    int arr[5] = {1, 2, 3, 4, 5};
    int* arr_ptr = arr;
    int (&arr_ref)[5] = arr;  // 数组引用语法
    
    cout << "通过指针: " << arr_ptr[2] << endl;    // 输出: 3
    cout << "通过引用: " << arr_ref[2] << endl;    // 输出: 3
    
    arr_ptr++;             // 指针可以进行算术运算
    // arr_ref++;          // 错误：引用不能进行算术运算
}
```

**9. 引用的陷阱和注意事项**
```cpp
// 危险示例：返回局部变量的引用
int& dangerous_function() {
    int local_var = 42;
    return local_var;      // 危险！返回局部变量的引用
}  // local_var在这里被销毁

// 安全示例：返回静态变量或成员变量的引用
int& safe_function() {
    static int static_var = 42;
    return static_var;     // 安全：静态变量生命周期长
}

// 临时对象的引用
string create_string() {
    return "Hello World";
}

void reference_pitfalls() {
    // === 1. 悬空引用 ===
    // int& dangerous_ref = dangerous_function();  // 危险！悬空引用
    int& safe_ref = safe_function();               // 安全
    
    // === 2. 临时对象的引用 ===
    // string& temp_ref = create_string();        // 错误：不能引用临时对象
    const string& temp_ref = create_string();     // 正确：const引用可以绑定临时对象
    
    // === 3. 引用的初始化顺序 ===
    class RefClass {
        int& ref_member;
        int normal_member;
    public:
        // 必须在初始化列表中初始化引用成员
        RefClass(int& r) : ref_member(r), normal_member(0) {}
        // RefClass(int& r) : normal_member(0), ref_member(r) {}  // 也可以，但顺序最好与声明一致
    };
    
    // === 4. 引用数组 ===
    int a = 1, b = 2, c = 3;
    // int& ref_array[3] = {a, b, c};  // 错误：不能创建引用数组
    
    // 但可以创建数组的引用
    int arr[3] = {1, 2, 3};
    int (&array_ref)[3] = arr;        // 正确：数组引用
    
    // === 5. 引用的引用 ===
    int x = 10;
    int& ref1 = x;
    int& ref2 = ref1;      // 这实际上是x的另一个引用，不是引用的引用
    cout << "ref2 = " << ref2 << endl;  // 输出: 10
}
```

**10. 引用在现代C++中的最佳实践**
```cpp
void modern_reference_practices() {
    // === 1. 优先使用const引用传递参数 ===
    auto process_data = [](const vector<int>& data) {
        // 避免拷贝大对象，同时保证不被修改
        for (const auto& item : data) {
            cout << item << " ";
        }
    };
    
    // === 2. 使用auto&进行范围for循环 ===
    vector<string> strings = {"hello", "world", "cpp"};
    for (const auto& str : strings) {    // 避免字符串拷贝
        cout << str << " ";
    }
    
    for (auto& str : strings) {          // 可修改版本
        str += "!";
    }
    
    // === 3. 结构化绑定与引用（C++17） ===
    map<string, int> score_map = {{"Alice", 95}, {"Bob", 87}};
    for (const auto& [name, score] : score_map) {
        cout << name << ": " << score << endl;
    }
    
    // === 4. 完美转发中的万能引用 ===
    template<typename T>
    void perfect_forward(T&& arg) {
        // T&&是万能引用，不是右值引用
        some_function(std::forward<T>(arg));
    }
    
    // === 5. 引用包装器（std::reference_wrapper） ===
    #include <functional>
    vector<reference_wrapper<int>> ref_vector;
    int a = 1, b = 2, c = 3;
    ref_vector.push_back(ref(a));
    ref_vector.push_back(ref(b));
    ref_vector.push_back(ref(c));
    
    for (auto& ref_wrap : ref_vector) {
        ref_wrap.get() *= 2;  // 修改原变量
    }
    cout << "a=" << a << ", b=" << b << ", c=" << c << endl;  // 输出: a=2, b=4, c=6
}
```

#### 引用 vs 指针
```cpp
void demo_reference_vs_pointer() {
    int a = 10, b = 20;
    
    // 指针可以重新指向，引用不能
    int* ptr = &a;
    int& ref = a;
    
    ptr = &b;  // 合法：指针可以重新指向
    // ref = b;   // 错误：引用不能重新引用其他变量
    
    // 指针可以为空，引用不能
    int* null_ptr = nullptr;  // 合法
    // int& null_ref;           // 错误：引用必须初始化
}
```

#### 函数参数传递
```cpp
// 值传递
void func1(int x) {
    x = 100;  // 不会影响原变量
}

// 引用传递
void func2(int& x) {
    x = 100;  // 会影响原变量
}

// 指针传递
void func3(int* x) {
    *x = 100;  // 会影响原变量
}

// 常量引用（只读）
void func4(const int& x) {
    // x = 100;  // 错误：不能修改
    cout << x << endl;  // 只能读取
}
```

### 3. 函数重载

#### 基本函数重载
```cpp
#include <iostream>
using namespace std;

// 根据参数个数重载
void print() {
    cout << "无参数版本" << endl;
}

void print(int x) {
    cout << "整数版本: " << x << endl;
}

void print(double x) {
    cout << "浮点数版本: " << x << endl;
}

void print(int x, int y) {
    cout << "两个整数版本: " << x << ", " << y << endl;
}

// 根据参数类型重载
void process(int x) {
    cout << "处理整数: " << x << endl;
}

void process(string str) {
    cout << "处理字符串: " << str << endl;
}
```

#### 重载规则
```cpp
// 合法的重载
void func(int x);
void func(double x);
void func(int x, int y);
void func(const int& x);

// 不合法的重载（仅返回类型不同）
// int func(int x);      // 错误：与第一个冲突
// double func(int x);   // 错误：与第一个冲突
```

### 4. 默认参数

#### 默认参数的使用
```cpp
// 声明时指定默认参数
void greet(string name, string greeting = "Hello", int times = 1);

// 实现
void greet(string name, string greeting, int times) {
    for (int i = 0; i < times; i++) {
        cout << greeting << ", " << name << "!" << endl;
    }
}

int main() {
    greet("Alice");                    // 使用默认参数
    greet("Bob", "Hi");               // 部分使用默认参数
    greet("Charlie", "Hey", 3);       // 不使用默认参数
    return 0;
}
```

#### 默认参数规则
```cpp
// 正确：默认参数必须从右向左连续
void func1(int a, int b = 2, int c = 3);

// 错误：默认参数不连续
// void func2(int a, int b = 2, int c);

// 错误：默认参数在左边
// void func3(int a = 1, int b);
```

### 5. 内联函数

#### inline关键字
```cpp
// 内联函数定义
inline int max(int a, int b) {
    return (a > b) ? a : b;
}

// 类中的函数默认是内联的
class Calculator {
public:
    inline int add(int a, int b) {  // explicit inline
        return a + b;
    }
    
    int multiply(int a, int b) {    // implicit inline
        return a * b;
    }
};
```

### 6. 命名空间

#### 命名空间的定义和使用
```cpp
// 定义命名空间
namespace Math {
    const double PI = 3.14159;
    
    double area(double radius) {
        return PI * radius * radius;
    }
    
    namespace Advanced {
        double log(double x) {
            // 高级数学函数
            return 0.0;  // 简化实现
        }
    }
}

namespace Physics {
    const double PI = 3.14159265359;  // 更精确的PI
    
    double force(double mass, double acceleration) {
        return mass * acceleration;
    }
}

int main() {
    // 使用完整的命名空间路径
    cout << Math::PI << endl;
    cout << Math::area(5.0) << endl;
    cout << Math::Advanced::log(2.0) << endl;
    
    // 使用using声明
    using Math::PI;
    cout << PI << endl;
    
    // 使用using指令
    using namespace Physics;
    cout << force(10.0, 9.8) << endl;
    
    return 0;
}
```

---

## 面向对象编程

### 1. 类和对象

#### 类的基本定义
```cpp
class Student {
private:
    string name;
    int age;
    double gpa;

public:
    // 构造函数
    Student() {
        name = "未知";
        age = 0;
        gpa = 0.0;
    }
    
    Student(string n, int a, double g) {
        name = n;
        age = a;
        gpa = g;
    }
    
    // 析构函数
    ~Student() {
        cout << "学生 " << name << " 对象被销毁" << endl;
    }
    
    // 成员函数
    void display() {
        cout << "姓名: " << name << ", 年龄: " << age << ", GPA: " << gpa << endl;
    }
    
    // Getter和Setter
    string getName() const { return name; }
    void setName(const string& n) { name = n; }
    
    int getAge() const { return age; }
    void setAge(int a) { 
        if (a >= 0) age = a; 
    }
};
```

#### 对象的创建和使用
```cpp
int main() {
    // 创建对象
    Student s1;                          // 默认构造函数
    Student s2("张三", 20, 3.8);         // 参数化构造函数
    Student* s3 = new Student("李四", 21, 3.9);  // 动态分配
    
    // 使用对象
    s1.display();
    s2.setAge(21);
    s3->display();
    
    // 释放动态分配的内存
    delete s3;
    
    return 0;
}
```

### 2. 构造函数和析构函数

#### 构造函数的类型
```cpp
class Rectangle {
private:
    double width, height;

public:
    // 默认构造函数
    Rectangle() : width(0), height(0) {
        cout << "默认构造函数调用" << endl;
    }
    
    // 参数化构造函数
    Rectangle(double w, double h) : width(w), height(h) {
        cout << "参数化构造函数调用" << endl;
    }
    
    // 拷贝构造函数
    Rectangle(const Rectangle& other) : width(other.width), height(other.height) {
        cout << "拷贝构造函数调用" << endl;
    }
    
    // 赋值运算符重载
    Rectangle& operator=(const Rectangle& other) {
        if (this != &other) {
            width = other.width;
            height = other.height;
        }
        cout << "赋值运算符调用" << endl;
        return *this;
    }
    
    // 析构函数
    ~Rectangle() {
        cout << "析构函数调用" << endl;
    }
    
    double area() const { return width * height; }
};
```

#### 初始化列表
```cpp
class Point {
private:
    const int x;  // 常量成员
    int& y_ref;   // 引用成员
    int y;

public:
    // 必须使用初始化列表初始化const成员和引用成员
    Point(int x_val, int y_val) : x(x_val), y(y_val), y_ref(y) {
        // 构造函数体
    }
    
    // 效率更高的初始化方式
    Point(int x_val, int y_val, string name) : 
        x(x_val), y(y_val), y_ref(y) {
        // 直接初始化，避免先默认构造再赋值
    }
};
```

### 3. 继承

#### 基本继承
```cpp
// 基类
class Animal {
protected:
    string name;
    int age;

public:
    Animal(string n, int a) : name(n), age(a) {}
    
    virtual void makeSound() {
        cout << name << " 发出声音" << endl;
    }
    
    virtual void display() {
        cout << "动物: " << name << ", 年龄: " << age << endl;
    }
    
    virtual ~Animal() {}  // 虚析构函数
};

// 派生类
class Dog : public Animal {
private:
    string breed;

public:
    Dog(string n, int a, string b) : Animal(n, a), breed(b) {}
    
    // 重写基类方法
    void makeSound() override {
        cout << name << " 汪汪叫" << endl;
    }
    
    void display() override {
        Animal::display();  // 调用基类方法
        cout << "品种: " << breed << endl;
    }
    
    // 派生类特有方法
    void wagTail() {
        cout << name << " 摇尾巴" << endl;
    }
};
```

#### 继承的访问控制
```cpp
class Base {
private:
    int private_var;     // 只有基类可以访问

protected:
    int protected_var;   // 基类和派生类可以访问

public:
    int public_var;      // 所有地方都可以访问
};

class Derived : public Base {
public:
    void test() {
        // private_var = 1;     // 错误：不能访问private成员
        protected_var = 2;      // 正确：可以访问protected成员
        public_var = 3;         // 正确：可以访问public成员
    }
};
```

### 4. 多态

#### 虚函数和动态绑定
```cpp
#include <vector>
#include <memory>

class Shape {
public:
    virtual double area() const = 0;  // 纯虚函数
    virtual void draw() const = 0;    // 纯虚函数
    virtual ~Shape() {}               // 虚析构函数
};

class Circle : public Shape {
private:
    double radius;

public:
    Circle(double r) : radius(r) {}
    
    double area() const override {
        return 3.14159 * radius * radius;
    }
    
    void draw() const override {
        cout << "绘制圆形，半径: " << radius << endl;
    }
};

class Rectangle : public Shape {
private:
    double width, height;

public:
    Rectangle(double w, double h) : width(w), height(h) {}
    
    double area() const override {
        return width * height;
    }
    
    void draw() const override {
        cout << "绘制矩形，宽: " << width << ", 高: " << height << endl;
    }
};

// 多态的使用
void polymorphism_demo() {
    vector<unique_ptr<Shape>> shapes;
    shapes.push_back(make_unique<Circle>(5.0));
    shapes.push_back(make_unique<Rectangle>(4.0, 6.0));
    
    for (const auto& shape : shapes) {
        shape->draw();                    // 动态绑定
        cout << "面积: " << shape->area() << endl;
    }
}
```

#### 运算符重载
```cpp
class Complex {
private:
    double real, imag;

public:
    Complex(double r = 0, double i = 0) : real(r), imag(i) {}
    
    // 二元运算符重载
    Complex operator+(const Complex& other) const {
        return Complex(real + other.real, imag + other.imag);
    }
    
    Complex operator-(const Complex& other) const {
        return Complex(real - other.real, imag - other.imag);
    }
    
    Complex operator*(const Complex& other) const {
        return Complex(
            real * other.real - imag * other.imag,
            real * other.imag + imag * other.real
        );
    }
    
    // 赋值运算符重载
    Complex& operator=(const Complex& other) {
        if (this != &other) {
            real = other.real;
            imag = other.imag;
        }
        return *this;
    }
    
    // 复合赋值运算符
    Complex& operator+=(const Complex& other) {
        real += other.real;
        imag += other.imag;
        return *this;
    }
    
    // 比较运算符
    bool operator==(const Complex& other) const {
        return real == other.real && imag == other.imag;
    }
    
    // 输出流运算符（友元函数）
    friend ostream& operator<<(ostream& os, const Complex& c) {
        os << c.real;
        if (c.imag >= 0) os << "+";
        os << c.imag << "i";
        return os;
    }
    
    // 输入流运算符
    friend istream& operator>>(istream& is, Complex& c) {
        is >> c.real >> c.imag;
        return is;
    }
};
```

---

## 模板编程

### 1. 函数模板

#### 基本函数模板
```cpp
// 简单函数模板
template <typename T>
T max(T a, T b) {
    return (a > b) ? a : b;
}

// 多个模板参数
template <typename T, typename U>
auto add(T a, U b) -> decltype(a + b) {
    return a + b;
}

// 模板特化
template <>
const char* max<const char*>(const char* a, const char* b) {
    return (strcmp(a, b) > 0) ? a : b;
}

void template_function_demo() {
    cout << max(10, 20) << endl;           // int版本
    cout << max(3.14, 2.71) << endl;       // double版本
    cout << max('a', 'z') << endl;         // char版本
    
    cout << add(10, 3.14) << endl;         // 混合类型
}
```

### 2. 类模板

#### 基本类模板
```cpp
template <typename T>
class Array {
private:
    T* data;
    size_t size;
    size_t capacity;

public:
    // 构造函数
    Array(size_t cap = 10) : size(0), capacity(cap) {
        data = new T[capacity];
    }
    
    // 拷贝构造函数
    Array(const Array& other) : size(other.size), capacity(other.capacity) {
        data = new T[capacity];
        for (size_t i = 0; i < size; i++) {
            data[i] = other.data[i];
        }
    }
    
    // 析构函数
    ~Array() {
        delete[] data;
    }
    
    // 赋值运算符
    Array& operator=(const Array& other) {
        if (this != &other) {
            delete[] data;
            size = other.size;
            capacity = other.capacity;
            data = new T[capacity];
            for (size_t i = 0; i < size; i++) {
                data[i] = other.data[i];
            }
        }
        return *this;
    }
    
    // 添加元素
    void push_back(const T& value) {
        if (size >= capacity) {
            resize();
        }
        data[size++] = value;
    }
    
    // 访问元素
    T& operator[](size_t index) {
        if (index >= size) {
            throw out_of_range("索引超出范围");
        }
        return data[index];
    }
    
    const T& operator[](size_t index) const {
        if (index >= size) {
            throw out_of_range("索引超出范围");
        }
        return data[index];
    }
    
    size_t getSize() const { return size; }
    bool empty() const { return size == 0; }

private:
    void resize() {
        capacity *= 2;
        T* new_data = new T[capacity];
        for (size_t i = 0; i < size; i++) {
            new_data[i] = data[i];
        }
        delete[] data;
        data = new_data;
    }
};
```

#### 模板特化
```cpp
// 类模板的完全特化
template <>
class Array<bool> {
private:
    unsigned char* data;
    size_t size;
    size_t capacity;

public:
    Array(size_t cap = 10) : size(0), capacity((cap + 7) / 8) {
        data = new unsigned char[capacity]();
    }
    
    ~Array() {
        delete[] data;
    }
    
    void push_back(bool value) {
        if (size >= capacity * 8) {
            resize();
        }
        size_t byte_index = size / 8;
        size_t bit_index = size % 8;
        
        if (value) {
            data[byte_index] |= (1 << bit_index);
        } else {
            data[byte_index] &= ~(1 << bit_index);
        }
        size++;
    }
    
    bool operator[](size_t index) const {
        size_t byte_index = index / 8;
        size_t bit_index = index % 8;
        return (data[byte_index] & (1 << bit_index)) != 0;
    }
    
    size_t getSize() const { return size; }

private:
    void resize() {
        size_t old_capacity = capacity;
        capacity *= 2;
        unsigned char* new_data = new unsigned char[capacity]();
        for (size_t i = 0; i < old_capacity; i++) {
            new_data[i] = data[i];
        }
        delete[] data;
        data = new_data;
    }
};
```

### 3. 可变参数模板（C++11）

#### 基本可变参数模板
```cpp
// 递归终止条件
template <typename T>
void print(T&& t) {
    cout << t << endl;
}

// 可变参数模板
template <typename T, typename... Args>
void print(T&& t, Args&&... args) {
    cout << t << " ";
    print(args...);  // 递归调用
}

// 计算参数个数
template <typename... Args>
constexpr size_t count_args(Args&&... args) {
    return sizeof...(args);
}

void variadic_template_demo() {
    print(1, 2.5, "hello", 'c');           // 输出: 1 2.5 hello c
    cout << count_args(1, 2, 3, 4, 5) << endl;  // 输出: 5
}
```

---

## STL标准模板库

### 1. 容器

#### 序列容器
```cpp
#include <vector>
#include <list>
#include <deque>
#include <array>

void sequence_containers_demo() {
    // vector - 动态数组
    vector<int> vec = {1, 2, 3, 4, 5};
    vec.push_back(6);
    vec.insert(vec.begin() + 2, 10);  // 在位置2插入10
    
    // list - 双向链表
    list<string> lst = {"apple", "banana", "cherry"};
    lst.push_front("apricot");
    lst.sort();
    
    // deque - 双端队列
    deque<double> dq;
    dq.push_back(1.1);
    dq.push_front(2.2);
    
    // array - 固定大小数组（C++11）
    array<int, 5> arr = {1, 2, 3, 4, 5};
}
```

#### 关联容器
```cpp
#include <set>
#include <map>
#include <unordered_set>
#include <unordered_map>

void associative_containers_demo() {
    // set - 有序集合
    set<int> s = {3, 1, 4, 1, 5, 9};  // 自动排序，去重
    s.insert(2);
    
    // map - 有序映射
    map<string, int> m;
    m["apple"] = 5;
    m["banana"] = 3;
    m.insert({"cherry", 8});
    
    // unordered_set - 哈希集合
    unordered_set<string> us = {"red", "green", "blue"};
    us.insert("yellow");
    
    // unordered_map - 哈希映射
    unordered_map<int, string> um;
    um[1] = "one";
    um[2] = "two";
}
```

### 2. 迭代器

#### 迭代器类型和使用
```cpp
#include <iterator>

void iterator_demo() {
    vector<int> vec = {1, 2, 3, 4, 5};
    
    // 正向迭代器
    for (auto it = vec.begin(); it != vec.end(); ++it) {
        cout << *it << " ";
    }
    cout << endl;
    
    // 反向迭代器
    for (auto it = vec.rbegin(); it != vec.rend(); ++it) {
        cout << *it << " ";
    }
    cout << endl;
    
    // 常量迭代器
    for (auto it = vec.cbegin(); it != vec.cend(); ++it) {
        cout << *it << " ";
        // *it = 10;  // 错误：不能修改
    }
    cout << endl;
    
    // 范围for循环（C++11）
    for (const auto& element : vec) {
        cout << element << " ";
    }
    cout << endl;
    
    // 使用迭代器修改元素
    for (auto it = vec.begin(); it != vec.end(); ++it) {
        *it *= 2;
    }
}
```

### 3. 算法

#### 常用算法
```cpp
#include <algorithm>
#include <numeric>
#include <functional>

void algorithm_demo() {
    vector<int> vec = {3, 1, 4, 1, 5, 9, 2, 6, 5, 3};
    
    // 排序算法
    sort(vec.begin(), vec.end());                    // 升序排序
    sort(vec.begin(), vec.end(), greater<int>());    // 降序排序
    
    // 查找算法
    auto it = find(vec.begin(), vec.end(), 5);
    if (it != vec.end()) {
        cout << "找到元素5在位置: " << distance(vec.begin(), it) << endl;
    }
    
    // 二分查找（需要已排序）
    bool found = binary_search(vec.begin(), vec.end(), 4);
    
    // 计数
    int count = std::count(vec.begin(), vec.end(), 5);
    cout << "元素5出现次数: " << count << endl;
    
    // 变换算法
    vector<int> squared(vec.size());
    transform(vec.begin(), vec.end(), squared.begin(), 
              [](int x) { return x * x; });
    
    // 累积算法
    int sum = accumulate(vec.begin(), vec.end(), 0);
    int product = accumulate(vec.begin(), vec.end(), 1, multiplies<int>());
    
    // 去重（需要先排序）
    sort(vec.begin(), vec.end());
    auto new_end = unique(vec.begin(), vec.end());
    vec.erase(new_end, vec.end());
    
    // 分区
    partition(vec.begin(), vec.end(), [](int x) { return x % 2 == 0; });
}
```

---

## 内存管理

### 1. 动态内存分配

#### new和delete
```cpp
void dynamic_memory_demo() {
    // 分配单个对象
    int* ptr = new int(42);
    cout << *ptr << endl;
    delete ptr;
    ptr = nullptr;  // 避免悬空指针
    
    // 分配数组
    int* arr = new int[10];
    for (int i = 0; i < 10; i++) {
        arr[i] = i * i;
    }
    delete[] arr;
    arr = nullptr;
    
    // 分配对象
    Student* student = new Student("张三", 20, 3.8);
    student->display();
    delete student;
    student = nullptr;
}
```

### 2. 智能指针（C++11）

#### unique_ptr
```cpp
#include <memory>

void unique_ptr_demo() {
    // 创建unique_ptr
    unique_ptr<int> ptr1 = make_unique<int>(42);
    unique_ptr<int[]> arr = make_unique<int[]>(10);
    
    // 访问数据
    cout << *ptr1 << endl;
    
    // 移动语义
    unique_ptr<int> ptr2 = move(ptr1);  // ptr1变为nullptr
    
    // 释放所有权
    int* raw_ptr = ptr2.release();
    delete raw_ptr;  // 需要手动删除
    
    // 重置
    ptr2.reset(new int(100));
    
    // 自定义删除器
    unique_ptr<FILE, decltype(&fclose)> file(fopen("test.txt", "w"), &fclose);
}
```

#### shared_ptr
```cpp
void shared_ptr_demo() {
    // 创建shared_ptr
    shared_ptr<int> ptr1 = make_shared<int>(42);
    cout << "引用计数: " << ptr1.use_count() << endl;  // 1
    
    {
        shared_ptr<int> ptr2 = ptr1;  // 共享所有权
        cout << "引用计数: " << ptr1.use_count() << endl;  // 2
    }  // ptr2析构，引用计数减1
    
    cout << "引用计数: " << ptr1.use_count() << endl;  // 1
    
    // 循环引用问题
    struct Node {
        shared_ptr<Node> next;
        weak_ptr<Node> parent;  // 使用weak_ptr避免循环引用
        int data;
        
        Node(int d) : data(d) {}
        ~Node() { cout << "Node " << data << " 被析构" << endl; }
    };
    
    shared_ptr<Node> node1 = make_shared<Node>(1);
    shared_ptr<Node> node2 = make_shared<Node>(2);
    
    node1->next = node2;
    node2->parent = node1;  // 不会造成循环引用
}
```

#### weak_ptr
```cpp
void weak_ptr_demo() {
    shared_ptr<int> shared = make_shared<int>(42);
    weak_ptr<int> weak = shared;
    
    cout << "weak_ptr过期了吗? " << weak.expired() << endl;  // false
    
    if (auto locked = weak.lock()) {
        cout << "通过weak_ptr访问: " << *locked << endl;
    }
    
    shared.reset();  // 释放shared_ptr
    cout << "weak_ptr过期了吗? " << weak.expired() << endl;  // true
}
```

---

## 异常处理

### 1. 基本异常处理

#### try-catch-throw
```cpp
#include <stdexcept>

double divide(double a, double b) {
    if (b == 0) {
        throw invalid_argument("除数不能为零");
    }
    return a / b;
}

void exception_demo() {
    try {
        double result = divide(10.0, 0.0);
        cout << "结果: " << result << endl;
    }
    catch (const invalid_argument& e) {
        cout << "捕获异常: " << e.what() << endl;
    }
    catch (const exception& e) {
        cout << "捕获通用异常: " << e.what() << endl;
    }
    catch (...) {
        cout << "捕获未知异常" << endl;
    }
}
```

### 2. 自定义异常

#### 自定义异常类
```cpp
class MathException : public exception {
private:
    string message;

public:
    MathException(const string& msg) : message(msg) {}
    
    const char* what() const noexcept override {
        return message.c_str();
    }
};

class DivisionByZeroException : public MathException {
public:
    DivisionByZeroException() : MathException("除零错误") {}
};

class NegativeSquareRootException : public MathException {
public:
    NegativeSquareRootException() : MathException("负数开平方根错误") {}
};

double safe_sqrt(double x) {
    if (x < 0) {
        throw NegativeSquareRootException();
    }
    return sqrt(x);
}

void custom_exception_demo() {
    try {
        double result = safe_sqrt(-4.0);
        cout << "平方根: " << result << endl;
    }
    catch (const MathException& e) {
        cout << "数学异常: " << e.what() << endl;
    }
}
```

### 3. RAII原则

#### 资源获取即初始化
```cpp
class FileHandler {
private:
    FILE* file;

public:
    FileHandler(const string& filename, const string& mode) {
        file = fopen(filename.c_str(), mode.c_str());
        if (!file) {
            throw runtime_error("无法打开文件: " + filename);
        }
    }
    
    ~FileHandler() {
        if (file) {
            fclose(file);
        }
    }
    
    // 禁止拷贝
    FileHandler(const FileHandler&) = delete;
    FileHandler& operator=(const FileHandler&) = delete;
    
    // 允许移动
    FileHandler(FileHandler&& other) noexcept : file(other.file) {
        other.file = nullptr;
    }
    
    FileHandler& operator=(FileHandler&& other) noexcept {
        if (this != &other) {
            if (file) fclose(file);
            file = other.file;
            other.file = nullptr;
        }
        return *this;
    }
    
    void write(const string& data) {
        if (fputs(data.c_str(), file) == EOF) {
            throw runtime_error("写入文件失败");
        }
    }
    
    string read() {
        string result;
        char buffer[1024];
        while (fgets(buffer, sizeof(buffer), file)) {
            result += buffer;
        }
        return result;
    }
};

void raii_demo() {
    try {
        FileHandler file("test.txt", "w");
        file.write("Hello, RAII!\n");
        // 文件会在FileHandler析构时自动关闭
    }
    catch (const exception& e) {
        cout << "文件操作异常: " << e.what() << endl;
    }
}
```

---

## 文件操作

### 1. 文件流

#### 基本文件操作
```cpp
#include <fstream>
#include <sstream>

void file_stream_demo() {
    // 写入文件
    ofstream outfile("data.txt");
    if (outfile.is_open()) {
        outfile << "第一行数据" << endl;
        outfile << "第二行数据" << endl;
        outfile << "数字: " << 42 << ", 浮点数: " << 3.14 << endl;
        outfile.close();
    }
    
    // 读取文件
    ifstream infile("data.txt");
    if (infile.is_open()) {
        string line;
        while (getline(infile, line)) {
            cout << "读取到: " << line << endl;
        }
        infile.close();
    }
    
    // 追加模式
    ofstream appendfile("data.txt", ios::app);
    if (appendfile.is_open()) {
        appendfile << "追加的数据" << endl;
        appendfile.close();
    }
    
    // 二进制文件操作
    struct Person {
        char name[50];
        int age;
        double salary;
    };
    
    Person p1 = {"张三", 25, 5000.0};
    
    // 写入二进制文件
    ofstream binout("person.bin", ios::binary);
    if (binout.is_open()) {
        binout.write(reinterpret_cast<char*>(&p1), sizeof(Person));
        binout.close();
    }
    
    // 读取二进制文件
    Person p2;
    ifstream binin("person.bin", ios::binary);
    if (binin.is_open()) {
        binin.read(reinterpret_cast<char*>(&p2), sizeof(Person));
        cout << "姓名: " << p2.name << ", 年龄: " << p2.age << ", 薪水: " << p2.salary << endl;
        binin.close();
    }
}
```

### 2. 字符串流

#### stringstream的使用
```cpp
void string_stream_demo() {
    // 字符串到数值的转换
    string data = "123 45.67 hello world";
    istringstream iss(data);
    
    int i;
    double d;
    string s1, s2;
    
    iss >> i >> d >> s1 >> s2;
    cout << "整数: " << i << ", 浮点数: " << d << endl;
    cout << "字符串: " << s1 << " " << s2 << endl;
    
    // 数值到字符串的转换
    ostringstream oss;
    oss << "结果: " << i * 2 << ", " << d * 2;
    string result = oss.str();
    cout << result << endl;
    
    // 格式化字符串
    stringstream ss;
    ss << "ID: " << setw(5) << setfill('0') << 42;
    ss << ", Score: " << fixed << setprecision(2) << 95.678;
    cout << ss.str() << endl;  // ID: 00042, Score: 95.68
}
```

---

## 高级特性

### 1. Lambda表达式（C++11）

#### Lambda基本语法
```cpp
void lambda_demo() {
    // 基本lambda表达式
    auto add = [](int a, int b) { return a + b; };
    cout << add(3, 4) << endl;  // 7
    
    // 捕获外部变量
    int x = 10, y = 20;
    
    auto lambda1 = [x, y]() { return x + y; };          // 按值捕获
    auto lambda2 = [&x, &y]() { return x + y; };        // 按引用捕获
    auto lambda3 = [=]() { return x + y; };             // 按值捕获所有
    auto lambda4 = [&]() { return x + y; };             // 按引用捕获所有
    auto lambda5 = [x, &y]() { return x + y; };         // 混合捕获
    
    // 修改捕获的变量（mutable）
    auto lambda6 = [x](int z) mutable { x += z; return x; };
    
    // 指定返回类型
    auto lambda7 = [](double a, double b) -> int { return static_cast<int>(a + b); };
    
    // 在STL算法中使用lambda
    vector<int> vec = {1, 2, 3, 4, 5};
    
    // 查找偶数
    auto it = find_if(vec.begin(), vec.end(), [](int n) { return n % 2 == 0; });
    
    // 变换元素
    transform(vec.begin(), vec.end(), vec.begin(), [](int n) { return n * n; });
    
    // 排序（自定义比较）
    sort(vec.begin(), vec.end(), [](int a, int b) { return a > b; });
}
```

### 2. 移动语义（C++11）

#### 右值引用和移动构造
```cpp
class BigObject {
private:
    int* data;
    size_t size;

public:
    // 构造函数
    BigObject(size_t s) : size(s) {
        data = new int[size];
        cout << "构造BigObject，大小: " << size << endl;
    }
    
    // 拷贝构造函数
    BigObject(const BigObject& other) : size(other.size) {
        data = new int[size];
        copy(other.data, other.data + size, data);
        cout << "拷贝构造BigObject" << endl;
    }
    
    // 移动构造函数
    BigObject(BigObject&& other) noexcept : data(other.data), size(other.size) {
        other.data = nullptr;
        other.size = 0;
        cout << "移动构造BigObject" << endl;
    }
    
    // 拷贝赋值运算符
    BigObject& operator=(const BigObject& other) {
        if (this != &other) {
            delete[] data;
            size = other.size;
            data = new int[size];
            copy(other.data, other.data + size, data);
            cout << "拷贝赋值BigObject" << endl;
        }
        return *this;
    }
    
    // 移动赋值运算符
    BigObject& operator=(BigObject&& other) noexcept {
        if (this != &other) {
            delete[] data;
            data = other.data;
            size = other.size;
            other.data = nullptr;
            other.size = 0;
            cout << "移动赋值BigObject" << endl;
        }
        return *this;
    }
    
    // 析构函数
    ~BigObject() {
        delete[] data;
        if (size > 0) {
            cout << "析构BigObject" << endl;
        }
    }
    
    size_t getSize() const { return size; }
};

BigObject createBigObject(size_t size) {
    return BigObject(size);  // 返回临时对象，会触发移动
}

void move_semantics_demo() {
    BigObject obj1(1000);                    // 构造
    BigObject obj2 = obj1;                   // 拷贝构造
    BigObject obj3 = move(obj1);             // 移动构造
    BigObject obj4 = createBigObject(2000);  // 移动构造（返回值优化）
    
    obj2 = obj3;                             // 拷贝赋值
    obj2 = move(obj4);                       // 移动赋值
}
```

### 3. 完美转发

#### forward和万能引用
```cpp
template <typename T>
void wrapper(T&& arg) {
    // 完美转发：保持参数的值类别
    process(forward<T>(arg));
}

void process(const string& s) {
    cout << "处理左值: " << s << endl;
}

void process(string&& s) {
    cout << "处理右值: " << s << endl;
}

void perfect_forwarding_demo() {
    string s = "hello";
    wrapper(s);              // 传递左值
    wrapper(string("world")); // 传递右值
}
```

### 4. 类型推导（C++11/14/17）

#### auto和decltype
```cpp
void type_deduction_demo() {
    // auto类型推导
    auto x = 42;              // int
    auto y = 3.14;            // double
    auto z = "hello";         // const char*
    auto w = string("world"); // string
    
    vector<int> vec = {1, 2, 3};
    auto it = vec.begin();    // vector<int>::iterator
    
    // decltype类型推导
    int a = 10;
    decltype(a) b = 20;       // b是int类型
    
    vector<int> v;
    decltype(v.size()) size = v.size();  // size_t类型
    
    // C++14的返回类型推导
    auto add = [](auto a, auto b) { return a + b; };
    
    // C++17的结构化绑定
    map<string, int> m = {{"apple", 5}, {"banana", 3}};
    for (const auto& [key, value] : m) {
        cout << key << ": " << value << endl;
    }
}
```

### 5. 范围for循环增强

#### 结构化绑定和范围for
```cpp
void range_for_demo() {
    vector<pair<string, int>> data = {
        {"apple", 5}, {"banana", 3}, {"cherry", 8}
    };
    
    // 传统方式
    for (auto it = data.begin(); it != data.end(); ++it) {
        cout << it->first << ": " << it->second << endl;
    }
    
    // 范围for循环
    for (const auto& item : data) {
        cout << item.first << ": " << item.second << endl;
    }
    
    // C++17结构化绑定
    for (const auto& [name, count] : data) {
        cout << name << ": " << count << endl;
    }
    
    // 修改元素
    for (auto& [name, count] : data) {
        count *= 2;  // 将所有计数翻倍
    }
}
```

---

## 编程实践建议

### 1. 编码规范

#### 命名约定
```cpp
// 类名：PascalCase
class StudentManager {
private:
    // 私有成员：下划线前缀
    vector<Student> students_;
    int total_count_;

public:
    // 函数名：camelCase或snake_case
    void addStudent(const Student& student);
    void remove_student(int id);
    
    // 常量：大写+下划线
    static const int MAX_STUDENTS = 1000;
    
    // 获取器：get前缀
    int getTotalCount() const { return total_count_; }
    
    // 设置器：set前缀
    void setMaxCapacity(int capacity);
};

// 枚举：大写
enum class Color { RED, GREEN, BLUE };

// 宏：大写+下划线
#define MAX_BUFFER_SIZE 1024
```

#### 代码组织
```cpp
// 头文件保护
#ifndef STUDENT_H
#define STUDENT_H

// 系统头文件
#include <iostream>
#include <vector>
#include <string>

// 项目头文件
#include "Person.h"

// 使用命名空间（在实现文件中，不在头文件中）
using namespace std;

class Student : public Person {
    // 类定义
};

#endif // STUDENT_H
```

### 2. 性能优化技巧

#### 常见优化策略
```cpp
// 1. 使用const引用避免不必要的拷贝
void processStudent(const Student& student) {  // 好
    // 处理学生信息
}

void processStudent(Student student) {  // 差：会拷贝整个对象
    // 处理学生信息
}

// 2. 使用移动语义
vector<Student> students;
Student newStudent("张三", 20, 3.8);
students.push_back(move(newStudent));  // 移动而不是拷贝

// 3. 使用emplace避免临时对象
students.emplace_back("李四", 21, 3.9);  // 直接构造

// 4. 预分配容器大小
vector<int> data;
data.reserve(1000);  // 预分配空间，避免重复扩容

// 5. 使用智能指针管理资源
unique_ptr<Student> student = make_unique<Student>("王五", 22, 3.7);

// 6. 避免在循环中进行昂贵操作
vector<string> names;
string suffix = ".txt";  // 在循环外定义
for (int i = 0; i < 1000; i++) {
    names.push_back("file" + to_string(i) + suffix);
}
```

### 3. 调试技巧

#### 断言和调试宏
```cpp
#include <cassert>

#ifdef DEBUG
    #define DBG(x) cout << #x << " = " << x << endl
    #define DBG_FUNC() cout << "调用函数: " << __FUNCTION__ << endl
#else
    #define DBG(x)
    #define DBG_FUNC()
#endif

void debug_demo() {
    int x = 42;
    DBG(x);  // 调试模式下输出: x = 42
    
    DBG_FUNC();  // 调试模式下输出: 调用函数: debug_demo
    
    // 断言检查
    assert(x > 0);  // 如果x <= 0，程序会终止
    
    // 条件编译
    #ifdef FEATURE_ENABLED
        // 只有定义了FEATURE_ENABLED才会编译这部分代码
        cout << "特性已启用" << endl;
    #endif
}
```

---

## 学习建议和总结

### 1. 学习路径
1. **第1-2周**：掌握基础语法，重点是输入输出、引用、函数重载
2. **第3-4周**：学习类和对象，理解封装概念
3. **第5-6周**：学习继承和多态，掌握虚函数
4. **第7-8周**：学习模板编程，理解泛型编程思想
5. **第9-10周**：熟悉STL容器和算法
6. **第11-12周**：学习内存管理和智能指针
7. **第13-14周**：掌握异常处理和文件操作
8. **第15-16周**：学习C++11/14/17新特性

### 2. 实践项目
- **学生管理系统**：练习类设计和STL使用
- **文本处理工具**：练习字符串操作和文件I/O
- **简单游戏**：练习面向对象设计
- **数据结构库**：练习模板编程

### 3. 常见错误避免
- **内存泄漏**：使用智能指针，遵循RAII原则
- **悬空指针**：删除指针后设为nullptr
- **数组越界**：使用vector替代原始数组
- **拷贝开销**：合理使用引用和移动语义
- **异常安全**：编写异常安全的代码

记住：C++是一门复杂但功能强大的语言，需要大量实践才能熟练掌握。从简单项目开始，逐步学习高级特性，多读优秀的开源代码，多实践多思考。