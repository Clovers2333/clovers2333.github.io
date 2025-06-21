# Smart Pointer


Smart Pointers in Standard Library¶
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