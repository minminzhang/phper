## php传值和传引用的区别：

传值在函数范围内，改变变量值的大小不会影响到函数外的变量值；
传引用在函数范围内，对值的任何改变在函数外也有所提现，传引用传的是内存地址。

传值和copy是一样的，传引用类似于C语言的指针

优缺点：传值会很耗时间，特别是对于大型的字符串和对象来说这将会是一个代价很大的操作；传引用 函数内的任何操作等同于对传送变量的操作，传送大型变量时效率高。
