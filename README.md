
# gdb高级应用记录
##内存断点
 watch *(unsigned long*)0x7fffffffe3f0 if *(unsigned long*)0x7fffffffe3f0 > 0xffffffff
 当0x7fffffffe3f0地址按照unsigned long取值，被写入的数据大于0xffffffff时，被断下
 
经常遇到因为内存而导致段错误，但是根本原因可能不在当前现场。命案现场已经被破坏了，那么我们怎么能获取第一现场呢？
所以我们经常需要用到内存断点，并且是需要下有条件的内存断点。
    
