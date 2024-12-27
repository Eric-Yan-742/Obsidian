[makefile介绍 — 跟我一起写Makefile 1.0 文档 (seisman.github.io)](https://seisman.github.io/how-to-write-makefile/introduction.html#id2)

[Makefile从入门到上手_makefile菜鸟教程-CSDN博客](https://blog.csdn.net/qq_41839588/article/details/132267196)

- 对于每一个目标的执行顺序
    1. 先递归式的准备（更新/生成）好所有依赖
    2. 如果目标的不存在或最后修改时间早于任何依赖的最后修改时间，执行下面的命令。
- Make会根据.o的同名.c/.cpp文件自动推导出.c文件作为依赖，但并不会自动推断头文件，依赖的头文件需要加入依赖列表
    
    ```Makefile
    objects = main.o kbd.o command.o display.o\    insert.o search.o files.o utils.o
    
    edit :$(objects)    cc -o edit$(objects)main.o : defs.h
    kbd.o : defs.h command.h
    command.o : defs.h command.h
    display.o : defs.h buffer.h
    insert.o : defs.h buffer.h
    search.o : defs.h buffer.h
    files.o : defs.h buffer.h command.h
    utils.o : defs.h
    
    .PHONY : clean
    clean :
        rm edit$(objects)
    ```
    
- 不要再同一个目标下编译再运行，不知道为啥运行完后就直接把目标给删除了
- 执行命令可以单独建一个目标。把执行命令加入.PHONY，代表不会真正生成这个目标
    
    ```Makefile
    eserver: echo_server.o
    	$(CC) echo_server.o -o eserver	
    run_eserver: eserver
    	./eserver 9190
    ```
    
- 在编辑Makefile脚本时，如果要用换行符\，切记一定不要在其后面加任何东西！包括Tab和空格！
    
    [嵌入式Linux学习笔记（1）——Makefile中使用换行符“\”的注意事项_makefile换行-CSDN博客](https://blog.csdn.net/Jan_wen/article/details/126429340)
    
- 在\前面还得加一个空格，不能让\紧跟着最后一个文件名
- 除了伪目标，所有目标名称都必须匹配文件的路径。因为在判断是否需要执行规则时，需要将目标的时间戳和依赖的时间戳做对比，如果Make找不到目标文件（路径不对），就会当目标不存在，所以不管依赖什么时候改的都会重新执行规则。
    - 无论什么情况我们肯定都需要执行伪目标的规则，所以伪目标的名称无所谓带不带路径。