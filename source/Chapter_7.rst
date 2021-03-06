===========================================================
制作一个属于自己的驱动源码
===========================================================

-----------------------------------------------------------
一. 制作源码
-----------------------------------------------------------

首先我们先创建一个文件夹作为我们的源码目录

.. code::

    mkdir demo

然后创建两个文件。一个是源码文件 demo.c，另一个时用于编译源码的 Makefile

.. code::

    touch demo.c Makefile

打开 demo.c, 输入以下内容

.. code::

    #include <linux/init.h>
    #include <linux/module.h>
    #include <linux/sched.h>

    static __init int Demo_Init(void)
    // 加了init定义以后该函数会加入到内核的init段
    {
            printk ("Hello Kernel\n");
            printk ("Hello Kernel\n");
            printk ("Hello Kernel\n");
            printk ("Hello Kernel\n");
            printk ("Hello Kernel\n");
            printk ("Hello Kernel\n");
            printk ("Hello Kernel\n");
            printk ("Hello Kernel\n");
            printk ("Hello Kernel\n");
            printk ("Hello Kernel\n");

            return 0;
    }

    module_init(Demo_Init);
    /* 模块初始化函数 */
    MODULE_LICENSE("GPL");
    /* 开源规则 */
    MODULE_AUTHOR("Moqi");
    /* 作者 */
    MODULE_VERSION("V1.0");
    /* 版本 */
    MODULE_DESCRIPTION("Demo");

当将该驱动编入内核中以后，内核源码就会调用 Demo_Init 函数来初始化该驱动。当系统启动以后就会打印 **Hello Kernel** ，为了防止系统启动以后打印过多日志而比较难找到该信息，我多打印了几次。
接下来制作 Makefile， 打开 Makefile 输入

.. code::

    obj-y += demo.o

最后整个文件夹复制到内核源码的 drivers 目录下

为了让源码知道这个目录, 还需要修改一下 drivers 的 Markfile 文件。打开Markfile ，在最后加入

.. code::

    obj-y                           += demo/

到这里就制作完毕了。只要再编译多一次源码， 然后将固件拷进开发板内，看看启动信息是否有 **Hello Kernel**。如果有，证明制作成功了。

.. figure:: ./_static/Chapter_7/Hello-Kernel.png
	:align: center
	:figclass: align-center

-----------------------------------------------------------
二. 使用菜单裁剪自己的代码
-----------------------------------------------------------

在 linux 系统下开发工程时可以使用一个图形界面来裁剪工具，这个图形界面会生成一份宏定义文件 .config，我们可以根据这些宏定义来裁剪我们的代码。只需在顶层目录下输入 make menuconfig 就会弹出这个图形界面。接下来我们就来试着在这个图形界面内加入我们想要的宏定义。

首先，我们进入到我们的源码目录下，创建一个文件 叫做 Kconfig

.. code::

    cd linux-3.5/drivers/demo && touch Kconfig

打开 Kconfig 输入以下内容

.. code::

    config  DEMO_ENABLE
            bool "Enable Demo"
            default n
            help
            Whether to use demo.c or not

DEMO_ENABLE 是该选项的名称。
bool 是该选项的类型，因为我要用这个定义来选择是否编译demo，所以我选择了布尔型。后面的字符串是对该选项的介绍
default 是该选项的默认值
help 是对该选项的帮助选项。

接下来我们要让内核代码知道该Kconfig的存在，我们得更改上层目录的Kconfig。打开 drivers 目录下的 Kconfig，在文件最后输入

.. code::

    source "drivers/demo/Kconfig"

然后退到 linux-3.5文件夹下，输入 make menuconfig 然后选择 **Device Drivers  --->** ，拖到最下面就可以看到我们制作的选项。

.. figure:: ./_static/Chapter_7/DEMO_Enable.png
	:align: center
	:figclass: align-center

将该选项选上，退出保存以后，在顶层目录下的.config里会出现一个新的宏定义 CONFIG_DEMO_ENABLE=y。到这里我们的选项就已经做好了。	

上面我们只是做了个宏定义，并没有将demo源码与其关联上，接下来我们将这两部分关联起来
打开 demo 下的 Markfile 将其修改为

.. code::

    obj-$(CONFIG_DEMO_ENABLE) += demo.o

这样就将这个宏定义与我们的驱动源码关联起来了。如果将该选项取消，内核源码就不会编译该驱动。

如果我们希望在源码内加入宏定义，来选择编译我们的代码也可以在该 Kconfig 里增加。Kconfig还有其他类型的变量这里就不再赘述。

