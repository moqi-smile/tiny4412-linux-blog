===========================================================
汇编指令
===========================================================

-----------------------------------------------------------
一 内嵌汇编语法
-----------------------------------------------------------

-----------------------------------------------------------
二 条件执行助记符
-----------------------------------------------------------

    相等 : EQ

    不相等 : NE

    无符号数大于或等于 : CS/HS

    无符号数小于 : CC/LO

    负数 : MI

    正数或零 : PL

    溢出 : VS

    没有溢出 : VC

    无符号数大于 : HI

    无符号数小于或等于 : LS

    有符号数大于或等于 : GE

    有符号数小于 : LT

    有符号数大于 : GT

    有符号数小于或等于 : LE

    无条件执行，跟不写一样 : AL

-----------------------------------------------------------
三 寄存器
-----------------------------------------------------------

-----------------------------------------------------------
四 堆栈
-----------------------------------------------------------

    满递增栈(fa)

    满递减栈(fd)

    空递增栈(ea)

    空递减栈(ed)

    .. note::

        C语言使用的是满递减栈

-----------------------------------------------------------
五 APCS 规则
-----------------------------------------------------------

    APCS规则是用于约束不同语言之间的相互调用

    内容:

    1. 如果参数传递小于四个，则放到r0-r3; 如果参数多余四个, 则第五个开始按照顺序入栈（越靠后的参数越先入栈）。
    2. 使用的堆栈必须是满递减栈。
    3. 返回值如果是32位，则将参数放在r0; 如果参数是64位, 则将参数放在r1与r0内

-----------------------------------------------------------
六 Arm汇编指令
-----------------------------------------------------------

    以下所有示例都是使用c语言作为例子

***********************************************************
1. 赋值指令
***********************************************************

    赋值指令 : mov

        将一个数值赋给寄存器

        用法 :

        .. code::

            mov r0, r1

        相当于c语言里的

        .. code::

            r0=r1;

        r0的位置必须是一个寄存器, r1的位置可以是一个寄存器也可以是一个立即数

    取反赋值指令 : mvn

        将一个数值取反再赋给寄存器

        用法 :

        .. code::

            mvn r0, r1

        相当于c语言里的

        .. code::

            r0=~r1;

        r0的位置必须是一个寄存器, r1的位置可以是一个寄存器也可以是一个立即数

    赋值伪指令: ldr

        将一个数值赋给寄存器

        .. code::

            ldr r0, =0x7FFFFFFF

        相当于c语言里的

        .. code::

            r0=0x7FFFFFFF;

        .. note::

            该指令是伪指令, 芯片无法使别，编译器会将该指令转化为多条芯片可以识别的指令。


***********************************************************
2. 算术指令
***********************************************************

    加法指令: add

        将两个数值的值相加并赋给一个寄存器

        用法 :

        .. code::

            add r0, r1, r2

        相当于c语言里的

        .. code::

            r0=r1+r2;

        r0, r1的位置必须是一个寄存器, r2的位置可以是一个寄存器也可以是一个立即数

    减法指令: sub

        将两个数值的值相减并赋给一个寄存器

        用法 :

        .. code::

            sub r0, r1, r2

        相当于c语言里的

        .. code::

            r0=r1-r2;

        r0, r1的位置必须是一个寄存器, r2的位置可以是一个寄存器也可以是一个立即数

    乘法指令: mul

        将两个寄存器的值相乘并赋给一个寄存器

        用法 :

        .. code::

            mul r0, r1, r2

        相当于c语言里的

        .. code::

            r0=r1*r2

        r0, r1, r2的位置必须是一个寄存器

    乘加指令: mla

        将一个寄存器的值加上两个寄存器相乘的值并赋给一个寄存器

        用法 :

        .. code::

            mla r0, r1, r2, r3

        相当于c语言里的

        .. code::

            r0=r3+r1*r2;

        r0, r1, r2, r3的位置必须是一个寄存器

    乘减指令: mls

        将一个寄存器的值减掉两个寄存器相乘的值并赋给一个寄存器

        用法 :

        .. code::

            mls r0, r1, r2, r3

        相当于c语言里的

        .. code::

            r0=r3-r1*r2;

        r0, r1, r2, r3的位置必须是一个寄存器

    除法指令:

    .. note::

        Arm指令+s代表要影响到CPSR。当我们要计算64位的加法运算的时候是不能使用add计算出来的，必须先用adds计算低32位,然后再用adc计算高32位, 此时的adc指令就会判断低32位计算是是否有进位, 有的话会自动加一

***********************************************************
3. 位操作指令
***********************************************************

    按位与指令: and

        将两个寄存器的值按位与并将结果赋给一个寄存器

        用法 :

        .. code::

            and r0, r1, r2

        相当于c语言里的

        .. code::

            r0=r1&r2;

        r0, r1的位置必须是一个寄存器, r2的位置可以是一个寄存器也可以是一个立即数

    按位或指令: orr

        将两个寄存器的值按位或并将结果赋给一个寄存器

        用法 :

        .. code::

            orr r0, r1, r2

        相当于c语言里的

        .. code::

            r0=r1|r2;

        r0, r1的位置必须是一个寄存器, r2的位置可以是一个寄存器也可以是一个立即数

    按位异或指令: eor

        用法 :

        .. code::

            eor r0, r1, r2

        相当于r0=r1^r2;

        r0, r1的位置必须是一个寄存器, r2的位置可以是一个寄存器也可以是一个立即数

    位清零指令: bic

        将一个寄存器的值的部分位清零并将结果赋给一个寄存器

        用法 :

        .. code::

            bic r0, r1, r2

        相当于c语言里的

        .. code::

            r0=r1&(~r2);

        r0, r1的位置必须是一个寄存器, r2的位置可以是一个寄存器也可以是一个立即数

    逻辑左移指令: lsl

        将一个寄存器的数值左移n位, 并在右边补n个0

        用法 :

        .. code::

            mov r0, r1, lsl r2

        相当于c语言里的

        .. code::

            r0 = r1 << r2;

        r0, r1的位置必须是一个寄存器, r2的位置可以是一个寄存器也可以是一个立即数

    逻辑右移指令: lsr

        将一个寄存器的数值y右移n位, 并在左边补n个0

        用法 :

        .. code::

            mov r0, r1, lsr r2

        相当于c语言里的

        .. code::

            r0=r1;
            for (int i=0; i<r2; i++)
            {
                r0 >>=1;
                ru &= ~0x80000000;
            }

        r0, r1的位置必须是一个寄存器, r2的位置可以是一个寄存器也可以是一个立即数

    算数右移指令: asr

        将一个寄存器的数值y右移n位, 如果该寄存器的数值为正则在左边补n个0,反之补n个1

        用法 :

        .. code::

            mov r0, r1, lsr r2

        相当于c语言里的

        .. code::

            r0 = r1 >> r2;

        r0, r1的位置必须是一个寄存器, r2的位置可以是一个寄存器也可以是一个立即数

    循环右移: ror

        将一个寄存器的数值y右移n位, 并将该数值移动的n位补充到左边

        用法 :

        .. code::

            mov r0, r1, ror r2

        r0, r1的位置必须是一个寄存器, r2的位置可以是一个寄存器也可以是一个立即数



***********************************************************
4. 比较指令
***********************************************************

    比较指令: cmp

        比较两个寄存器的大小, 并根据运算结果更新CPSR中条件标志位的值

        用法 :

        .. code::

            cmp r0, r1
            movgt r2, #1

        相当于c语言里的

        .. code::

            if (r0>r1)
                r2=1;

        r0的位置必须是寄存器, r1的位置可以是一个寄存器也可以是一个立即数


    比较相等指令: teq

        比较两个寄存器的大小是否相等, 并根据运算结果更新CPSR中条件标志位的值

        用法 :

        .. code::

            teq r0, r1
            moveq r2, #2
            movne r2, #3

        相当于c语言里的

        .. code::

            if (r0==r1)
                r2=2;
            else
                r2=3;

        r0的位置必须是寄存器, r1的位置可以是一个寄存器也可以是一个立即数

    比较指令: tst

        把两个寄存器的数值进行按位与运算, 并根据运算结果更新CPSR中条件标志位的值

        用法 :

        .. code::

            tst r0, r1
            moveq r2, #0
            movne r2, #1

        相当于c语言里的

        .. code::

            if ((r0&r1)==0)
                r2=0;
            else
                r2=1;

        r0的位置必须是寄存器, r1的位置可以是一个寄存器也可以是一个立即数

    .. note::

        比较指令类似于减法指令, 但不会保存结果。 一般使用完比较指令都会使用条件执行指令来执行想要执行的指令

***********************************************************
5. 内存操作指令
***********************************************************

    地址取值指令: ldr

        将一个地址里的值放到寄存器中

        用法① :

        .. code::

            ldr r0, [r1]

        相当于c语言里的

        .. code::

            r0 = *r1;

        用法② :

        .. code::

            ldr r0, [r1, #4]

        相当于c语言里的

        .. code::

            r0 = *(r1+4);

    地址赋值指令: str

        将一个寄存器的值放到一个地址里

        用法① :

        .. code::

            str r0, [r1]

        相当于c语言里的

        .. code::

            *r1 = r0;

        用法② :

        .. code::

            str r0, [r1, #4]

        相当于c语言里的

        .. code::

            *(r1+4) = r0;

    .. note::

        如果希望执行完指令以后更改r1寄存器内的地址，可以在方括号外增加一个感叹号或者将增加/减少的地址放在方括号外

        例:

        ① ldr r0, [r1, #4]! 相当于c语言里的 r1+=4; r0=r1;

        ② ldr r0, [r1], #4 相当于c语言里的 r0=r1; r1+=4;

        str 的用法与ldr一样


***********************************************************
6. 堆栈指令
***********************************************************

    压栈指令: push

        用满递减栈的方式将寄存器的值写入到堆栈

        用法

        .. code::

            push {r0, r1, r2}

    弹栈指令: pop

        用满递减栈的方式将堆栈的值写入到寄存器

        用法

        .. code::

            pop {r0, r1, r2}

    压栈指令: stm

        用指定方式将寄存器的值写入到堆栈

        用法

        .. code::

            stmfd sp!, {r0, r1, r2}

    弹栈指令: ldm

        用指定方式将堆栈的值写入到寄存器

        用法

        .. code::

            ldmfd sp!, {r0, r1, r2}

    .. code::

        如果是以空/满递增的方式操作堆栈，则寄存器号小的寄存器先入栈，寄存器号大的先出栈
        如果是以空/满递减的方式操作堆栈，则寄存器号大的寄存器先入栈，寄存器号小的先出栈


***********************************************************
7. 跳转指令
***********************************************************

    跳转指令: b

        跳转到指定位置(标号)

        .. code::

            b start

        相当于c语言里的

        .. code::

            goto start;

        start的位置必须是一个标号

        .. note::

            **start** 是由用户定义的标号， 必须使用合法标识符

    跳转指令: bl

        跳转到指定位置(标号), 并将跳转前将当前的指令地址传递到lr, 一般与bx指令搭配使用

        .. code::

            b start

        相当于c语言里的函数调用

        start的位置必须是一个标号

    跳转指令: bx

        跳转到指定位置(寄存器), 一般与bl指令搭配使用

        .. code::

            b lr

        相当于c语言里的函数返回

        lr的位置必须是一个寄存器

***********************************************************
8. 特殊指令
***********************************************************

    系统调用指令: swi/svc

        调用系统调用函数

        用法

        .. code::

            swi 0x900004

        相当于c语言里的

        .. code::

            syscall(4);

        .. note::

            0x900000是由内核源码内的 __NR_SYSCALL_BASE 宏定义所决定;

            4是write函数的系统调用号, 也可以在内核源码内找到。

            系统调用如果要往函数传递参数, 需要遵守APCS规则

    读cpsr指令: mrs

        读取寄存器CPSR的值

        用法 :

        .. code::

            mrs r0, cpsr

        r0的位置必须是寄存器

    写cpsr指令: msr

        给寄存器cpsr赋值


        用法 :

        .. code::

            msr cpsr, r0

        r0的位置必须是寄存器
