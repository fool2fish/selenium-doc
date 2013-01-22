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

### 代理配置

如果你的 AUAT 使用了一个需要授权的 HTTP 代理，你需要使用以下命令来配置 http.proxyHost, http.proxyPort, http.proxyUser 和 http.proxyPassword。

    $ java -jar selenium-server-standalone-<version-number>.jar -Dhttp.proxyHost=proxy.com -Dhttp.proxyPort=8080 -Dhttp.proxyUser=username -Dhttp.proxyPassword=password

### 多窗口模式

如果你正在使用 Selenium 1，你可以跳过这部分内容，因为多窗口模式已经是默认配置。但是在更早的版本中，AUT 默认是在子帧(sub frame)中运行的。

![multi-window](http://seleniumhq.org/docs/_images/chapt5_img26_single_window_mode.png)

有些应用在子帧中不能正常运行，必须要加载到顶级帧中运行。多窗口模式允许 AUT 在两个独立的窗口中运行，而不是在默认的帧中运行，这样它就能在顶级帧中运行了。

![multi-window2](http://seleniumhq.org/docs/_images/chapt5_img27_multi_window_mode.png)

对于老版本的 Selenium 来说，你必须通过下面的配置项明确指定多窗口模式：

    -multiwindow
    
在 Selenium 1 以及更新的版本中，如果你希望在单窗口中运行你的测试，你可以使用以下配置项：

    -singlewindow

### 指定 Firefox 配置

Firefox 不会同时运行两个实例，除非你为每一个指定单独的配置。Selenium RC 1 及其后续版本会自动运行两个单独的配置，所以如果你正在使用 Selenium 1，你可以跳过这个章节。如果你在使用更老的版本而你有需要指定单独的配置，你需要明确的指定它。

首先，穿加你一个单独的 Firefox 配置，根据以下步骤。打开 Windows 的开始菜单，选择 “run”，然后键入以下内容：

    firefox.exe -profilemanager

    firefox.exe -P

使用对话框来创建新配置。当你运行 Selenium 服务时，你需要使用命令行选项 -firefoxProfileTemplate 告诉它使用新的 Firefox 配置，并且指定要使用的配置的路径。

    -firefoxProfileTemplate "path to the profile"

**警告**

确保你的配置文件被存放在一个不同于默认路径的文件夹中！！！Firefox 配置管理会在你删除一个配置的时候删除该配置所在文件夹的所有内容，而不管它是不是配置文件。

更多请参考 [Mozilla’s Knowledge Base](http://support.mozilla.com/zh-CN/kb/Managing+profiles)

### 通过 -htmlSuite 配置项在服务端直接运行 Selenese

通过将 html 文件传递给服务端的命令行，你可以直接在 Selenium 服务端运行 Selenese html 文件。例如：

    java -jar selenium-server-standalone-<version-number>.jar -htmlSuite "*firefox"
    "http://www.google.com" "c:\absolute\path\to\my\HTMLSuite.html"
    "c:\absolute\path\to\my\results.html"
    
这个例子将自动加载你的 html 测试套件，运行所有的测试并生成一份 html 格式的测试报告。

**注意**

在使用这个配置项时，服务端将开始运行测试，并为测试结束等待指定的秒数，如果测试没有在指定时间内结束，命令行将以一个非0的退出码退出，并且没有报告文件生成。

这个命令行非常长，所以键入它的时候需要非常小心。注意这要求你传入一个 html 测试套件，而非单个的测试。并且配置项和 -interactive 不兼容，你不能同时使用他们。

### Selenium 服务日志

#### 服务端日志

当启动 Selenium 服务，可以使用 -log 配置项来将 Selenium 服务报告的有价值的 debug 信息记录到一个文本文件。

    java -jar selenium-server-standalone-<version-number>.jar -log selenium.log

这个日志文件相比标准的 console 日志而言要冗余的多（它包括了 debug 级别的日志信息）。它页包含了 logger name，打印日志信息的线程 id。例如：

    20:44:25 DEBUG [12] org.openqa.selenium.server.SeleniumDriverResourceHandler -
    Browser 465828/:top frame1 posted START NEW

该信息格式为：

    TIMESTAMP(HH:mm:ss) LEVEL [THREAD] LOGGER - MESSAGE

#### 浏览器端日志

在浏览器端的 javascript （Selenium Core）也将记录重要的日志信息。在很多时候，对最终用户而言，这比常规的 Selenium 服务端日志有用的多。为了访问浏览器端日志，将 -browserSideLog 参数传递给 Selenium 服务。

    java -jar selenium-server-standalone-<version-number>.jar -browserSideLog
    
为了将所有浏览器端的日志保存到一个文件中，-browserSideLog 必须和 -log 配置项联合使用。

### 指定特定浏览器路径

你可以为 Selenium RC 指定一个特定浏览器的路径。如果你需要测试同一个浏览器的不同版本时，这一功能将非常有效。同时这也允许你在一个 Selenium RC 不直接支持的浏览器中运行你的测试。当指定这个运行模式，使用 *cunstom 来指定可执行的浏览器的全路径：

    *custom <path to browser>

## Selenium RC 架构

**注意**

该主题尝试解释 Selenium RC 背后的运行原理。这并不是 Selnium 用户需要了解的基础知识，但是你会发现它对于了解一些问题非常有用。

为了理解 Selenium RC 服务端工作的细节，以及为什么它使用代理注入和高特权模式你必须先了解 [同源策略](http://seleniumhq.org/docs/05_selenium_rc.jsp#the-same-origin-policy)。

### 同源策略

Selenium 面临的主要约束即同源策略。市面上所有的浏览器都有这个安全约束，它的目的是确保一个网站的内容永远不会被另外一个站点的脚本访问到。同源策略规定浏览器加载的任何脚本仅能操作引入它的页面所在的域的内容。它也不能执行另一个网站中的方法。例如，如果浏览器在载入 www.mysite.com 时加载了一个脚本，这脚本就不能操作 www.mysite2.com 的内容，即使那是另一个你自己的网站。如果这被允许，脚本将可以操作你打开的任何网站的内容，于是当你在 tab 页中打开一个银行站点时它就能读取你的银行账号信息。。我们把这叫 XSS(Cross-site Scripting) 攻击。

为了在这个约束下工作，Selenium Core（包括它的 javascript 脚本）必须和 AUT 放置在同一个域下。

之前，因为 Selenium core 使用 JavaScript 实现的，所以一直被这个问题困扰。但现在，这个问题已经得到解决。它使用 Selenium 服务端作为一个代理来避免这个问题。本质上来讲，Selenium RC 告诉浏览器它是运行在服务端提供的一个“被欺骗的”站点上。

**注意**

你可以在维基百科上找到更多关于 [同源策略](http://en.wikipedia.org/wiki/Same_origin_policy) 和 [XSS](http://en.wikipedia.org/wiki/Cross-site_scripting) 的内容

### 代理注入

Selenium 避免同源策略约束的首选方法是代理注入。在代理注入模式，Seleniium 服务端扮演一个客户端配置[1] 的 HTTP 代理[2] 的角色，它位于浏览器和 AUT 之间。它为 AUT 伪装了一个虚假的 url（将Selenium Core 和测试注入到 AUT，就好像他们来自同一个域）。

1. 代理扮演一个第三方角色，在双方传递内容的过程中。它好像一个 web 服务器将 AUT 传送给浏览器。作为一个代理，使得 Selenium 服务端有能力伪装 AUT 的真实 url。
2. 浏览器加载的时候，配置文件将指定 localhost:4444 作为 http 代理，这就是为什么浏览器发起一个 http 请求将通过 Selenium 服务端并且响应页将通过它而不是来自真实的服务器。以下是结构图：

![proxy](http://seleniumhq.org/docs/_images/chapt5_img02_Architecture_Diagram_1.png)

当测试开始时，将发生以下事情：

1. 客户端驱动将和 Selenium RC 服务端建立一个连接。
2. Selenium RC 服务端启动一个打开指定 url 的浏览器（或复用一个已打开的），将 Selenium Core 的 JavaScript 代码注入的这个页面中。
3. 客户端驱动向服务端传递一个 Selenese 命令。
4. 服务端解析这个命令，然后触发 JavaScript 脚本执行浏览器中相应的命令。
5. Selenium Core 指示浏览器在第一个指令后开始执行，典型的是打开一个 AUT 页面。
6. 浏览器收到打开页面的请求，并且从 Selenium RC 服务端询问获取页面内容（作为浏览器的 http 代理）
7. Selenium RC 服务端和网站服务器通讯，一旦获取到页面，它就对页面的源进行伪装然后发送到浏览器，使这个页面看起来像是和 Selenium Core 来自于同一个源（这使得我们可以绕开同源策略的限制）
8. 浏览器接收到这个页面并且渲染到相应的帧或者窗口。

### 高特权浏览器（Heightened Privileges Browsers）






