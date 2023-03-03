python3将byte输出要使用以下方法，而不是直接print
https://stackoverflow.com/questions/54550845/strange-behaviour-of-python3-in-hex

比较简便的操作可以看：[Buffer overflow prepare](https://tryhackme.com/room/bufferoverflowprep)


immunity debugger 和 edb的用法类似
mona 只有在immunity debugger上有， edb有自己的插件


1. 通过fuzzing找到大概多少字节的输入会导致应用程序崩溃
2. 利用msf-pattern_create 生成特殊字符串，并发送到应用
3. 查看当前EIP寄存器中的值，并通过msf确定最终的offset长度
4. 利用msf nasm 查找jmp esp的操作码，并使用mona在debugger中搜索此操作码的地址，注意，需要在没有应用各种安全措施的module中搜索
5. 将jmp esp的地址防止在覆盖EIP的位置
6. 测试可以用于shellcode的空间地址是多少
7. 如果地址够用：
	1. 找出所有的bad charactor，方法有很多，mona有简单的方法，一个个尝试也可以
	2. msf生成shellcode，注意bad charactor和encoder
	3. 添加nop操作码
8. 如果地址不够：
	1. 查找是否有寄存器的值指向之前输入的offset的字符串
	2. 查看可用的shellcode空间
	3. 利用nasm查看jmp 到这个寄存器的操作码是多少
	4. 将这个操作码作为shellcode 的stager
	5. 将真正的shell code 储存到前面offset的字符串中，注意长度要保持一致。


