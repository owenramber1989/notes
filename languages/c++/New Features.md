## structured binding

c++11引入了std::tuple,而c++17允许我们获取元组中的数据并定义之。

![[Pasted image 20230409174352.png]]

```cpp
#include <iostream> 
#include <tuple> 

using std::cout;
using std::endl;

auto f(){
	return std::make_tuple(1,2,3,"here we go!");
}

int main(){
	auto [x,y,z,w] = f();
	cout<<x<<" "<<y<<" "<<z<<" "<<w<<endl;
	return 0;
}
```

