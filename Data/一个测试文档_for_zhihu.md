# 一个测试文档

## 文字与图片

这里是一个测试文档这里是一个测试文档这里是一个测试文档这里是一个测试文档这里是一个测试文档这里是一个测试文档这里是一个测试文档这里是一个测试文档这里是一个测试文档这里是一个测试文档这里是一个测试文档这里是一个测试文档这里是一个测试文档这里是一个测试文档这里是一个测试文档。

![这里是一个测试图片](https://raw.githubusercontent.com/Bill-Mars/Markdown4Zhihu/master/Data/一个测试文档/image-01.png)

## 下面是一段C++代码：

```C++
#include <memory>

struct C10_API Allocator {
  virtual ~Allocator() = default;

  virtual DataPtr allocate(size_t n) const = 0;
  virtual DeleterFnPtr raw_deleter() const {
    return nullptr;
  }
  void* raw_allocate(size_t n) {
    auto dptr = allocate(n);
    AT_ASSERT(dptr.get() == dptr.get_context());
    return dptr.release_context();
  }
  void raw_deallocate(void* ptr) {
    auto d = raw_deleter();
    AT_ASSERT(d);
    d(ptr);
  }
};
```

## 这里是一个表格和基础的公式

| Item1 | Item2   | Item3   | Item4   |

| ----- | ------- | ------- | ------- |

| Row1  | Content | Content | Content |

| Row2  | Content | Content | Content |

| Row3  | Content | Content | Content |


$$
Sum=\sum_{i=0}^{n}{\sqrt{x_i^2+y_i^2}}\\
Sum2=\sum_{i=2}^{n}{\sqrt{x_i^2+y_i^2}}
$$

$y=x$ 

$$
Sum3=\sum_{i=0}^{n}{\sqrt{x_i^2+y_i^2}}
$$
test$Sum=\sum_{i=0}^{n}{\sqrt{x_i^2+y_i^2}}$ test

werewdctest
