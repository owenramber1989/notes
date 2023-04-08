# Containers
***
There are two types of containers.

- Sequence:  
	● Containers that can be accessed  sequentially  
	● Anything with an inherent order  goes here!
- Associative  
	● Containers that don’t necessarily have a sequential order  
	● More easily searched  
	● Maps and sets go here!

## 顺序容器
-   和其他容器不同，默认构造的`array`是非空的。
-   直接复制：将一个容器复制给另一个容器时，类型必须匹配：容器类型和元素类型都必须相同。
-   使用迭代器复制：不要求容器类型相同，容器内的元素类型也可以不同。


### 容器操作可能使迭代器失效

-   在向容器添加元素后：
    -   如果容器是`vector`或`string`，且存储空间被重新分配，则指向容器的迭代器、指针、引用都会失效。
    -   对于`deque`，插入到除首尾位置之外的任何位置都会导致指向容器的迭代器、指针、引用失效。如果在首尾位置添加元素，迭代器会失效，但指向存在元素的引用和指针不会失效。
    -   对于`list`和`forward_list`，指向容器的迭代器、指针和引用依然有效。
-   在从一个容器中删除元素后：
    -   对于`list`和`forward_list`，指向容器其他位置的迭代器、引用和指针仍然有效。
    -   对于`deque`，如果在首尾之外的任何位置删除元素，那么指向被删除元素外其他元素的迭代器、指针、引用都会失效；如果是删除`deque`的尾元素，则尾后迭代器会失效，但其他不受影响；如果删除的是`deque`的头元素，这些也不会受影响。
    -   对于`vector`和`string`，指向被删元素之前的迭代器、引用、指针仍然有效。
    -   注意：当我们删除元素时，尾后迭代器总是会失效。
    -   注意：使用失效的迭代器、指针、引用是严重的运行时错误！
    -   建议：将要求迭代器必须保持有效的程序片段最小化。
    -   建议：不要保存`end`返回的迭代器。

### 容器内元素的类型约束

-   元素类型必须支持赋值运算；
-   元素类型的对象必须可以复制。
-   除了输入输出标准库类型外，其他所有标准库类型都是有效的容器元素类型


## 各种容器之间的主要差别

1. 支持快速随机访问的有:vector deque  array
2. 连续存储元素的有: vector string ，连续存储带来的最大的问题就是在中间插入或者修改元素时非常耗时，vector最好只在尾部插入/删除元素，deque头尾都很快
3. 任意位置： list 和 forward_list 在任意位置插入/删除元素都很快，相应的没法随机访问，其中list 双向，forward_list 则是单向链表，这俩开销比较大
4.  array 对象的大小是固定的

### swap
```cpp
vector<int> v1 = {1,2,3,4,5};  
vector<int> v2 = {9,8,7,6,5};  
std::swap(v1,v2);
```
swap只是交换了两个容器的内部数据结构
除array外，swap不对任何元素进行拷贝、插入或删除操作，因此可以保证在常数时间内完成
## Vector
### Initialization
Notice the slight difference
```cpp
std::vector<int> vec1(3,5);
//makes {5,5,5}
std::vector<int> vec2{3,5};
//makes {3,5}
```

```cpp
list<string> slist;
vector<string> svec;
slist.insert(slist.begin(),v.end()-2,v.end());
```

