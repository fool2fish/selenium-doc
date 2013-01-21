# Selenium 1 (Selenium RC)¶

## 介绍

正如你在 Selenium 项目简史里读到的，Selenium RC 在很长一段时间内都是 Selenium 的主要项目，直到 WebDriver/Selenium 合并而产生了最新和最强大的 Selenium 2。

Selenium 仍然被活跃的支持（大部分是维护工作），并且提供了一些 Selenium 2 短期不会支持的特性，包括支持多语言 (Java, Javascript, Ruby, PHP, Python, Perl 和 C#) 和支持几乎所有的浏览器。 

## Selenium RC 如何工作

首先，我们将讲述 Selenium RC 的组件如何操作，以及在测试脚本运行时各自扮演的角色。

### RC 组件

Selenium RC 组件是：

Selenium Server 能启动和杀死浏览器进程，解析并运行由测试程序传递过来的 Selenium 命令，并且可以是一个 HTTP 代理，拦截和验证浏览器和 AUT(测试中的应用)之间的 HTTP 通信。

客户端库提供了各种编程语言和 Selenium RC Server 之间的接口。

以下是一个简单的架构图：

！[架构图](http://seleniumhq.org/docs/_images/chapt5_img01_Architecture_Diagram_Simple.png)

上图演示了客户端和服务端进行通信以传递要执行的 Selenium 命令。然后服务端使用 Selenium-Core JavaScript 命令将 Selenium 命令传递给浏览器。浏览器则使用其内置的 JavaScript 解析器来执行 Selenium 命令。这样运行 Seleniun 动作或者验证你指定的测试脚本。

