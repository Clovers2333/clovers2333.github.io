# 指针相关

## Smart Pointers in Standard Library

在标准库中，有一些原始指针：

- std::unique_ptr：独占所有权，不能被复制
  - 支持移动操作（std::move），允许将所有权从一个 std::unique_ptr 转移到另一个。当所有权转移后，原 std::unique_ptr 不再指向该对象
- std::shared_ptr：共享所有权，可以被复制
  - 它内部通过引用计数(reference count) 机制来跟踪有多少个 std::shared_ptr 实例正共享该对象
  - 引用计数：
    - 当一个新的 std::shared_ptr 指向该对象（例如通过拷贝构造或赋值），引用计数会增加
    - 当一个 std::shared_ptr 被销毁或不再指向该对象时，引用计数会减少
  - 自动销毁：只有当最后一个指向对象的 std::shared_ptr 被销毁，使得引用计数降为零时，该对象才会被自动删除
- std::weak_ptr：不拥有所有权，不能被复制
- std::auto_ptr：过时的智能指针，不能被复制，在 C++11 中被删除.

## static_cast and dynamic_cast

在继承体系下，在继承体系中，从基类向派生类的类型转换方向称为"向下"。

一个很常见的应用就是，我只有一个基类的指针，但是我想判断它到底是指向哪一个派生类（在编译原理实验中我们就有用到）：

```cpp
class Animal { virtual ~Animal() {} };
class Dog : public Animal { void bark() {...} };
class Cat : public Animal { void meow() {...} };

void process(Animal* animal) {
    // 需要判断具体是Dog还是Cat
    if (Dog* dog = dynamic_cast<Dog*>(animal)) {
        dog->bark(); // 调用Dog特有方法
    }
}
```

在此我们特地说明，static_cast 和 dynamic_cast 的区别：

| 特性 | static_cast | dynamic_cast |
|------|-------------|--------------|
| 检查时机 | 编译时检查 | 运行时检查 |
| 安全性 | 不安全（程序员负责正确性） | 安全（失败返回nullptr或抛出异常） |
| 性能开销 | 无额外开销 | 有运行时类型检查开销 |
| 多态要求 | 不要求基类有虚函数 | 要求基类至少有一个虚函数 |
| 使用场景 | 基本类型转换、继承体系内向上/向下转换 | 主要用于多态类型的向下转换 |

**static_cast 只能用于以下情况：**

- 有继承关系的类（基类 ↔ 派生类）
- 同一类型的不同cv限定（如 const 转换）
- 基本类型的显式转换（如 int → double）
- 用户定义的转换操作符

在使用 dynamic_cast 时，基类必须有虚函数，这是因为 dynamic_cast 需要 RTTI（Run-Time Type Information），而 RTTI 需要虚函数表。

**dynamic_cast 只要有虚函数，就不会编译错误，但可能是会返回 nullptr**

!!! 一个例子

    Which code below fails compilation?

    A. 

    ```c++
    struct U {};
    struct V : public U {};
    struct W : public U {};
    int main()
    {
      U * p = new V;
      W * q = static_cast<W*>(p);
      return q == nullptr;
    }
    ```

    B. 

    ```c++
    struct U { virtual void foo() {} };
    struct V : public U {};
    struct W {};
    int main()
    {
      U * p = new V;
      W * q = dynamic_cast<W*>(p);
      return q == nullptr;
    }
    ```

    C.

    ```c++
    struct U { virtual void foo() {} };
    struct V : public U {};
    struct W : public U {};
    int main()
    {
      U * p = new V;
      W * q = dynamic_cast<W*>(p);
      return q == nullptr;
    }
    ```

    D. 

    ```c++
    struct U {};
    struct V : public U {};
    struct W {};
    int main()
    {
      U * p = new V;
      W * q = static_cast<W*>(p);
      return q == nullptr;
    }
    ```

    Answer: D

