# 介绍

## 用于网站应用的测试自动化

如今，大多数软件应用都是跑在浏览器中的网站应用。不同公司和组织之间的测试效率迥异。在这个富交互和响应式处理随处可见的时代，很多组织都使用敏捷的方式来开发，因此测试自动化也成为软件项目的必备部分。测试自动化意味着使用软件工具来反复运行项目中的测试，并为回归测试提供反馈。

测试自动化有很多优点。大多数都和测试的可重复性和高执行效率这两点相关。市面上有一些商业或开源的同居来辅助测试自动化开发。Selenium 应该是最广泛使用的开源方案。本文档将帮助新手和有经验的使用者学习为网站应用创建测试自动化的有效技术。

本文档介绍了 Selenium，其细节和从社区中积累的最佳实践。其中包含很多范例。同时，也将提及 Selenium 的一些技术细节和推荐用法。

对于一个软件团队的测试过程来说，测试自动化具有提高长期效率的优势。测试自动化包括：

- 频繁的回归测试
- 快速反馈
- 几乎无限制的测试用例迭代执行
- 支持敏捷和极限编程
- 遵循测试用例的文档
- 自定义缺陷报告
- 能找出手工测试中没发现的缺陷

## 自动化？或不自动化？

自动化是否总是好的？什么时候我们应该使用自动化的方式来测试？

自动化测试不总是有优势的。这里有一些场景就更适合手工测试。例如，如果一个应用的接口在不久的将来会发生变化，那时所有的测试用例都需要重写。有时仅仅是因为没有足够的时间来实现测试自动化。短期来说，手工测试更快捷。如果一个应用的发布日是掐死的，而又没有可用的自动化测试，而测试工作又必须在指定时间内完成，那么此时手工测试也是最佳选择。

## 介绍 Selenium

Selenium 是一组软件工具集,每一个都有不同的方法来支持测试自动化。大多数使用 Selenium 的QA工程师只关注一两个最能满足他们的项目需求的工具上。然而，学习所有的工具你将有更多选择来解决不同类型的测试自动化问题。这一整套工具具备丰富的测试功能，很好的契合了测试各种类型的网站应用的需要。这些操作非常灵活，有多种选择来定位 UI 元素，同时将预期的测试结果和实际的行为进行比较。Selenium 一个最关键的特性是支持在多浏览器平台上进行测试。

## Selenium 项目简史

Selenium 诞生于 2004 年，当在 ThoughtWorks 工作的 Jason Huggins 在测试一个内部应用时。作为一个聪明的家伙，他意识到相对于每次改动都需要手工进行测试，他的时间应该用得更有价值。他开发了一个可以驱动页面进行交互的 Javascript 库，能让多浏览器自动返回测试结果。那个库最终变成了 Selenium 的核心，它是 Selenium RC（远程控制）和 Selenium IDE 所有功能的基础。Selenium RC 是开拓性的，因为没有其他产品能让你使用自己喜欢的语言来控制浏览器。

Selenium 是一个庞大的工具，所以它也有自己的缺点。由于它使用了基于 Javascript 的自动化引擎，而浏览器对 Javascript 又有很多安全限制，有些事情就难以实现。更糟糕的是，网站应用正变得越来越强大，它们使用了新浏览器提供的各种特性，都使得这些限制让人痛苦不堪。

在 2006 年，一名 Google 的工程师， Simon Stewart 开始基于这个项目进行开发，这个项目被命名为 WebDriver。此时，Google 早已是 Selenium 的重度用户，但是测试工程师们不得不绕过它的限制进行工具。Simon 需要一款能通过浏览器和操作系统的本地方法直接和浏览器进行通话的测试工具，来解决Javascript 环境沙箱的问题。WebDriver 项目的目标就是要解决 Selenium 的痛点。

跳到 2008 年。北京奥运会的召开显示了中国在全球的实力，大规模的次贷危机引发了“大萧条”以来美国最大的经济危机。但是当年最重要的故事是 Selenium 和WebDriver 的合并。Selenium 有着丰富的社区和商业支持，但 WebDriver 显然代表着未来的趋势。两者的合并为所有用户提供了一组通用功能，并且借鉴了一些测试自动化领域最闪光的思想。或许，关于两者合并的最好解释，是由 WebDriver 的开发者，在 2009 年 8 月 6 日发出的一封给社区的联合邮件中提到的：

> 为什么这两个项目要合并？一部分是因为 WebDriver 弥补了 Selenium 的一些短处（例如提供了一组很棒的 API，绕开浏览器的限制），一部分是因为 Selenium 弥补了 WebDriver 的一些短处（例如对浏览器更广泛的支持），还有一部分是因为 Selenium 的主要贡献者和我都认为这样能为用户提供最优秀的框架。

## Selenium 工具集

Selenium 由多个软件工具组成，每个具备特定的功能。

### Selenium 2 (又叫 Selenium Webdriver)

Selenium 2 代表了这个项目未来的方向，也是最新被添加到 Selenium 工具集中的。这个全新的自动化工具提供了很多了不起的特性，包括更内聚和面向对象的 API，并且解决了旧版本限制。

正如简史中提到的，Selenium 和 WebDriver 的作者都赞同两者各具优势，而两者的合并使得这个自动化工具更加强健。

Selenium 2.0正是于此的产品。它支持WebDriver API及其底层技术，同时也在WebDriver API底下通过Selenium 1技术为移植测试代码提供极大的灵活性。此外，为了向后兼容，Selenium 2 仍然使用 Selenium 1 的 Selenium RC 接口。 

### Selenium 1 (又叫 Selenium RC 或远程控制)

正如你在简史中读到的，在很长一段时间内，Selenium RC 都是最主要的 Selenium 项目，直到 WebDriver 和 Selenium 合并而产生了最新且最强大的 Selenium 2.

Seleinum 1 仍然被活跃的支持着（更多是维护），并且提供一些 Selenium 2 短时间内可能不会支持的特性，包括对多种语言的支持(Java, Javascript, Ruby, PHP, Python, Perl and C#) 和对大多数浏览器的支持。

### Selenium IDE

Selenium IDE (集成开发环境) 是一个创建测试脚本的原型工具。它是一个 Firefox 插件，提供创建自动化测试的建议接口。Selenium IDE 有一个记录功能，能记录用户的操作，并且能选择多种语言把它们导出到一个可重用的脚本中用于后续执行。

**注意** 

虽然 Selenium IDE 有保存功能，能让用户以表格的形式保存测试，以供后续的导入和执行，但它不是用于执行你的测试是否通过，也不能创建所有你需要的自动化测试。需要注意的是，Selenium IDE 不能生成含有迭代和条件语句的测试脚本。在本文档编写时也没有要实现该功能的计划。这部分是因为技术原因，部分是因为 Selenium 的开发者所推荐的自动化测试的最佳实践常常是需要编写一些代码的。Selenium IDE 只是被设计为一个快速的原型工具。Selenium 的开发者推荐选用支持的最好的语言来创建严谨、健壮的测试，不管是使用 Selenium 1 还是 Selenium 2.

### Selenium-Grid

Selenium-Grid 使得 Selenium RC 解决方案能提升针对大型的测试套件或者哪些需要运行在多环境的测试套件的处理能力。Selenium Grid 能让你并行的运行你的测试，也就是说，不同的测试可以同时跑在不同的远程机器上。这样做有两个有事，首先，如果你有一个大型的测试套件，或者一个跑的很慢的测试套件，你可以使用 Selenium Grid 将你的测试套件划分成几份同时在几个不同的机器上运行，这样能显著的提升它的性能。同时，如果你必须在多环境中运行你的测试套件，你可以获得多个远程机器的支持，它们将同时运行你的测试套件。在每种情况下，Selenium Grid 都能通过并行处理显著地缩短你的测试套件的处理时间。

## 选择合适你的 Selenium 工具

很多人都从 Selenium IDE 开始学习使用，如果你不是特别善于编程或者编写一门脚本语言，你可以通过使用 Selenium IDE 来熟悉 Selenium 命令。使用 IDE，你能在很短的时间内（有时是数秒）创建简单的测试。

但是我们不推荐你使用 Selenium IDE 来处理所有的测试自动化工作。更高效的做法是，你需要使用它支持的语言创建和运行你的测试，无论是 Selenium 1 还是 Selenium 2。至于选择什么语言则取决于你的喜好。

在编写本文档时，Selenium 的开发者认为Selenium-WebDriver API 才是 Selenium未来的趋势。但Selenium 1 提供向后兼容。同时，我们也在之前讨论了两者的优势和劣势。

我们强烈建议那些初次接触 Selenium 的用户通读这这个章节的内容。那些第一次使用 Selenium ，随意创建了一些测试套件的用户，你通常会希望从 Selenium 2 开始，因为这部分是 Selenium 在将来都会持续支持的。

## 支持的浏览器和平台

在 Selenium 2.0 中，支持的浏览器完全取决于你是否使用 Selenium-WebDriver 或 Selenium-RC。

### Selenium-WebDriver

Selenium-WebDriver 支持如下浏览器，在所有支持这些浏览器的操作系统中能都运行良好。

- Google Chrome 12.0.712.0+
- Internet Explorer 6, 7, 8, 9 - 32 and 64-bit where applicable
- Firefox 3.0, 3.5, 3.6, 4.0, 5.0, 6, 7
- Opera 11.5+
- HtmlUnit 2.9
- Android – 2.3+ for phones and tablets (devices & emulators)
- iOS 3+ for phones (devices & emulators) and 3.2+ for tablets (devices & emulators)

**注意：** 

在写本文档的时候，一款 Android 2.3 的模拟器被报有bug。但是在 tablet 模拟器和真实设备中均工作良好。

### Selenium 1.0 and Selenium-RC

这里是指老的，支持 Selenium 1 的部分。它也适用于 Selenium 2 版本中的 Selenium RC。

<table>
    <tbody>
    <tr>
        <td><strong>Browser</strong></td>
        <td><strong>Selenium IDE</strong></td>
        <td><strong>Selenium 1 (RC)</strong></td>
        <td><strong>Operating Systems</strong></td>
    </tr>
    <tr>
        <td>Firefox 3.x</td>
        <td>Record and playback tests</td>
        <td>Start browser, run tests</td>
        <td>Windows, Linux, Mac</td>
    </tr>
    <tr>
        <td>Firefox 3</td>
        <td>Record and playback tests</td>
        <td>Start browser, run tests</td>
        <td>Windows, Linux, Mac</td>
    </tr>
    <tr>
        <td>Firefox 2</td>
        <td>Record and playback tests</td>
        <td>Start browser, run tests</td>
        <td>Windows, Linux, Mac</td>
    </tr>
    <tr>
        <td>IE 8</td>
        <td>Test execution only via Selenium RC*</td>
        <td>Start browser, run tests</td>
        <td>Windows</td>
    </tr>
    <tr>
        <td>IE 7</td>
        <td>Test execution only via Selenium RC*</td>
        <td>Start browser, run tests</td>
        <td>Windows</td>
    </tr>
    <tr>
        <td>IE 6</td>
        <td>Test execution only via Selenium RC*</td>
        <td>Start browser, run tests</td>
        <td>Windows</td>
    </tr>
    <tr>
        <td>Safari 4</td>
        <td>Test execution only via Selenium RC</td>
        <td>Start browser, run tests</td>
        <td>Windows, Mac</td>
    </tr>
    <tr>
        <td>Safari 3</td>
        <td>Test execution only via Selenium RC</td>
        <td>Start browser, run tests</td>
        <td>Windows, Mac</td>
    </tr>
    <tr>
        <td>Safari 2</td>
        <td>Test execution only via Selenium RC</td>
        <td>Start browser, run tests</td>
        <td>Windows, Mac</td>
    </tr>
    <tr>
        <td>Opera 10</td>
        <td>Test execution only via Selenium RC</td>
        <td>Start browser, run tests</td>
        <td>Windows, Linux, Mac</td>
    </tr>
    <tr>
        <td>Opera 9</td>
        <td>Test execution only via Selenium RC</td>
        <td>Start browser, run tests</td>
        <td>Windows, Linux, Mac</td>
    </tr>
    <tr>
        <td>Opera 8</td>
        <td>Test execution only via Selenium RC</td>
        <td>Start browser, run tests</td>
        <td>Windows, Linux, Mac</td>
    </tr>
    <tr>
        <td>Google Chrome</td>
        <td>Test execution only via Selenium RC</td>
        <td>Start browser, run tests</td>
        <td>Windows, Linux, Mac</td>
    </tr>
    <tr>
        <td>Others</td>
        <td>Test execution only via Selenium RC</td>
        <td>Partial support possible**</td>
        <td>As applicable</td>
    </tr>
    </tbody>
</table>

\* 在 Firefox 上通过 Selenium IDE 开发的测试，可以通过简单的 Selenium RC 命令行在任意支持的浏览器上运行。

\*\* Selenium RC 服务器能开启任何可运行的测试。但根据浏览器的安全设置，可能会有部分特性不可用。

## 灵活性和可扩展性

你将发现 Selenium 是高度灵活的。你有很多方式为 Selenium 的测试脚本和 Selenium 框架添加功能来定制你的自动化测试。同其他的自动化工具相比，Selenium 可能是最强的。定制相关的内容贯穿整个文档有多处提及。另外，Selenium 是开源的，它的源码可以下载和修改。

## 本文档包含哪些内容？

本文档同时面向于新手和那些希望了解更多的 Selenium 用户。我们向新手介绍 Selenium，我们并不要求你对 Selenium 非常了解，但你至少需要知道一些自动化测试的基本知识。对于那些经验丰富的用户来说，本文档可作为一个使用参考。如果真的非常熟悉，我们建议你浏览一下每个章节和其副标题。我们提供了 Selenium 的架构信息，常见用法的例子和一章关于测试设计的内容。

剩下的章节将讲述以下内容:

### Selenium IDE

介绍 Selenium IDE 以及如何使用它创建测试脚本。如果你缺乏编程经验，但仍然希望学习测试自动化，那么从本章开始入手是个不错的主意，并且你将会发现自己能通过 Selenium IDE 创建不少的测试用例。如果你编程经验丰富，但你希望使用 Seleinum IDE 快速创建测试原型的话，这一章对你也很有用。本章还将向你演示如何导出指定语言的测试脚本，以添加更多 Selenium IDE 不能支持的功能。

### Selenium 2

解释如何通过 Selenium 2 创建自动化测试项目。

### Selenium 1

解释如何通过 Selenium RC API 开发一个自动化测试项目。我们使用了多种语言来进行代码演示。同时包括了如何安装 Selenium RC 的内容。Seleium RC 支持的各种模式、配置也将会介绍，包括它们的限制和如何进行权衡。我们还提供了架构图来帮助演示这些点。对于 Selenium RC 新手来说，一些常见问题的解决方案也列在其中，例如，操作安全证书， HTTPS 请求，弹出框和打开新窗口。

### 测试设计

这个章节介绍了使用 Selenium WebDriver 和 Selenium RC 的编程技巧。我们还演示了论坛中常被问道的技巧，例如如何设计 setup 和 teardown 方法，如何实施数据驱动测试（每次测试通过数据都有数据发生变化）和其他一些常见的测试自动化任务的编程方法。

### Selenium-Grid

这部分内容未完成。

### User extensions

讲述如何修改、扩展和定制 Selenium。






