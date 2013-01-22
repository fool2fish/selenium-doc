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

## 编写你的测试代码

现在我们将为每种支持的语言演示如何通过上述例子编写你自己的测试代码。我们主要需要做2件事情：

- 从 Selenium-IDE 导出指定语言的脚本，有选择性的修改它。
- 编写一个 main() 方法来执行创建的代码。

你可以选择平台支持的任意测试引擎，如 Java 的 JUnit 或 TestNG。

这里我们将演示指定语言的例子。每种语言的 API 都有所不同，所以我们将单独解释每一个。

### Java

在 Java 中，大家通常选择 JUnit 或 TestNG 作为测试引擎。一些像 Eclipse 这样的 IDE 能通过插件直接支持它们，使得事情更简单。JUnit 和 TestNG 教学不在本文档的范围内，但是你可以通过网络找到相关资料。如果你是一个 Java 程序员，你可能已经有使用这些框架的经验了。

你可能希望为 “NewTest” 测试类重命名。同时，你可能也需要修改以下语句中的浏览器打开参数。

    selenium = new DefaultSelenium("localhost", 4444, "*iehta", "http://www.google.com/");

使用 Selenium-IDE 创建的代码看起来大致如下。为了使代码更清晰易读，我们手工加入了注释。

    
    package com.example.tests;
    // 我们指定了这个文件的包
    
    import com.thoughtworks.selenium.*;
    // 导入驱动。
    // 你将使用它来初始化浏览器并执行一些任务。
    
    import java.util.regex.Pattern;
    // 加入正则表达式模块，因为有些我们需要使用它进行校验。
    // 如果你的代码不需要它，完全可以移除掉。 
    
    public class NewTest extends SeleneseTestCase {
    // 创建 Selenium 测试用例
    
          public void setUp() throws Exception {
            setUp("http://www.google.com/", "*firefox");
                 // 初始化并启动浏览器
          }
    
          public void testNew() throws Exception {
               selenium.open("/");
               selenium.type("q", "selenium rc");
               selenium.click("btnG");
               selenium.waitForPageToLoad("30000");
               assertTrue(selenium.isTextPresent("Results * for selenium rc"));
               // 以上为真实的测试步骤
         }
    }

## 学习使用 API

Selenium RC API 使用以下约定：假设你了解 Selenese，并且大部分接口是自解释的。在此，我们仅解释最具争议或者看起来不那么直接明了的部分。

### 启动浏览器

    setUp("http://www.google.com/", "*firefox");

每个例子都打开了一个浏览器，并且将浏览器作为一个浏览器对象返回，赋值给一个变量。这个变量将用于调用浏览器方法。这些方法可以执行 Selenium 命令，例如打开、键入或者校验。

创建浏览器对象所需要的参数如下：

#### host

指定服务所在的机器的 IP 地址。通常它和运行客户端的机器是同一台。所以在这个例子中我们传入 localhost。在某些客户端中，这是一个可选参数。

#### port

指定服务监听的客户端用于创建连接的 TCP/IP socket。这在某些客户端中也是可选的。

#### browser

指定你希望运行测试的浏览器。该参数必选。

#### url

AUT 的基准 url。在所有的客户端中必选，并且是启动浏览器代理的 AUT 通讯的必须信息。

注意，有些客户端要求调用 start() 方法来启动浏览器。

### 运行 命令

一旦你初始化了一个浏览器并且将其赋值给一个变量（通常命名为 "Selenium"），你可以使用这个变量调用各种方法来运行 Selenese 命令。例如，调用 selenium 对象的键入方法：

    selenium.type(“field-id”,”string to type”)

此时浏览器将真正执行指定的操作，在这个方法调用时指定了定位符和要键入的字符串，本质上就像是一个用户在浏览器中输入了这些内容。

## 报告结果

Selenium RC 没有内置的结果报告机制。而是让你根据所选语言的特性创建符合你需求的自定义报告。这非常棒！但是你是不是希望这些事情都已经就绪，而你可以快速使用它们？其实市面上不难找到符合你需求的库或框架，这比编写你自己的测试报告代码快多了。

### 测试框架报告工具

很多语言都有对应的测试框架。它们除了提供灵活的测试引擎执行你的测试之外，通常还包括结果报告的库。例如，Java有两个常用的测试框架，JUnit 和 TestNG. .NET 也有适合它的, NUnit。

我们不会教你如何使用这些框架，那超出了本指南的范围。但我们将简单介绍一下这些框架中你可以使用的跟 Selenium 相关的特性。有很多关于学习这些测试框架的书，互联网上页有丰富的资料。

### 测试报告库

同样可以利用的是使用你所选语言编写的专门用于报告测试结果的三方库。它们通常支持多种格式，如 HTML 或 PDF。

### 最佳实践是？

大多数新接触测试框架的人将会从框架内置的报告功能开始。他们会检查任何可用库，这可比你自己开发的开销要小。当你开始使用 Selenium，毫无疑问你将开始在报告处理中使用你自己的 “print 语句”。这将可能导致你在使用一个库或框架的同时，逐渐开发开发你自己的报告功能。无论如何，在最初短暂的学习曲线之后，你将自然而然的开发出最适合你的报告功能。

### 测试报告范例

为了进行演示，我们将直接使用 Selenium 支持的语言的特定工具。以下列出的是最常用的，而且也是最为推荐的。

#### Java 中的测试报告

- 如果 Selenium 测试用例是使用 JUnit 开发的，那么 JUnit 报告就能用于创建测试报告。了解更多 [JUnit 报告](http://ant.apache.org/manual/Tasks/junitreport.html) 。
- 如果 Selenium 测试用例是使用 TestNG 开发的，那也不需要依赖外部任务来创建测试报告。TestNG 框架创建包含测试详情列表的 HTML 报告。了解更多 [TestNG 报告](http://testng.org/doc/documentation-main.html#test-results) 。
- ReportNG 是一个用于TestNG 框架的 HTML 报告插件。它的初衷是用于取代默认的 HTML 报告。ReportNG 提供了简单、彩色的测试结果显示。了解更多 [TestNG](http://reportng.uncommons.org/)
- 同时，TestNG-xslt 是一个很好的摘要报告工具。TestNG-xslt 报告看起来如下图：

    ![TestNG-xslt](http://seleniumhq.org/docs/_images/chapt5_TestNGxsltReport.png)

    了解更多 [TestNG-xslt]()

##### 记录 Selenese 命令

Logging Selenium 可以用于为你的测试创建一个含有所有 Selenium 命令及其运行结果（成功或失败）的报告。为了获得这项功能，使用 Logging Selenium 扩展你的 Java 客户端。了解更多 [Logging Selenium](http://loggingselenium.sourceforge.net/index.html)

## 为你的测试加点料

现在我们将获得所有使用 Selenium 的理由，它能为你的测试添加逻辑。就像任何程序一样。程序流通过条件语句和迭代控制。另外，你能使用 IO 来报告处理信息。在这一小结中，我们将演示一些可联合 Selenium 使用的编程语言构建例子，用以解决常见的测试问题。

当你将页面元素是否存在的简单测试转换成涉及多个网页和数据的动态功能时，你将发现你需要编程逻辑来校验期待的结果。一般的， Selenium-IDE 不支持迭代和标准的条件语句。你可以通过将 javascript 嵌入 Selenese 参数来实现条件控制和迭代，并且大部分的条件都比真正的编程语言要简单。此外，你可能需要使用异常处理来进行错误回复。基于这些原因，我们编写了这一小结内容来演示普通编程技巧的使用，以使你在自动化测试中获得更大的校验能力。

本小结例子使用 C# 和 Java 编写而成，它们非常简单，也很容易转换成其他语言。如果你有一些面向对象编程的基础知识，你将很容易掌握这个章节。

### 迭代

迭代是测试中最常用的功能了。例如你可能希望执行一个查询多次。或者你需要处理那些从数据库中返回的结果集以校验你的测试结果。

使用同之前一样的 [Google 搜索例子](http://seleniumhq.org/docs/05_selenium_rc.jsp#google-search-example)，让我们来检查搜索结果。这个测试将使用 Selenese：

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
        <tr>
            <td>type</td>
            <td>q</td>
            <td>selenium ide</td>
        </tr>
        <tr>
            <td>clickAndWait</td>
            <td>btnG</td>
            <td>&nbsp;</td>
        </tr>
        <tr>
            <td>assertTextPresent</td>
            <td>Results * for selenium ide</td>
            <td>&nbsp;</td>
        </tr>
        <tr>
            <td>type</td>
            <td>q</td>
            <td>selenium grid</td>
        </tr>
        <tr>
            <td>clickAndWait</td>
            <td>btnG</td>
            <td>&nbsp;</td>
        </tr>
        <tr>
            <td>assertTextPresent</td>
            <td>Results * for selenium grid</td>
            <td>&nbsp;</td>
        </tr>
    </tbody>
</table>

同样的代码重复跑了3次。将同样的代码拷贝多次运行可不是一个好的编程实践，因为维护的时候成本会很高。使用编程语言，我们可以通过迭代这一更灵活更易于维护的方式来处理搜索结果。

### In Csharp

    // Collection of String values.
    String[] arr = {"ide", "rc", "grid"};
    
    // Execute loop for each String in array 'arr'.
    foreach (String s in arr) {
        sel.open("/");
        sel.type("q", "selenium " +s);
        sel.click("btnG");
        sel.waitForPageToLoad("30000");
        assertTrue("Expected text: " +s+ " is missing on page."
        , sel.isTextPresent("Results * for selenium " + s));
    }

### 条件语句

我们使用一个例子来演示条件语句的使用。让运行 Selenium 测试时，如果一个原本应该存在的元素没有出现在页面上时，将会触发一个普通的错误。例如，我们运行如下 代码：

    // Java
    selenium.type("q", "selenium " +s);
    
如果元素“q”不在页面上将会抛出一个异常：

    com.thoughtworks.selenium.SeleniumException: ERROR: Element q not found

这个异常将会终止你的测试。对于某些测试来说这正是你想要的。但是更多的时候，你并不希望这样，因为还有很多后续的测试要执行。

一个更好的解决办法是我们首先判定元素是否存在，然后再进行相应的处理。我们来看看 Java 的写法：

    // 如果元素可用，则则行类型判定操作
    if(selenium.isElementPresent("q")) {
        selenium.type("q", "Selenium rc");
    } else {
        System.out.printf("Element: " +q+ " is not available on page.")
    }

这样做的好处是，即使页面上没有这个元素测试也能够继续执行。

### 在你的测试中执行 JavaScript

在一个应用程序中使用 JavaScript 是非常方便的，但是 Selenium 不直接支持它。你可以在 Selenium RC 中使用 getEval 接口的方法来执行它。

考虑一个应用中的没有静态 id 的多选框。在这种情况下，你可以通过使用 Selenium RC 对 JavaScript 语句进行求值（evaluate）来找到所有的多选框并处理它们。


    // Java
    public static String[] getAllCheckboxIds () {
         String script = "var inputId  = new Array();";// Create array in java script.
                script += "var cnt = 0;"; // Counter for check box ids.
                script += "var inputFields  = new Array();"; // Create array in java script.
                script += "inputFields = window.document.getElementsByTagName('input');"; // Collect input elements.
                script += "for(var i=0; i<inputFields.length; i++) {"; // Loop through the collected elements.
                script += "if(inputFields[i].id !=null " +
                          "&& inputFields[i].id !='undefined' " +
                          "&& inputFields[i].getAttribute('type') == 'checkbox') {"; // If input field is of type check box and input id is not null.
                script += "inputId[cnt]=inputFields[i].id ;" + // Save check box id to inputId array.
                          "cnt++;" + // increment the counter.
                          "}" + // end of if.
                          "}"; // end of for.
                script += "inputId.toString();" ;// Convert array in to string.
         String[] checkboxIds = selenium.getEval(script).split(","); // Split the string.
         return checkboxIds;
     }
 
如果要计算页面中的图片数，你可以：

    // Java
    selenium.getEval("window.document.images.length;");
    
记住要调用 window 对象，以防在 DOM 表达式中其默认指向 Selenium 窗口而不是测试窗口。

## 服务端选项

当服务启动时，可以使用命令行配置项来改变其默认行为。

回想一下，我们是这样启动服务的：

    $ java -jar selenium-server-standalone-<version-number>.jar
    
你可以使用 -h 来查看所有的配置项：

    $ java -jar selenium-server-standalone-<version-number>.jar -h
    
你将看到所有配置项列表，每个配置项附带间断描述。这里提供的描述并不总是足够禽畜，所以接下来我们将对一些重要的配置项进行补充描述。




















