# 从cpu中学习系统设计的原则
如果从1971年intel发布第一款商用微处理器4004算起，cpu已经有近50年的发展历史了，这几十年间，
伴随着半导体工艺的发展，为了守护神圣的摩尔定律，cpu的设计者们不断引入一些新的设计和优化，大幅提
高了cpu的性能和功能。从这段历史中，我们又可以学到哪些系统设计的思想呢，本文抛砖引玉，希望给读者一定的启发。

### 并行和流水线
* 并行似乎是个很初级的话题，现在很多程序员都了解多线程，多进程，多机分布式等话题，系统设计时往这些方向上
靠就行了，流水线平常谈的少一些，但是大家也不陌生，无非就是将复杂的过程分解为多个小的步骤，提升并行度。
然而要系统性的设计和优化一个系统的并行处理能力，则需要从不同的角度和层次综合考量。并行可以简单分为计算并行和数据并行
或者是二者的混合，我们在系统设计的时候可以自顶向下的逐层设计。以现代微处理器为例，cpu的并行已经发展为指令并行，
数据并行，线程并行三种不同的类型，每种类型都引入了不同的技术来进行并行优化。指令并行中引入了包括分支预测，推测执行，超标量，流水线，乱序发射等技术，其中以超标量和流水线效果最为显著，超标量通过引入多个ALU/FPU等计算单元，以空间换时间达到并行执行多个指令；流水线技术将指令执行分解为取指令，解码，执行，内存访问等多个阶段，可以更充分利用各个单元，提高系统吞吐量，然而，如何设计流水线又是一个问题，需要我们在系统设计时仔细权衡，过长或过短的流水线都有弊端，吞吐量和延时往往不可兼得，比如英特尔P4的Netburst架构引入了最长31级流水线，然而性能不升反降，后续从core架构开始到现在都维持14级流水线，性能稳定提升。数据并行一般通过向量处理器或者向量指令扩展实现，采用SIMD架构，能够极大的提升特定运算的性能，用户通过简单的指令即可以很好的优化并行，目前类似AVX指令集已经得到了非常广泛的应用，可见系统提供简单易用的并行原语/接口非常有利于用户开发和调优。线程并行，可以看作是计算并行和数据并行的混合，对于SMP架构的cpu来说，线程之间通过共享内存和缓存/总线通讯，对于我们设计的系统来说，要考虑的核心问题是如何分发任务，任务之间如何通讯和共享数据，任务执行的结果如何聚合等。

### 局部性原理
局部性原理应该是计算机系统里最重要的原理之一了，也是各种优化手段的可以实施的基本假设，它指的是程序往往会使用它最近使用过的代码和数据，一般可以分为时间局部性和空间局部性，时间局部性指的是最近使用过的数据很可能会再次使用，空间局部性指的是附近的数据很可能也要一并使用。cpu内部各种cache，TLB，预取等技术的引入，都是基于局部性原理，各种优化迭代也是为了不断提高预测的准确率。在系统设计中，我们需要深刻理解业务模型，从时间和空间两个维度分析业务模型的局部性，选用经济适用的方案来优化性能。

### 28定律
28定律也叫帕累托定律，指的是对于许多事件20%的因素产生80％的影响，在计算机科学中，常见的28现象有20％的代码有80％的错误，最难的20％的代码占用80％的时间等，它指导我们聚焦最重要的部分可以取得最大的效果。在处理器的设计过程中，我们看到很多


### 没有什么问题是引入一个中间层解决不了的，如果有，就引入两个


### 万金油的缓存


### 存储和计算分离
冯诺依曼架构和哈佛架构


### 批处理和异步


### 没有银弹，只有TradeOff
光速的极限是30w公里/s，晶体管的极限是1nm，资源的极限是老板给你的预算，系统的设计无法规避各种各样的限制，从最优化的角度来说，
系统设计是一个带约束的优化问题，你根据业务目标构建合适的目标系统，同时要满足各种约束条件，其中很核心的问题就是如何取舍，各个取舍的利弊需要考虑清楚。2018年曝光的处理器Meltdown和Spectre漏洞，震惊了业界，其危害性和普适性史无前例，而其根源来源于现代处理器的架构设计，由于现代化的处理器都采用了推测执行（speculative execution）来进行任务调度，即处理器会根据工作任务预先给出猜测指令，如果符合特定的条件即进行执行，否则的话就会丢弃这条推测的指令，不进行执行。推测执行是所有处理器中最基本的一个架构功能，所以它可以直接从缓存中获取所有包括标识为不可读的数据，另外有这样权限的还有处理器的分支预测（branch predictor），这些处理器功能都可以被利用来获取一些敏感信息，这就为恶意程序利用缓存非法读取内核和其他程序的数据留下了可能。而这个问题要彻底修复，只能等下一代处理从硬件上修改才行。所以我们看到，在处理器中引入推测执行，你获得了更高的指令级并行程度，提高了性能，但是也留下了数据泄露的隐患。我们无从得知当初处理器的设计者是没有考虑到这些问题还是决定安全性让位给性能，留给我们的思考是引入任何一项新的功能和组件时，我们都要明确分析利弊，尽量给出多个方案进行比较，集合不同领域专家的意见，以期给出最为科学的取舍，天下没有免费的午餐，同样适合于系统设计领域。