# CPU-Pi：java、开源的CPU运算能力测试工具
## CPU-Pi: An Open Source、java based cpu-performance test tool.

### 为什么编写它
> 作为一个java程序员和攒机爱好者的结合体，作者本人当然不可能满足于使用现有的工具进行花样折腾。于是，在他入手了一块Intel i5-8500 CPU后，他就产生了这样一个疑问：这款号称“突破性”的桌面CPU相比于他手里的2016款MacBook Pro用的那块CPU有多大的差别？

------

* 在这种疑问的驱动下，他自己实现了一个CPU性能测试工具：CPU-Pi。

虽然，目前已经有很多种可用于测试CPU计算性能的成熟工具（例如 _国际象棋_ ）。不过作者也发现并非所有测试工具都靠谱——例如有些偷懒的“跑分软件”会将所有型号CPU存入一个内置数据库，然后仅仅通过获取主机的CPU型号来显示“分数”。这种行为显然算不上是一般意义上的 **性能测试**，它打分的过程根本没进行任何测试；还有稍微偷懒一点的做法——仅仅获得CPU的核心数和主频然后就打分。另外，这样的“跑分”软件多数还是闭源的，很难看到其内部逻辑。

综上，作者本着折腾的精神，弄出了这样一个 **可以进行单线程/多线程性能测试** 的、可以 **自定义测试压力** 的、 **算法和源代码均开源** 的简单测试工具。

### 它是如何对CPU性能进行测试呢？

作者使用一种“古老”并且认可度较高的测试方式：利用CPU计算π的近似值。根据莱布尼茨公式：

```
   pi  (-1)^n
   - = ------ (n=0-正无穷)
   4   2n+1
```

利用迭代的形式可以计算出π的精确值，且迭代次数越多，精确度越高，对CPU的运算压力也越大。利用这一方法即可对CPU的性能进行量化评比，评价的主要依据就是运算耗时。

作者设计了一套多线程计算方法，使得整个运算任务能够分别交由1-n线程进行，每个线程平分这些计算任务。这样即可以对CPU的单任务能力和多任务能力分别进行测试，目前本工具提供了从单线程到128线程的多种测试模式，基本可以满足大多数CPU的测试要求（对应家用CPU和服务器CPU的常见核心数）。

注意，并不是说4核8线程的CPU就只能应对最多8个线程的测试，实际上由于操作系统的线程优化和CPU硬件的一些指令优化，它的最高线程执行极限要比8个更高一些。例如经过作者本人测试，MacBook pro 2016 款的双核CPU在6线程模式下才能达到整体耗时最小，而进一步提高线程数无法减少时间，可见其多任务能力在6线程模式下发挥最好。

*附：MacBook pro 2016 CPU测试结果：
> 单线程模式（单任务能力测试）
测试压力-二十万次迭代<br />
π的计算结果-3.1415876535897618<br />
耗时-12481毫秒<br />
> 六线程模式（多任务能力测试）
测试压力-十九万九千九百九十八次迭代<br />
π的计算结果-3.141587653539797<br />
耗时-6789毫秒<br />

_注：如果你得出的成绩比上述两项高（耗时更少），则说明你所用的CPU比作者的要好。_

### 如何评价或比对性能？

首先要说明的是：这个测试工具主要着眼于CPU的基本能力： __双精度浮点运算能力__ ——进行测试。无代码层级的平台优化（因为java跨平台），也无代码层级的指令集优化。

测试标准：由耗时进行测试。你可以自定义测试压力，在 __同样的迭代压力__ 下， __耗时越短__ 则 __性能越强__ 。测试时应尽可能保证操作系统闲置——关闭其他程序以确保测试准确。

多任务压力测试时，你可以逐步提高线程数目，直到耗时不再减少，此时即达到你CPU的最大多线程承载能力。在线程模式相同且压力相同的情况下，耗时越短的平台其多任务能力越强。如果两个测试平台的最大线程承载能力不同，则在相同压力下，总耗时能可以通过提高线程数做到尽可能少的那个更强。

> 注意：它只是个工具，而工具是由人来使用的。如果你想使用自己的评价标准也可以，不用完全依照上述说法。

### 怎么用呢？

* 由于本工具是用java开发的，所以你必须先安装java环境。就像用DirectX开发的游戏必须先安装DirectX才能运行一样。
去java官网下载并安装java：[java官方网站](https://www.java.com/zh_CN/) 或百度搜索java进入官网。
* 下载并解压本工具，运行 CPU-Pi.jar 。
* 设定好迭代压力、选择线程模式并开始测试。
你可以在多个平台上对比测试结果或与朋友交流测试结果来评判自己的CPU性能处于一个什么水平。

### 我想查看其源代码学习一下/看看算法是否合理/看看有没有内置恶意代码
该程序的源代码已经封装在了jar包之中，您可以使用Eclipse或IDEA将其导入并查看其内容。具体的各个类的作用已经被清晰地注释了，相信你能轻松地看懂它们。

### 需要联系作者？
请至信： kohgylw@163.com 青阳龙野





