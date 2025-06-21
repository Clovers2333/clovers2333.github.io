# 模版

## 模版类

- **类模版/Class Template**：是一个模版，用于生成类的定义，编译器会使用它来实例化各种具体的类。
- **模版类/Template Class**：是一个类模版实例化后的**具体类**，当使用类模版定义对象时，需要指定实际的类型参数，从而生成模版类。

定义一个类模版 `Vector`:

```cpp
template <typename T> class Vector {
   public:
    T& at(size_t index);
    void push_back(const T& elem);

   private:
    T* elems;
};
```

- 模版声明：`Vector` 是一个模版，接受类型参数 `T`；
- 模版实例化：当使用对应的实例（T 对应的类型）的时候，编译器会根据指定的具体类型生成相应的代码。
- 理解就是，最开头的 `template <typename>` 把后面需要用到的未知的类型给提前声明了。

```cpp
Vector<int> intVector;
Vector<std::string> stringVector;
```

- 除了 `typename`，模版参数还可以是别的类型，并且在模版中，`typename` 和 `class` 是等价的。

```cpp
template <typename T, std::size_t N> struct std::array;
std::array<int, 5> arr; // An array of exactly 5 integers

template <class T, std::size_t N> struct std::array;      // Equivalent to the above
```

- 声明成员函数的时候需要注意，对于上面类模版 `Vector` 而言，`Vector` 不是一个类型，但是 `Vector<T>` 是一个类型。

```cpp
template <typename T> T& Vector<T>::at(size_t index) {
    return elems[index];
}
```

- 如果我们在传参的时候声明 `const` 会怎么样？

```cpp
template <typename T> class Vector {
   public:
    size_t size();
    T& at(size_t index);
};

void printVec (const Vector<int>& vec) {
    for (size_t i = 0; i < vec.size(); ++i) {
        std::cout << vec.at(i) << std::endl;
    }
}
```

- 如果 `at` 和 `size` 没定义好的话，其实是会报错的：因为传参的 `const` 保证我传进来的参数一定不会被改变，因此，我要保证我这两个函数也不会改变参数：

```cpp
size_t size() const;
T& at(size_t index) const; 
```

- 在调用成员函数时，我们会把成员变量传入 `this` 的指针，而将成员函数定义为 const 的意义则是，保证这一个指向常量的指针，在成员函数中无法修改指针指向的值，也就是 `const Vector<int>* this`.

## 模版函数

```cpp
template <typename T> T min (const T &a, const T &b) {
    return a < b ? a : b;
}

template <typename T, typename U> auto min(const T &a, const U &b) {
    return a < b ? a : b;
}
```

实例化方法对应有两种：可以直接指定类型，也可以让编译器自行推断，但是推断的结果不一定如使用者本意（比如会把字符串字面量推断成 `const char*`）：

```cpp
min<int>(3, 4);                     // 显式实例化：编译器帮我们生成对应代码
min(3.14, 2.71);                    // 隐式实例化：编译器自行推断数据类型
min("hello", "world");              // 隐式实例化：编译器自行推断数据类型
min<const char*>("hello", "world"); // 显式实例化：上一行对应的其实是这个
```

更进一步的例子：

假如我们想要定义一个普适化的容器中的 find 函数：

```cpp
std::vector<int>::iterator find(
	std::vector<int>::iterator begin,
    std::vector<int>::iterator end,
    int value
) // This is too specfic!
```

改进以后的：

```cpp
template <class InputIt, class T> InputIt find(InputIt first, InputIt last, const T &value);
```

## 模板和继承

**模板能够继承自非模板类：**

```cpp
class Base {
    int val;
};

template <typename T> class Derived : public Base<T> {
```

**模板能够继承自模板类：**

```cpp
template <typename T> class Base {
    T val;
};

template <typename T> class Derived : public Base<T> {
```

**非模板类可继承自模板类：**

```cpp
class SupervisorGroup : public
List<Employee*> { ...
```
