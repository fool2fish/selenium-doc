# Selenium Grid

## 快速上手

如果你对 Selenium 自动化测试已经非常熟悉，你仅仅需要一个快速上手来使程序运行起来。本章节的内容能满足不同的技术层次，但是如果你仅仅需要一个可以快速上手的指引，那么就显得有点多。如果是这样，你可以参考 [Selenium Wiki](http://code.google.com/p/selenium/wiki/Grid2) 的相关文章。

## 什么是 Selenium-Grid ?

Selenium-Grid 允许你在多台机器的多个浏览器上并行的进行测试，也就是说，你可以同时运行多个测试。本质上来说就是，Selenium-Grid 支持分布式的测试执行。它可以让你的测试在一个分布式的执行环境中运行。

## 何时需要使用

通常，以下两种情况你都会需要使用 Selenium-Grid。

- 在多个浏览器中运行测试，在多个版本的浏览器中进行测试，或在不同操作系统的浏览器中进行测试。
- 减少测试运行时间。

Selenium-Grid 通过使用多台机器并行地运行测试来加速测试的执行过程。例如,如果你有一个包含100个测试用例的测试套件,你使用 Selenium-Grid 支持4台不同的机器（虚拟机或实体机均可）来运行那些测试，同仅使用一台机器相比，你的测试所需要的运行时间大致为其 1/4。对于大型的测试套件和那些会进行大量数据校验的需要长时间运行的测试套件来说，这将节约很多时间。有些测试套件可能要运行好几小时。另一个需要缩短套件运行时间的原因是开发者检入（check-in）AUT 代码后，需要缩短测试的运行周期。越来越多的团队使用敏捷开发，相比整夜整夜的等待测试通过，他们希望尽快地看到测试反馈。

Selenium-Grid 也可以用于支持多执行环境的测试运行，典型的，同时在多个不同的浏览器中运行。例如，Grid 的虚拟机可以安装测试必须的各种浏览器。于是，机器 1 上有 ie8，机器 2 上有 ie9，机器 3 上有最新版的 chrome，而机器 4 上有最新版的 firefox。当测试套件运行时，Selenium-Grid 可以使测试在指定的浏览器中运行，并且接收每个浏览器的运行结果。

另外，我们可以拥有一个装有多个类型和版本都一样的浏览器 Grid。例如，一个 Grid 拥有 4 台机器，每台机器可以运行 3 个 firefox 12 实例，形成一个 firefox 的服务农场。当测试套件运行时，每个传递给 Selenium-Grid 的测试都被指派给下一个可用的 firefox 实例。通过这种方式，我们可以使得同时有 12 个测试在并行的运行以完成测试，显著地缩短了测试完成需要的时间。

Selenium-Grid 非常灵活。以上两个例子可以联合起来使用，这样可以就可以使得不同类型和版本的浏览器有多个可运行实例。使用这样的配置，既并行地执行测试，同时又可以测试多个浏览器类型和版本。

## Selenium-Grid 2.0

Selenium-Grid 2.0 是在编写本文时(5/26/2012)已发布的最新版本。它同版本 1 有很多不同之处。在 2.0 中，Selenium-Grid 和 Selenium-RC 服务端进行了合并。现在，你仅需要下载一个 jar 包就可以获得它们。

## Selenium-Grid 1.0

版本 1 是 Selenium-Grid 的第一个发布版本。如果你是一个 Selenium-Grid 新手，你应该选择版本 2 。新版本已经在原有基础上进行了更新，页增加了一些新特性，并且支持 Selenium-WebDriver。一些老的系统可能仍然在使用版本 1.关于 Selenium-Grid 版本 1 的信息可以参考 [Selenium-Grid website](http://selenium-grid.seleniumhq.org/)

## Selenium-Grid 的 Hub 和 Nodes 是如何工作的？

Grid 由一个中心和一到多个节点组成。两者都是通过 selenium-server.jar 启动。在接下来的章节中，我们列出了一些例子。

中心接收要执行的测试信息，包括在哪些平台和浏览器执行等。它知道每个注册了的节点的配置。根据测试信息，它会选择符合需求的节点进行测试。一旦选定了一个节点，测试脚本就会初始化 Selenium 命令，并且由重心发送给选定的要运行测试的节点。这个节点会启动浏览器，然后在浏览器中执行这个 AUT 的 Selenium 命令。 

我们提供了一些[图标](http://selenium-grid.seleniumhq.org/how_it_works.html)来演示其原理。第二张图标是用以说明 Selenium-Grid 1 的，版本 2 也适用并且对于我们的描述是一个很好的说明。唯一的区别在于相关术语。使用“Selenium-Grid 节点”替换“Selenium Remote Control”即符合我们对 Selenium-Grid 2 的描述。

## 下载

下载过程很简单。从 SeleniumHq 站点的[下载页面](http://docs.seleniumhq.org/download/)下载 Selenium-Server jar 包。你需要的链接在“Selenium-Server (以前是 Selenium-RC)”章节中。

将它存放到任意文件夹中。你需要确保机器上正确的安装了 java。如果 java 没有正常运行，检查你系统的 path 变量是否包含了 java.exe 的路径。

## 启动 Selenium-Grid

由于节点对中心有依赖，所以你通常需要先启动一个中心。这也不是必须的，因为节点可以识别其中心是否已经启动，反之亦然。作为教程，我们建议你先启动中心，否则会显示一些错误信息，你应该不会想在第一次使用 Selenium-Grid 的时候就看到它们。

### 启动中心

通过在命令行执行以下命令，可以启动一个使用默认设置的中心。所有平台可用，包括 Windows Linux, 或 MacOs 。

    java -jar selenium-server-standalone-2.21.0.jar -role hub

我们将在接下来的章节中解释各个参数。注意，你可能需要修改上述命令中 jar 包的版本号，这取决于你使用的 selenium-server 的版本。

### 启动节点

通过在命令行执行以下命令，可以你懂一个使用默认设置的节点。

    java -jar selenium-server-standalone-2.21.0.jar -role node  -hub http://localhost:4444/grid/register

该操作假设中心是使用默认设置启动的。中心用于监听请求使用的默认端口号为 4444，这就是为什么端口 4444 被用于中心 url 中。同时“localhost”假定你的节点和中心运行在同一台机器上。对于新手来说，这是最简单的方式。如果要在两台不同的机器上运行中心和节点，只需要将“localhost”替换成中心所在机器的 hostname 即可。

**警告：** 确保运行中心和节点的机器均已关闭防火墙，否则你将看到一个连接错误。

## 配置 Selenium-Grid

### 默认配置
### JSON 配置文件
### 通过命令行选项配置

## 中心配置

通过指定 `-role hub` 即以默认设置启动中心：

    java -jar selenium-server-standalone-2.21.0.jar -role hub

你将看到以下日志输出：

    Jul 19, 2012 10:46:21 AM org.openqa.grid.selenium.GridLauncher main
    INFO: Launching a selenium grid server
    2012-07-19 10:46:25.082:INFO:osjs.Server:jetty-7.x.y-SNAPSHOT
    2012-07-19 10:46:25.151:INFO:osjsh.ContextHandler:started o.s.j.s.ServletContextHandler{/,null}
    2012-07-19 10:46:25.185:INFO:osjs.AbstractConnector:Started SocketConnector@0.0.0.0:4444

### 指定端口

中心默认使用的端口是 4444 。这是一个 TCP/IP 端口，被用于监听客户端，即自动化测试脚本到 Selenium-Grid 中心的连接。如果你电脑上的另一个应用已经占用这个接口，或者你已经启动了一个 Selenium-Server，你将看到以下输出：

    10:56:35.490 WARN - Failed to start: SocketListener0@0.0.0.0:4444
    Exception in thread "main" java.net.BindException: Selenium is already running on port 4444. Or some other service is.

如果看到这个信息，你可以关掉在使用端口 4444 的进程，或者告诉 Selenium-Grid 使用一个别的端口来启动中心。`-port` 选项用于修改中心的端口：

    java -jar selenium-server-standalone-2.21.0.jar -role hub -port 4441

即使已经有一个中心运行在这台机器上，只要它们不使用同一个端口，就能正常工作。

你可能想知道哪个进程使用了 4444 端口，这样你就可以让中心使用这个默认端口。使用以下命令可以查看你机器上所有运行程序使用的端口：

    netstat -a
    
Unix/Linux, MacOs 和 Windows 均支持此命令，只是在 Windows 中 -a 参数为必须的。基本上，你需要显示进程 id 和端口。在 Unix 中，你可以通过管道 “grep” 输出那些你关心的端口相关的条目。

## 节点配置

## 时间参数

## 获取命令行帮助

Selenium-Server 提供了一个可选项列表，每个选项都有一个简短的描述。目前（2012夏），命令行帮助还有一些奇怪，但是如果你知道如何去找、如何解读信息会对你很有帮助。

Selenium-Server 提供了两种不同的功能，Selenium-RC server 和 Selenium-Grid。它们是两个不同的团队编写的，所以每个功能的命令行帮助被放置在不同的地方。因此，对于新手来说，在初次使用任意一个功能时，帮助都不是那么显而易见。

如果你仅传递一个 `-h` 选项，你将看到 Selenium-RC Server 的可选项而不是 Selenium-Grid 的。

    java -jar selenium-server-standalone-2.21.0.jar -h

上述代码将显示 Selenium-RC server 选项。如果你想看到 Selenium-Grid 的命令行帮助，你需要先使用 `-hub` 或 `-node` 选项告诉 Selenium-Server 你想看的是关于 Selenium-Grid 的，然后再追加 `-h` 选项。

    java -jar selenium-server-standalone-2.21.0.jar -role node -h

对于这个问题，你还可以给 `-role node` 传递一个垃圾参数：

    java -jar selenium-server-standalone-2.21.0.jar -role node xx

你将先看到 “INFO...” 和一个 “ERROR”，在其后你将看到 Selenium-Grid 的命令行选项。我们没有列出这个命令的所有输出，因为它实在太长了，这个输出的最初几行看起来如下：

    Jul 19, 2012 10:10:39 AM org.openqa.grid.selenium.GridLauncher main
    INFO: Launching a selenium grid node
    org.openqa.grid.common.exception.GridConfigurationException: You need to specify a hub to register to using -hubHost X -hubPort 5555. The specified config was -hubHost null -hubPort 4444
            at org.openqa.grid.common.RegistrationRequest.validate(RegistrationRequest.java:610)
            at org.openqa.grid.internal.utils.SelfRegisteringRemote.startRemoteServer(SelfRegisteringRemote.java:88)
            at org.openqa.grid.selenium.GridLauncher.main(GridLauncher.java:72)
    Error building the config :You need to specify a hub to register to using -hubHost X -hubPort 5555. The specified config was -hubHost null -hubPort 4444
    Usage :
      -hubConfig:
            (hub) a JSON file following grid2 format.
    
     -nodeTimeout:
            (node) <XXXX>  the timeout in seconds before the hub
              automatically ends a test that hasn't had aby activity than XX
              sec.The browser will be released for another test to use.This
              typically takes care of the client crashes.

## 常见错误

### Unable to acess the jarfile

Unable to access jarfile selenium-server-standalone-2.21.0.jar

无论是启动中心还是节点都有可能产生这个错误。这意味着 java 无法找到 selenium-server jar 包。你需要从 selenium-server-XXXX.jar 文件存放在目录运行命令或者指定 jar 包的完整路径。


