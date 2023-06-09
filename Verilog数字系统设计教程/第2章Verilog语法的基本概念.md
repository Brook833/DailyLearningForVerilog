# 第二章 Verilog语法的基本概念

## 概述
Verilog HDL是一种用于数字系统设计的语言。用Verilog HDL描述的电路设计就是该电路的Verilog HDL模型，也成为模块。Verilog HDL既是一种行为描述的语言，也是一种结构描述的语言。

这就是说，无论描述电路功能行为的模块或描述元器件或较大部件互联的模块都可以用Verilog语言来建立电路模型。如果按照一定的规则和风格编写，功能行为模块可以通过工具自动地转换为门级互联地结构模块。

Verilog模型可以是实际电路地不同级别地抽象。这些抽象的级别和它们所对应的模型类型共有以下5种，现分别给以简述。
> + (1) 系统级(system-level):用语言提供的高级结构能够实现待设计模块的外部性能的模型。
> + (2) 算法级(algorithem-level):用语言提供的高级结构能够实现算法运行的模型。
> + (3) RTL级(register tramsfer level):描述数据在寄存器之间的流动和如何处理、控制这些数据流动的模型。

以上三种都属于行为描述，只有RTL级才与逻辑电路有明确的对应关系。

> + (4) 门级(gate-level):描述逻辑门以及逻辑门之间连接的模型。

与逻辑电路有确定的连接关系，以上4种数字系统设计工程师必须掌握。

> + (5) 开关级(switch-level):描述器件中三极管和储存节点以及它们之间连接的模型。

这与具体的物理电路有对应关系，工艺库元件和宏部件设计人员必须掌握，将在高级教程中介绍。

一个复杂电路系统的完整Verilog HDL模型是由若干个Verilog HDL模块构成的，每一个模块又可以由若干个子模块构成。其中有些模块需要综合成具体电路，而有些模块只是与用户所设计的模块由交互联系的现存电路或激励信号源。利用Verilog HDL语言结构所提供的这种功能就可以构造一个模块间的清晰层次结构，以此来描述及其复杂的大型设计，并对所作设计的逻辑电路进行严格的验证。

Verilog HDL行为描述语言作为一种结构化和过程性的语言，其语法结构非常适合于算法级和RTL级的模型设计。这种行为描述语言具有以下功能:

> + 可描述顺序执行或并行执行的程序结构；
> + 用延迟表达式或事件表达式来明确地控制过程的启动时间；
> + 通过命名地事件来触发其他过程里的激活行为或停止行为；
> + 提供了可带参数且非零延续时间的任务(task)程序结构;
> + 提供了可定义新的操作符的函数结构(function);
> + 提供了可用于建立表达式的算数运算符、逻辑运算符、位运算符;
> + Verilog HDL语言作为一种结构化的语言非常适用于门级和开关级的模型设计。因其结构化的特点又使它具有以下功能:
>> + 提供了一套完整的表示组合逻辑的基本元件的原语(primitive);
>> + 提供了双向通路(总线)和电阻器件的原语;
>> + 可建立MOS器件的点和分享和电荷衰减动态模型。

Verilog HDL的构造性语句可以精确地建立信号地模型。这是因为在Verilog HDL中，提供了延迟和输出强度地原语来建立精确程度很高地信号模型。信号值可以有不同的强度，可以通过设定宽范围地模糊值来降低不确定条件地影响。

Verilog HDL作为一种高级地硬件描述编程语言，与C语言地风格有许多类似之处。其中有许多语句，如if语句、case语句和C语言中地对应语句十分相似。

## 2.1 Verilog模块的基本概念
下面先介绍几个简单的Verilog HDL程序，从中了解Verilog模块的特性。

二选一多路选择器:
```verilog
module muxtwo(out, a, b, sl);
    input   a, b, sl;
    output  out;
    reg out;
        always @ (sl or a or b)
            if (!sl) out = a;
                else out = b;
```

输出out与输入a一致，还是与输入b一致，由sl的电平决定。当控制信号sl为非(低电平0)时，输出out与输入a相同，否则与b相同。

`always @ (sl or a or b)`表示只要sl或a或b，其中有一个裱花时就执行下面的语句。人们并不关心它的电路结构，关心的是如何从逻辑功能上来描述它。

为了实现这个电路的逻辑功能,也能用布尔表达式来描述,Verilog中，可以用`~ & |`操作符分别表示:求反、相与和相或。

上述代码也可以用下面的代码实现:
```verilog
module muxtwo(out, a, b, sl);
    input a, b, sl;
    output out;
    wire nsl, sela, selb;
    assign nsl = ~sl;
    assign sela = a & nsl;
    assign selb = b & sl;
    assign out = sela | seilb;
endmodule
```

## 2.2 Verilog用于模块的测试
Verilog还可以用来描述变化的测试信号。描述测试信号的变化和测试过程的模块也称为测试平台(testbench或testfixture)，它可以对电路模块(无论是行为的或结构的)进行动态的全面测试。通过观测被测试模块的输出信号是否符合要求，可以调试和验证逻辑系统的设计和结构正确与否，并发现问题及时修改。

## 2.3 小结
> + (1) Verilog HDL程序是由模块构成的。每个模块的内容都是位于module和endmodule两个语句之间。每个模块实现特定的功能。
> + (2) 模块是可以进行层次嵌套的。正因为如此，才可以将大型的数字电路设计分割成不同的小模块来实现特定的功能。
> + (3) 如果每个模块都是可以综合的，则通过综合工具把他们的功能描述全都转换为最基本的逻辑单元描述，最后可以用一个上层模块通过实例引用把这些模块连接起来，把它们整合成一个很大的逻辑系统。
> + (4) Verilog模块可以分为两种类型:一种是为了让模块最终能生成电路的结构，另一种只是测试所设计的电路的逻辑功能是否正确。
> + (5) 每个模块要进行端口定义，并说明输入、输出口，然后对模块功能进行描述。
> + (6) Verilog HDL程序的书写格式自由，一行可以写几个语句，一个语句也可以分写多行。
> + (7) 除了endmodule语句外，每个语句和数据定义的最后必须由分号。
> + (8) 可以用/*···*/和//···对Verilog HDL程序的任何部分作注释。一个好的、有使用价值的源程序都应当加上必要的注释，以增强程序的可读性和可维护性。
