# 单元测试

> Todo: 
>
> 待添加stub、mock代码的解释、实例。



## 什么是单元测试？

单元测试是指，对软件中的最小可测试单元在与程序其他部分相隔离的情况下进行检查和验证的工作，这里的最小可测试单元通常是指函数或者类。

单元测试通常由开发工程师完成，并以自动化的方式执行。在大量回归测试的场景下更能带来高收益。



## 为什么要写单元测试？

- 写单元测试，是为了验证某段代码的行为和开发者所期望的是否一致。通过单元测试使得程序单元变得更可靠，有助于集成。

- 一旦变更导致错误发生，借助于单元测试可以快速定位并修复错误，减少花在调试的时间。作为回归测试，防止倒退，尤其是有大量回归测试的情况下高收益。

- 对函数以不同输入参数组合调用，构成了函数的使用说明，帮助他人充分理解API的调用。

- 帮助开发工程师改善代码的设计与实现：为了使得程序模块具有可测试性而分离修改接口，使方法职责单一，有助于解耦，提高代码质量，也提高开发者能力。



## 如何做好单元测试？

测试的对象是代码，所以我们先考虑代码的基本特征与产生错误的原因，然后掌握单元测试的相关方法。

### 1. 代码的基本特征和产生错误的原因

代码中会有条件分支、循环处理、函数调用等逻辑控制，本质是对数据进行分类，每一次判定都是一次分类。所以要做到代码功能逻辑正确，需要做到分类正确并且完备无遗漏，同时每个分类的逻辑处理必须正确。

开发的时候，工程师一般会这么考虑：

1. 如果要事先正确的功能逻辑，会有哪些正常的输入；
2. 是否有需要特殊处理的多种边界输入；
3. 各种潜在非法输入的可能性以及如何处理。

这些开发工程师眼中的“功能点”，就是单元测试的”等价类“。

### 2. 单元测试用例详解

单元测试的用例是一个"输入数据"和“预计输出”的集合。针对确定的输入，根据逻辑功能推算出预期正确的输出，然后执行被测试代码拿到输出，将两者对比。

**并非只有被测试函数的输入参数是”输入数据“，常见的“输入数据”还有：**

- 被测函数内部需要读取的全局静态变量
- 被测函数内部需要读取的成员变量
- 函数内部调用子函数获得的数据
- 函数内部调用子函数改写的数据
- 嵌入式系统中，在中断调用时改写的数据
- ....

**预计输出并非只有函数返回值，还包括函数执行完成后所改写的所有数据：**

- 被测函数的返回值
- 被测函数的输出参数
- 被测函数所改写的成员变量
- 被测该函所改写的全局变量
- 被测函数中进行的文件更新
- 被测函数中进行的数据库更新
- 被测函数中进行的消息队列更新
- ……

### 3. 单元测试的相关方法

> 什么是驱动代码、桩代码和Mock代码？

#### 驱动代码(Driver)

指调用被测试函数的代码，驱动模块一般包括三个步骤：调用被测试函数前的数据准备、调用被测函数以及验证相关结果。

#### 桩代码(Stub) 

指用来代替真实代码的临时代码，是替换一部分功能的程序段。比如，某个函数 A 的内部实现中调用了一个尚未实现的函数 B，为了对函数 A 的逻辑进行测试，那么就需要模拟一个函数 B，这个模拟的函数 B 的实现就是所谓的桩代码。

https://en.wikipedia.org/wiki/Method_stub

https://en.wikipedia.org/wiki/Test_stub

#### Mock 代码：

https://en.wikipedia.org/wiki/Mock_object

#### Stub 和 Mock 的区别

https://en.wikipedia.org/wiki/Test_double

stub 用于为测试代码提供“间接输入”

mock 用于验证测试代码的“间接输出”，首先在执行测试代码之前定义期望

> Meszaros uses the term **Test Double** as the generic term for any kind of pretend object used in place of a real object for testing purposes. The name comes from the notion of a Stunt Double in movies. (One of his aims was to avoid using any name that was already widely used.) Meszaros then defined five particular kinds of double:
>
> - **Dummy** objects are passed around but never actually used. Usually they are just used to fill parameter lists.
> - **Fake** objects actually have working implementations, but usually take some shortcut which makes them not suitable for production (an [in memory database](https://www.martinfowler.com/bliki/InMemoryTestDatabase.html)is a good example).
> - **Stubs** provide canned answers to calls made during the test, usually not responding at all to anything outside what's programmed in for the test.
> - **Spies** are stubs that also record some information based on how they were called. One form of this might be an email service that records how many messages it was sent.
> - **Mocks** are what we are talking about here: objects pre-programmed with expectations which form a specification of the calls they are expected to receive.
>
> Of these kinds of doubles, only mocks insist upon behavior verification. The other doubles can, and usually do, use state verification. Mocks actually do behave like other doubles during the exercise phase, as they need to make the SUT believe it's talking with its real collaborators - but mocks differ in the setup and the verification phases.
>
> https://www.martinfowler.com/articles/mocksArentStubs.html#TheDifferenceBetweenMocksAndStubs



## 好的单元测试有什么特性？

- 自动化
  - 调用测试自动化：对于测试需要的任何条件，都应该让它们成为测试自身的一个自动化组成部分
  - 检查结果自动化：让测试自己判断结果通不通过
- 彻底的：测试了所有可能会出问题的情况
- 可重复：测试应该能以任意的顺序一次又一次地运行，并且产生相同的结果
- 独立的：任何单个测试可以独立运行，互相之间没有依赖

- 可维护：如同产品代码一样，遵守普遍的设计原则（封装、降低耦合、DRY等）

> 五大规则(F.I.R.S.T) 来源于《clean code》
>
> 快速：就是代码质量要好，高效
> 独立：不同测试之间应相互独立
> 可重复：就是在各种环境中都可测试通过
> 自足验证：测试不依赖手工操作来知晓是否通过
> 及时：测试应在生产代码之前进行编写



## 测试理念

- 花合理时间抓到大多数 bug: 对于不大可能包含 bug 的情况，就不要去测试了，如简单的访问属性函数（get**）可以不做测试，因为这些函数的错误编译器可以捕获，但如果访问函数当中做了一些其他的操作，可能就值得测试了。
- 如果有 bug 通过了现有测试网，你需要增加新的测试，以在下一次抓住它。因为它可能再次发生，而我们不想花时间去再次追踪。
- 对测试进行测试：在你编写了一个测试，用以检测特定的 bug 时，故意引发 bug ,并确定测试会发出提示。这可以确保测试在 bug 真的出现时抓住它。
- 以测试代码的心态而非写 sample 的心态，不要下意识避开那些薄弱的地方，不要温和地测试。
- bug 并不是均匀分布于源代码之中，相反，它们更倾向于扎堆于一堆问题区域之中，找出这部分代码，重新设计和编写它们。



## 什么时候写单元测试？

- bug 被发现得早，进行修补的成本就越低。

- 单元测试也有时间和人力开销，但延后测试，复杂度的增加使得效率和生产率下降更多。编写这些测试代码所花的时间从长远来看是值得的，最后会便宜得多。

- TDD：先编写单元测试用例，再开发程序单元。编码开始之前编写测试用例，同编码之后编写测试用例相比较，前者可以缩短缺陷-侦测-调试-修正这一周期。

- 编一点，测一点。持续集成。

- 对于每个新写的函数，在其他代码使用这个函数并对它形成依赖之前，都要先做单元测试。确保所有的单元测试都能够通过，而不只是新写的测试能够通过。



##  Base Practice

- 只有底层模块或者核心模块的测试才会采用单元测试
- 单元测试框架的选型和开发语言直接相关：[List of unit testing frameworks](https://en.wikipedia.org/wiki/List_of_unit_testing_frameworks) 还需要对桩代码框架和 Mock 代码框架选型，一般选型有测试架构师和开发架构师决定
  - 比如Java – Junit, TestNG; C/C++ – CppTest, Parasoft
- 引入代码覆盖率统计工具，比如 Java 的 JaCoCo，JavaScript 的 Istanbul
- 将单元测试、代码覆盖率统计一同做到持续集成的流水线上
- 在 [C++测试框架 googletest 的使用](./gtest_usage.md) 中我整理了相关的入门笔记。



## 做单元测试中，可能遇到的问题：

- 紧密耦合的代码难以隔离；
- 隔离后编译链接运行困难；
- 代码本身的可测试性较差，通常代码的可测试性和代码规模成正比；
- 无法通过桩代码直接模拟系统底层函数的调用；
- 代码覆盖率越往后越难提高。



## 参考

- [什么是单元测试？如何做好单元测试？](https://time.geekbang.org/column/article/10275)
- 《单元测试之道》
- 《程序员修炼之道——从小工到专家》
- 《重构——改善既有代码的设计》