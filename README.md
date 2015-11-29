
# gdb高级应用记录
##内存断点
 `watch *(unsigned long*)0x7fffffffe3f0 if *(unsigned long*)0x7fffffffe3f0 > 0xffffffff`
    当0x7fffffffe3f0地址按照unsigned long取值，被写入的数据大于0xffffffff时，被断下
 
    经常遇到因为内存而导致段错误，但是根本原因可能不在当前现场。命案现场已经被破坏了，那么我们怎么能获取第一现场呢？
所以我们经常需要用到内存断点，并且是需要下有条件的内存断点。
##汇编调试
* set disassemble-next-line on
* set disassembly-flavor att (或者intel) //熟悉INTEL汇编指令格式的，可以指定汇编代码格式

##gdb启动自加载断点
1. DB使用中比较麻烦的事情，就是每次启动，还要手动敲一把命令，特别是断点比较多的情况，这个特便影响，工作效率。查了一下gdb info，gdb支持自动读取一个启动脚本文件.gdbinit，所以经常输入的启动命令，就都可以写在gdb启动目录的.gdbinit里面。比如
`
.gdbinit:
  file myapp
  handle SIGPIPE nostop
  break ss.c:100
  break ss.c:200
  run`
GDB和bash类似，也支持source这个命令，执行另外一个脚本文件。所以可以修改一下.gdbinit:
`.gdbinit:
  file myapp
  handle SIGPIPE nostop
  source gdb.break
  run
gdb.break:
  break ss.c:100
  break ss.c:200`
这样修改的断点配置，只需要编辑gdb.break就可以了。

2. 偶而还是需要单独启动GDB，不想执行自动脚本，于是又改进了一下。首先把.gdbinit命名为gdb.init，然后定义一个shell alias:
  $ alias .gdb=”gdb -x gdb.init”
这样如果需要使用自动脚本，就用.gdb命令，否则用gdb进入交互状态的gdb。这样配置以后可以一个简单命令就开始调试，整个效率就能提高不少。
