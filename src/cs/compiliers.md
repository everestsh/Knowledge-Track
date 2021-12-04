# 编译原理

> 逆水行舟，鱼翔浅底。

## 通用理论/导论

- [ ] [LLVM](https://aosabook.org/en/llvm.html)：这是《[开源应用程序架构](https://aosabook.org/en/index.html)》一书中的一章，由 Chris Lattner 编写，涵盖了 LLVM 架构设计
- [ ] [编译器（Compilers）](https://online.stanford.edu/courses/soe-ycscs1-compilers)：该课程由斯坦福大学出品，Alex Aiken 教授。在本课程中，从头开始为真正的编程语言构建一个编译器。它涵盖了整个编译流程：解析(parsing)、类型检查(type-checking)、优化(optimizations)、代码生成(code generation)。除了实践部分，它还深入研究了理论。
- [ ] [自动机理论（Automata Theory）](https://online.stanford.edu/courses/soe-ycsautomata-automata-theory)：该课程由由斯坦福大学出品， Jeffrey Ullman 教授。十分偏重理论基础。它从相对简单的主题开始，例如状态机和有限自动机。逐渐转向更复杂的事物，如图灵机、计算复杂性、著名的 P vs. NP 问题等。
- [ ] [计算理论（Theory of Computation）](https://ocw.mit.edu/courses/mathematics/18-404j-theory-of-computation-fall-2020/)：本课程由麻省理工大学出品，Michael Sipser 教授。它与上面的类似，但以不同的方式讲授，更详细地介绍了特定的主题。

## 前端（Front-end）

编译器前端是与实际源代码进行交互的地方。编译器将源代码解析为抽象语法树 (AST)，进行语义分析和类型检查，并将其转换为中间表示 (IR)。

以下是 Clang 相关：

- [ ] [理解 Clang 抽象语法树（Understanding the Clang AST）](https://jonasdevlieghere.com/understanding-the-clang-ast/): 本文由 Jonas Devlieghere 撰写。它详细介绍了 Clang 的 AST 实现细节。也有很多很好的资源链接可以深入研究这个主题。
- [ ] [clang-tutor](https://github.com/banach-space/clang-tutor/)：该仓库由 Andrzej Warzyński 维护。它包含多个 Clang 插件，涵盖各种主题，从简单的 AST 遍历到更复杂的主题，例如自动重构和混淆。

## 中端（Middle-End）

中端是进行各种优化的地方。通常，中端使用一些中间表示。 LLVM 的中间表示通常称为 LLVM IR 或 LLVM Bitcode。简而言之，它是一种用于伪机器的人类可读的汇编语言（即 IR 不针对任何特定的 CPU）。LLVM IR 保持某些属性：它采用静态单分配 (SSA) 形式，组织为控制流图 (CFG)。

- [ ] [LLVM IR Tutorial - Phis, GEPs and other things, oh my!](https://www.youtube.com/watch?v=m8G_S5LwlTo). 这是 Vince Bridgers 和 Felipe de Azevedo Piovezan 的精彩演讲。
- [ ] [Introduction to LLVM](https://www.youtube.com/watch?v=J5xExRGaIIY). 来自 Eric Christopher 和 Johannes Doerfert 的 LLVM 开发者会议的一小时演讲/教程。
- [ ] [CS 6120: Advanced Compilers](https://www.cs.cornell.edu/courses/cs6120/2020fa/self-guided/). 该课程由康奈尔大学出品，阿德里安·桑普森教授。标题说“高级”，但它涵盖了人们对现代生产级编译器的基础部分：SSA、CFG、优化、各种分析。
- [ ] [Bitcode Demystified](https://lowlevelbits.org/bitcode-demystified/)(🔌). 文章介绍了 LLVM 位码是什么的描述。
- [ ] [llvm-tutor](https://github.com/banach-space/llvm-tutor). 这个也是来自 Andrzej Warzyński。它涵盖了 LLVM 插件（所谓的 pass），允许人们分析和转换 LLVM IR 形式的程序。

## 后端（Back-end）

编译的最后一个阶段是后端。这个阶段的目标是将中间表示转换为最终机器代码（零和一）。生成的 0 和 1 可以在 CPU 上运行。因此，要了解后端，需要了解机器代码和 CPU 的工作原理。

- [ ] [依据基本原理构建现代计算机：从与非门到俄罗斯方块](https://www.coursera.org/learn/build-a-computer). 由希伯来大学出品，Shimon Schocken 和 Noam Nisan 授课。本课程是反着来的：首先，构建逻辑门（和、或、异或等），然后使用逻辑门构建算术逻辑单元 (ALU)，然后使用 ALU 构建 CPU。然后学习如何使用零和一（机器代码）控制 CPU，并最终开发汇编程序以将人类可读的程序代码转换为机器代码。
- [ ] [Parsing Mach-O files](https://lowlevelbits.org/parsing-mach-o-files/)(🔌). 文章介绍了如何在 macOS (Mach-O) 上解析目标文件。如果你使用的是 Linux 或 Windows，请分别搜索有关 elf 和 PE/COFF 文件的类似文章。
- [ ] [Performance Analysis and Tuning on Modern CPUs](https://book.easyperf.net/perf_book). 丹尼斯·巴赫瓦洛夫的书。虽然它与性能有关，但它很好地介绍了 CPU 的工作原理。

## 链接

- [LLVM’s YouTube 频道](https://www.youtube.com/channel/UCv2_41bSAa5Y_8BacJUZfjQ). 在这里可以找到很多来自开发者会议的演讲。
- [LLVM Weekly](https://llvmweekly.org/). 由亚历克斯·布拉德伯里 (Alex Bradbury) 运营的每周通讯。这是我所知道的没有广告的纯粹时事通讯！
- [LLVM 博客](https://blog.llvm.org/).
- [LLVM 教程](https://llvm.org/docs/tutorial/).
- [Embedded in academia](https://blog.regehr.org/archives/category/compilers). John Regehr 的博客有很多关于 LLVM 和编译器的好文章

- [x] [HOW TO LEARN COMPILERS: LLVM EDITION](https://lowlevelbits.org/how-to-learn-compilers-llvm-edition/) - 编译器学习路径推荐