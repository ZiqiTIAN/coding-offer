https://zhuanlan.zhihu.com/p/48523943
# 1. C和C++的区别
1. C是面向过程的语言，是一个结构化的语言，考虑如何通过一个过程对输入进行处理得到输出；<br>
C++是面向对象的语言，主要特征是“封装、继承和多态”。封装隐藏了实现细节，使得代码模块化；派生类可以继承父类的数据和方法，扩展了已经存在的模块，实现了代码重用；多态则是“一个接口，多种实现”，通过派生类重写父类的虚函数，实现了接口的重用。<br>
2. C和C++动态管理内存的方法不一样，C是使用malloc/free，而C++除此之外还有new/delete关键字。<br>
3. C++支持函数重载，C不支持函数重载<br>
4. C++中有引用，C中不存在引用的概念<br>

# 2.#include<file.h>与#include "file.h"的区别
前者是从标准库路径寻找，后者是从当前工作路径寻找

# 3.C++文件编译与执行的四个阶段
1. 预处理：根据文件中的预处理指令来修改源文件的内容 <br>
2. 编译：编译成汇编代码 <br>
3. 汇编：把汇编代码翻译成目标机器指令 <br>
4. 链接：链接目标代码生成可执行程序 <br>

# 4.堆和栈有什么区别
1. 概念<br>
栈stack:<br>
存放函数的参数值、局部变量，由编译器自动分配释放<br>
堆heap：<br>
是由new分配的内存块，由应用程序控制，需要程序员手动利用delete释放，如果没有，程序结束后，操作系统自动回收<br>
2. 因为堆的分配需要使用频繁的new/delete，造成内存空间的不连续，会有大量的碎片<br>
3. 堆的生长空间向上，地址越大，栈的生长空间向下，地址越小<br>
可参考：https://blog.csdn.net/yingms/article/details/53188974
# 5.深拷贝和浅拷贝的区别
[深拷贝&&浅拷贝](深拷贝&&浅拷贝.md)

