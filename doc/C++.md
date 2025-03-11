# C++ 中结构体和类的区别
在C++中，`struct`（结构体）和`class`（类）均可用于定义封装数据和函数的用户自定义类型。尽管两者功能相似，但存在以下关键区别：

---

### 1. **默认访问权限**
- **`struct`**：成员（数据和函数）**默认为public**，可直接从外部访问。
- **`class`**：成员**默认为private**，需通过公有成员函数（如getter/setter）访问私有成员。

```cpp
struct MyStruct {
    int data; // 默认为public
};

class MyClass {
    int data; // 默认为private
public:
    int getData() const { return data; }
    void setData(int val) { data = val; }
};
```

---

### 2. **继承时的默认访问级别**
- **`struct`**：作为基类时，派生类默认以**public方式继承**基类成员。
- **`class`**：作为基类时，派生类默认以**private方式继承**基类成员。

```cpp
struct BaseStruct {
    int baseData;
};

struct DerivedStruct : BaseStruct { 
    // baseData 在此为public
};

class BaseClass {
    int baseData;
};

class DerivedClass : BaseClass { 
    // baseData 在此为private
};
```

---

### 3. **使用惯例**
- **`struct`**：通常用于**纯数据（Plain-Old-Data, POD）**结构，不含复杂行为。
- **`class`**：用于遵循面向对象原则的实体，既封装数据又定义行为（如继承、多态）。

---

### 注意事项
在现代C++中，`struct`和`class`的功能界限已非常模糊。两者均可定义构造函数、析构函数、成员函数和访问修饰符。选择使用哪一种通常取决于以下因素：
- **代码意图**：数据容器（用`struct`） vs 行为封装（用`class`）。
- **项目规范**：遵循团队的编码标准或历史惯例。

---

### 总结
| 特性                | `struct`                  | `class`                   |
|---------------------|---------------------------|---------------------------|
| 默认访问权限         | public                    | private                   |
| 默认继承方式         | public继承                | private继承               |
| 典型用途             | 数据聚合（轻量级）         | 对象封装（复杂逻辑）       |

# 静态局部变量\全局变量\局部变量的区别和使用场景
在 C++ 中，**局部变量**、**全局变量**和**静态变量**在作用域、生命周期和存储位置等方面存在显著差异。以下是对它们的详细比较及使用场景：

### 1. 局部变量（Local Variables）

- **定义**：在函数或代码块内部声明的变量。
- **作用域**：仅在声明它们的函数或代码块内有效。
- **生命周期**：在函数调用时创建，函数返回时销毁。
- **存储位置**：通常存储在栈（stack）中。

**使用场景**：适用于仅在特定函数或代码块内需要使用的数据，确保数据的临时性和局部性，避免与其他部分的代码发生冲突。

**示例**：


```cpp
void exampleFunction() {
    int localVar = 10; // 局部变量
    // 仅在此函数内有效
}
```


### 2. 全局变量（Global Variables）

- **定义**：在所有函数外部（通常在文件顶部）声明的变量。
- **作用域**：在整个源文件中有效，如果使用 `extern` 关键字，在其他源文件中也可访问。
- **生命周期**：程序执行期间始终存在，直到程序结束。
- **存储位置**：存储在数据段（data segment）中。

**使用场景**：适用于需要在多个函数或文件之间共享的数据。然而，过度使用全局变量可能导致代码维护困难，建议谨慎使用。

**示例**：


```cpp
int globalVar = 20; // 全局变量

void functionA() {
    globalVar = 30; // 访问全局变量
}

void functionB() {
    int localVar = globalVar; // 将全局变量值赋给局部变量
}
```


### 3. 静态局部变量（Static Local Variables）

- **定义**：在函数内部使用 `static` 关键字声明的变量。
- **作用域**：仅在声明它们的函数内有效。
- **生命周期**：在程序执行期间始终存在，函数调用结束后其值不会丢失，下次调用该函数时，变量保持上次的值。
- **存储位置**：通常存储在数据段（data segment）中。

**使用场景**：适用于需要在多次函数调用之间保留状态信息的情况，例如计数函数调用次数等。

**示例**：


```cpp
void counterFunction() {
    static int count = 0; // 静态局部变量
    count++;
    std::cout << "Function called " << count << " times." << std::endl;
}
```


**注意**：静态局部变量的初始化仅在第一次调用函数时进行一次，之后的调用将保留上次的值。

**总结**：

- **局部变量**：用于函数内部，生命周期仅限于函数调用期间，适用于临时数据存储。
- **全局变量**：在整个程序中可访问，适用于共享数据，但可能导致命名冲突和维护困难。
- **静态局部变量**：在函数内部，生命周期贯穿整个程序执行过程，适用于需要在多次函数调用之间保留数据的场景。

理解这些变量的特性，有助于在编程中做出合理的设计选择，确保代码的清晰性和可维护性。 

# C++强制类型转换关键字

在 C++ 中，类型转换对于在不同数据类型之间进行转换至关重要。该语言提供了四种主要的强制类型转换运算符来执行显式类型转换：`static_cast`、`dynamic_cast`、`reinterpret_cast` 和 `const_cast`。每种运算符都有特定的用途，应在适当的场景中使用，以确保代码的安全性和清晰度。

### 1. `static_cast`

**用途**：用于在相关类型之间进行定义明确的编译时转换。

**用法**：
- 在数值类型之间进行转换（例如，从 `int` 转换为 `float`）。
- 在继承层次结构中转换指针或引用（向上转换或向下转换），前提是这种关系是已知且安全的。
- 调用显式构造函数或转换运算符。

**示例**：
```cpp
#include <iostream>

class Base {
public:
    virtual ~Base() = default;
};

class Derived : public Base {
public:
    void show() { std::cout << "Derived class" << std::endl; }
};

int main() {
    Base* basePtr = new Derived();
    // 向上转换：从 Derived* 到 Base*
    Derived* derivedPtr = static_cast<Derived*>(basePtr);
    derivedPtr->show(); // 输出：Derived class
    delete basePtr;
    return 0;
}
```

**注意**：要确保在类层次结构中进行的转换是有效的，以避免出现未定义行为。

### 2. `dynamic_cast`

**用途**：在继承层次结构中安全地转换指针或引用，会进行运行时检查以确保转换的有效性。

**用法**：
- 在多态层次结构中进行向下转换（从基类指针/引用转换为派生类指针/引用）。
- 基类中至少需要有一个虚函数，以支持运行时类型识别（RTTI）。

**示例**：
```cpp
#include <iostream>

class Base {
public:
    virtual ~Base() = default; // 虚析构函数启用 RTTI
};

class Derived : public Base {
public:
    void show() { std::cout << "Derived class" << std::endl; }
};

int main() {
    Base* basePtr = new Derived();
    // 使用 dynamic_cast 进行向下转换
    Derived* derivedPtr = dynamic_cast<Derived*>(basePtr);
    if (derivedPtr) {
        derivedPtr->show(); // 输出：Derived class
    } else {
        std::cout << "Invalid cast" << std::endl;
    }
    delete basePtr;
    return 0;
}
```

**注意**：如果转换无效（例如，转换为对象层次结构中不存在的类型），对于指针，`dynamic_cast` 会返回 `nullptr`；对于引用，会抛出 `std::bad_cast` 异常。

### 3. `reinterpret_cast`

**用途**：在不相关的类型之间进行低级别的按位转换。

**用法**：
- 在不同的指针类型之间进行转换，例如从 `int*` 转换为 `char*`。
- 在整数值和指针类型之间进行相互转换。

**示例**：
```cpp
#include <iostream>

int main() {
    int num = 65;
    // 将 num 的地址重新解释为指向 char 的指针
    char* charPtr = reinterpret_cast<char*>(&num);
    std::cout << *charPtr << std::endl; // 输出：A
    return 0;
}
```

**注意事项**：如果使用不当，`reinterpret_cast` 可能会导致未定义行为，因为它允许在不一定兼容的类型之间进行转换。使用时要格外谨慎。

### 4. `const_cast`

**用途**：为变量添加或移除 `const` 限定符。

**用法**：
- 移除 `const` 限定符，以便修改作为常量引用或指针传递的变量。
- 当函数接口需要时，为非 `const` 变量添加 `const` 限定符。

**示例**：
```cpp
#include <iostream>

void modify(int& value) {
    value = 100;
}

int main() {
    const int num = 10;
    // 移除常量性，以便传递给一个会修改该值的函数
    modify(const_cast<int&>(num));
    std::cout << num << std::endl; // 输出：100
    return 0;
}
```

**注意**：使用 `const_cast` 移除 `const` 限定符并修改最初声明为 `const` 的变量会导致未定义行为。要确保对象最初不是常量。

**总结**：
- **`static_cast`**：用于在相关类型之间进行标准的编译时转换。
- **`dynamic_cast`**：用于在多态类层次结构中进行安全的向下转换，并进行运行时检查。
- **`reinterpret_cast`**：用于在不相关的类型之间进行低级别的按位转换；使用时要格外小心。
- **`const_cast`**：用于为指针或引用添加或移除 `const` 限定符。

选择合适的强制类型转换运算符可以确保 C++ 代码的类型安全和清晰度。 

# Difference between heap and stack in C++
在 C++ 中，内存分配主要通过两个区域来管理：**栈（stack）** 和 **堆（heap）**。理解它们之间的区别对于有效的内存管理和程序性能至关重要。

### 栈
- **用途**：栈用于存储局部变量和函数调用信息。
- **分配与释放**：内存分配是自动进行的；当调用一个函数时，其局部变量会被压入栈中，当函数退出时，这些变量会从栈中弹出。
- **大小限制**：栈的大小是有限的，如果超过这个限制，尤其是在深度递归或使用大型局部变量的情况下，可能会导致栈溢出。
- **访问速度**：由于栈采用后进先出（LIFO）结构，并且离 CPU 缓存较近，所以访问栈内存通常更快。

**示例**：
```cpp
void function() {
    int localVar = 10; // 存储在栈上
    // 函数操作
} // localVar 在此处自动销毁
```

### 堆
- **用途**：堆用于动态内存分配，允许在运行时分配内存。
- **分配与释放**：必须使用 `new` 等运算符显式地分配内存，并使用 `delete` 释放内存。如果不释放内存，会导致内存泄漏。
- **大小限制**：堆通常比栈大得多，其大小受系统可用内存的限制，因此适合分配大型数据结构。
- **访问速度**：由于动态内存管理的开销，访问堆内存通常比访问栈内存慢。

**示例**：
```cpp
void function() {
    int* dynamicVar = new int(10); // 分配在堆上
    // 使用 dynamicVar
    delete dynamicVar; // 释放已分配的内存
}
```

**主要区别**：
- **生命周期**：
  - **栈**：变量仅在函数的作用域内存在；当函数退出时，它们会自动销毁。
  - **堆**：变量会一直存在，直到被显式释放，因此可以实现动态的生命周期。
- **大小限制**：
  - **栈**：大小有限；过度使用可能会导致栈溢出。
  - **堆**：容量更大，适合分配大型或可变大小的数据结构。
- **性能**：
  - **栈**：由于其结构化的性质，分配和释放速度更快。
  - **堆**：由于管理动态内存的复杂性，操作速度较慢。

**使用建议**：
- **栈**：适用于在编译时就已知大小的小型、短期使用的变量。
- **堆**：适用于大型、复杂的数据结构，或者在事先无法确定所需内存量的情况。

理解何时以及在何处分配内存对于编写高效、可靠的 C++ 程序至关重要。对于临时的小规模数据使用栈，对于动态的大规模数据使用堆，可以实现最佳的性能和资源管理。 