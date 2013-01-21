# Selenium 1 (Selenium RC)¶

## 介绍

正如你在 Selenium 项目简史里读到的，Selenium RC 在很长一段时间内都是 Selenium 的主要项目，直到 WebDriver/Selenium 合并而产生了最新和最强大的 Selenium 2。

Selenium 仍然被活跃的支持（大部分是维护工作），并且提供了一些 Selenium 2 短期不会支持的特性，包括支持多语言 (Java, Javascript, Ruby, PHP, Python, Perl 和 C#) 和支持几乎所有的浏览器。 

## Selenium RC 如何工作

首先，我们将讲述 Selenium RC 的组件如何操作，以及在测试脚本运行时各自扮演的角色。

### RC 组件

Selenium RC 组件是：

Selenium Server 能启动和杀死浏览器进程，解析并运行由测试程序传递过来的 Selenese 命令，并且可以是一个 HTTP 代理，拦截和验证浏览器和 AUT(测试中的应用)之间的 HTTP 通信。

客户端库提供了各种编程语言和 Selenium RC Server 之间的接口。

以下是一个简单的架构图：

![架构图](http://seleniumhq.org/docs/_images/chapt5_img01_Architecture_Diagram_Simple.png)

上图演示了客户端和服务端进行通信以传递要执行的 Selenium 命令。然后服务端使用 Selenium-Core JavaScript 命令将 Selenium 命令传递给浏览器。浏览器则使用其内置的 JavaScript 解析器来执行 Selenium 命令。这样运行 Seleniun 动作或者验证你指定的测试脚本。

### Selenium 服务端

Selenium 服务端从你的测试程序接收 Selenium 命令，解析它们，并且反馈给你程序的测试执行结果。

RC 服务端绑定了 Selenium Core 并且自动将其注入浏览器。这在你的测试程序打开浏览器时发生（使用客户端库的方法）。Selenium-Core 是一个 JavaScript 程序，实际上是一些利用浏览器的内置 JavaScript 解析器解析和实行 Selenese 命令 的 JavaScript 函数。

Server 使用简单的 HTTP GET/POST 请求来接收你的测试程序中的 Selenese 命令。这意味这你可以使用任何可以发送 HTTP 请求的编程语言来实现 Selenium 测试在浏览器中的自动运行。

### 客户端库

客户端库提供了能让你从自定义的程序中运行 Selenium 命令的编程支持。每种支持的语言都有一个不同的客户端库。Selenium 客户端库提供了一组接口，例如一些从你的程序中运行 Selenium 命令的方法。通过实现这些接口，我们就能得到一个支持所有 Selenese 命令的编程方法。

客户端库将 Selenese 命令传递给 Selenium 服务端来处理一个特定的动作或者执行 AUT 的测试。客户端库同时接收所传递命令的执行结果，并将其返回给你的程序。你的程序可以接收这个结果并且将其存储到一个变量中，然后报告其运行结果是成功还是失败，或者当其发生错误是进行适当的处理。

因此要创建一个测试程序，你仅仅需要使用客户端库的 API 来编写一个可以运行 Selenium 命令的程序。或者，如果你已经有了使用 Selenium-IDE 创建的 Selenium 测试脚本，你可以使用它来生成 Selenium RC 代码。Selenium-IDE 可以将 Selenium 命令转换（使用导出菜单）成客户端 API 的方法调用。查看 Selenium-IDE 章节中关于从 Selenium-IDE 中导出 RC 代码的细节。

## 安装

用安装这个词不是很恰当。Selenium 在你选择的编程语言中有一组组件可用。你可以从下载页面下载它们。

一旦你选定了一种编程语言，你仅需要：

- 安装 Selenium RC 服务端。
- 使用特定于该语言的客户端驱动创建你的项目

### 安装 Selenium 服务端

Selenium RC 服务端是一个简单的 jar 包 (selenium-server-standalone-<version-number>.jar)，它不需要安装。只需要下载这个zip文件，并提取服务所需的目录即可。

### 运行 Selenium 服务

在开始任何测试之前，你必须先启动服务。进到 Selenium RC 服务端所在的目录，并在命令行中运行以下命令：

    java -jar selenium-server-standalone-<version-number>.jar
    

你也可以简单的创建一个包含上述命令的批处理或shell文件（Windows 中扩展名为 .bat，Linux 中扩展名为 .sh）。然后在你的桌面上创建一个该可执行文件的快捷方式，通过双击图标来启动服务。

要成功启动服务必须确保 Java 已安装，并且设置了正确的 PATH 环境变量。你可以通过下面的命令检查你的 Java 是否安装正确：

    java -version

如果你得到一个版本号（必须>=1.5），那么你已经成功启动 Selenium RC。

### 使用 Java 客户端驱动

- 从 SeleniumHQ 下载页面下载 Selenium java 客户端驱动 zip 包。
- 提取 selenium-java-<version-number>.jar
- 打开你喜欢的 Java IDE (Eclipse, NetBeans, IntelliJ, Netweaver, etc.)
- 创建一个 java 项目。
- 将 selenium-java-<version-number>.jar 文件作为引用添加到你的项目中。
- 将 selenium-java-<version-number>.jar 文件添加到你项目的 classpath 中。
- 从 Selenium-IDE 到处一个 Java 文件，并放入你的项目，或者使用 Selenium 的 Java 客户端 API 编写一个 Selenium 测试文件。这些 API 将在本章的后面部分进行讲解。你可以使用 JUnit，或者 TestNg 来运行你的测试，或者你可以简单的写一个 main() 方法。这些概念也将在本文后面进行说明。
- 从命令行运行 Selenium 服务。
- 从 Java IDE 或者命令行中执行你的测试。

关于更多 Java 测试项目的配置细节，可查看本章附件：**在 Eclipse 中配置 Selenium RC** 和 **在 Intellij 中配置 Selenium RC**。

## 将 Selenese 转换成程序

使用 Selenium RC 的主要任务就是将你的 Selenese 转换成一个编程语言。在本小结中，我们提供几种不同的语言演示。

### 测试脚本范例

让我们从一个 Selenese 测试脚本的例子开始. 假定我们使用 Selenium-IDE 记录了如下测试：

<table>
    <tbody>
        <tr>
            <td>open</td>
            <td>/</td>
            <td>&nbsp;</td>
        </tr>
        <tr>
            <td>type</td>
            <td>q</td>
            <td>selenium rc</td>
        </tr>
        <tr>
            <td>clickAndWait</td>
            <td>btnG</td>
            <td>&nbsp;</td>
        </tr>
        <tr>
            <td>assertTextPresent</td>
            <td>Results * for selenium rc</td>
            <td>&nbsp;</td>
        </tr>
    </tbody>
</table>

注意: 这个例子仅仅在 Google 搜索页面 http://www.google.com 工作。

## Selenese 作为编程代码

以下为使用支持的多种编程序言从 Selenium-IDE 中导出的测试脚本。如果你有一些面向对象编程的基础知识，你就可以通过阅读以下代码理解 Selenium 如何运行 Selenese 命令。

    /** Add JUnit framework to your classpath if not already there
     *  for this example to work
     */
    package com.example.tests;
    
    import com.thoughtworks.selenium.*;
    import java.util.regex.Pattern;
    
    public class NewTest extends SeleneseTestCase {
        public void setUp() throws Exception {
            setUp("http://www.google.com/", "*firefox");
        }
          public void testNew() throws Exception {
              selenium.open("/");
              selenium.type("q", "selenium rc");
              selenium.click("btnG");
              selenium.waitForPageToLoad("30000");
              assertTrue(selenium.isTextPresent("Results * for selenium rc"));
        }
    }

在接下来的章节中，我们将介绍如何通过生成的代码创建你的测试程序。






















