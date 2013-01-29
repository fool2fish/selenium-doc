# Selenium WebDriver

注意：本章内容官方团队正在完善中。

## 介绍 WebDriver

Selenium 2.0 最主要的一个新特性就是集成了 WebDriver API。WebDriver 提供更精简的编程几口，以解决 Selenium-RC API 中的一些限制。WebDriver 为那些页面元素可以不通过页面重新加载来更新的动态网页提供了更好的支持。WebDriver 的目标是提供一套精心设计的面向对象的 API 来更好的支持现代高级 web 应用的测试工作。

## 同 Selenium-RC 相比，WebDriver 如何驱动浏览器的？

Selenium-WebDriver 直接通过浏览器自动化的本地接口来调用浏览器。如何直接调用，和调用的细节取决于你使用什么浏览器。本章后续的内容介绍了每个 “browser driver” 的详细信息。

相比 Selenium-RC ，WebDriver 确实非常不一样。Selenium-RC 在所有支持的浏览器中工作原理是一样的。它将 JavaScript 在浏览器加载的时候注入浏览器，然后使用这些 JavaScript 驱动 AUT 运行 WebDriver 使用的是不同的技术，再一次强调，它是直接调用浏览器自动化的本地接口。

## WebDriver 和 Selenium-Server

你可能需要，也可能不需要 Selenium Server，取决于你打算如何使用 Selenium-WebDriver。如果你仅仅需要使用 WebDriver API，那就不需要 Selenium-Server。如果你所有的测试和浏览器都在一台机器上，那么你仅需要 WebDriver API。WebDriver 将直接操作浏览器。

在有些情况下，你需要使用 Selenium-Server 来配合 Selenium-WebDriver 工作，例如：

- 你使用 Selenium-Grid 来分发你的测试给多个机器或者虚拟机。
- 你希望连接一台远程的机器来测试一个特定的浏览器。
- 你没有使用 Java 绑定（例如 Python, C#, 或 Ruby），并且可能希望使用 HtmlUnit Driver。

## 设置一个 Selenium-WebDriver 项目

安装 Selenium 意味着当你创建一个项目，你可以在项目中使用 Selenium 开发。具体怎么做取决于你的项目语言和开发环境。

### Java

创建一个 Selenium 2.0 Java 项目最简单的方式是使用 maven。Maven 将下载 Java 绑定（Selenium 2.0 的 Java 客户端）和其所有依赖，并且通过 pom.xml（mvn项目配置）为你创建项目。当你完成这些操作的时候，你可以将 maven 项目导入到你偏好的 IDE 中，例如 IntelliJ IDEA 或 Eclipse。

首先，创建一个用于放置项目的文件夹。然后，在这个文件夹中创建 pom.xml 文件，内容如下：

    <?xml version="1.0" encoding="UTF-8"?>
    <project xmlns="http://maven.apache.org/POM/4.0.0"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                 xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
        <modelVersion>4.0.0</modelVersion>
        <groupId>MySel20Proj</groupId>
        <artifactId>MySel20Proj</artifactId>
        <version>1.0</version>
        <dependencies>
            <dependency>
                <groupId>org.seleniumhq.selenium</groupId>
                <artifactId>selenium-java</artifactId>
                <version>2.28.0</version>
            </dependency>
            <dependency>
                <groupId>com.opera</groupId>
                <artifactId>operadriver</artifactId>
            </dependency>
        </dependencies>
        <dependencyManagement>
            <dependencies>
                <dependency>
                    <groupId>com.opera</groupId>
                    <artifactId>operadriver</artifactId>
                    <version>1.1</version>
                    <exclusions>
                        <exclusion>
                            <groupId>org.seleniumhq.selenium</groupId>
                            <artifactId>selenium-remote-driver</artifactId>
                        </exclusion>
                    </exclusions>
                </dependency>
            </dependencies>
        </dependencyManagement>
    </project>
    
确保你指定了最新版本。在编写本文档时，范例代码中的即为最新版本。但是，稍后 Selenium 2.0 还会不断有新发布。检查 [Maven 下载页面](http://seleniumhq.org/download/maven.html) 中的最新版本，并修改上述文件中依赖的版本。

命令行进入本目录，运行如下命令：

    mvn clean install

该命令会下载 Selenium 和其所有依赖，并添加到这个项目中。

最后，将项目导入到你的 IDE。对于不太熟悉 IDE 的用户，我们提供了附件来说明相关内容。

[Importing a maven project into IntelliJ IDEA](http://seleniumhq.org/docs/appendix_installing_java_driver_Sel20_via_maven.jsp#importing-maven-into-intellij-reference)

[Importing a maven project into Eclipse](http://seleniumhq.org/docs/appendix_installing_java_driver_Sel20_via_maven.jsp#importing-maven-into-eclipse-reference)

## 从 Selenium 1.0 迁移

对于那些已经使用 Selenium 1.0 编写测试套件的用户，我们提供了一些迁移的建议。Selenium 2.0 的核心工程师 Simon Stewart 写了一篇关于从 Selenium 1.0 迁移的文章，包含在本文的附件中。

[Migrating From Selenium RC to Selenium WebDriver](http://seleniumhq.org/docs/appendix_migrating_from_rc_to_webdriver.jsp#migrating-to-webdriver-reference)

## 实例介绍 Selenium-WebDriver API

WebDriver 是一个进行 web 应用测试自动化的工具，主要用于验证它们的行为是否符合期望。WebDriver 的目标是提供一套易于掌握的 API，且比 Selenium-RC (1.0) 更易于使用，页能是你的测试更具可读性和维护性。它没有同任何特定的测试框架进行绑定，所以可以在单元测试或者是 main 方法中工作良好。本小节介绍  WebDriver API，并且帮助你熟悉它。如果你还没有任何 WebDriver 项目，请按照上一小节的介绍新建一个。  

建好项目后，你可以发现 WebDriver 和任何普通的库一样：它是自包含的，通常不需要进行任何额外的处理或者运行安装。这一点和 Selenium-RC 的代理服务器是不一样的。

**注意：** 使用 Chrome Driver、 Opera Driver、Android Driver 和 iPhone Driver 是需要一些额外操作的。

我们准备了一个简单的例子：在 Google 上搜索 “Cheese”，然偶输出搜索结果页的页面标题到 console。

    package org.openqa.selenium.example;

    import org.openqa.selenium.By;
    import org.openqa.selenium.WebDriver;
    import org.openqa.selenium.WebElement;
    import org.openqa.selenium.firefox.FirefoxDriver;
    import org.openqa.selenium.support.ui.ExpectedCondition;
    import org.openqa.selenium.support.ui.WebDriverWait;
    
    public class Selenium2Example  {
        public static void main(String[] args) {
            // 创建了一个 Firefox driver 的实例
            // 注意，其余的代码依赖于接口而非实例
            WebDriver driver = new FirefoxDriver();
    
            // 使用它访问 Google
            driver.get("http://www.google.com");
            // 同样的事情也可以通过以下代码完成
            // driver.navigate().to("http://www.google.com");
    
            // 找到搜索输入框
            WebElement element = driver.findElement(By.name("q"));
    
            // 输入要查找的词
            element.sendKeys("Cheese!");
    
            // 提交表单
            element.submit();
    
            // 检查页面标题
            System.out.println("Page title is: " + driver.getTitle());
            
            // Google 搜索结果由 JavaScript 动态渲染
            // 等待页面加载完毕，超时时间设为10秒
            (new WebDriverWait(driver, 10)).until(new ExpectedCondition<Boolean>() {
                public Boolean apply(WebDriver d) {
                    return d.getTitle().toLowerCase().startsWith("cheese!");
                }
            });
    
            //应该能看到: "cheese! - Google Search"
            System.out.println("Page title is: " + driver.getTitle());
            
            //关闭浏览器
            driver.quit();
        }
    }

在接下来的章节中，你将学习到更多使用 WebDriver 的知识，例如根据浏览器历史记录前进和后退，如何测试 frames 和 windows。针对这些点我们提供了全面的讨论和范例。

## Selenium-WebDriver API 和操作

### 获取一个页面

访问一个页面或许是使用 WebDriver 时你第一件想要做的事情。最常见的是调用 “get” 方法：

    driver.get("http://www.google.com");

包括操作系统和浏览器在内的多种因素影响，WebDriver 可能会也可能不会等待页面加载。在某些情况下，WebDriver可能在页面加载完毕前就返回控制了，甚至是开始加载之前。为了确保健壮性，你需要使用 [Explicit and Implicit Waits](http://seleniumhq.org/docs/04_webdriver_advanced.jsp#explicit-and-implicit-waits-reference) 等到页面元素可用。

### 查找 UI 元素（web 元素）

WebDriver 实例可以查找 UI 元素。每种语言实现都暴露了 “查找单个元素” 和 “查找所有元素” 的方法。第一个方法如果找到则返回该元素，如果没找到则抛出异常。第二种如果找到则返回一个包含所有元素的列表，如果没找到则返回一个空数组。

“查找” 方法使用了一个定位器或者一个叫 “By” 的查询对象。“By” 支持的元素查找策略如下：

#### By id

这是最高效也是首选的方法用于查找一个元素。UI 开发人员常犯的错误是，要么没有指定 id，要么自动生成随机 id，这两种情况都应避免。及时是使用 class 也比使用自动生成随机 id 要好的多。

HTML:

    <div id="coolestWidgetEvah">...</div>

Java：

    WebElement element = driver.findElement(By.id("coolestWidgetEvah"));

#### By Class Name

"class" 是 DOM 元素上的一个属性。在实践中，通常是多个 DOM 元素有同样的 class 名，所以通常用它来查找多个元素。

HTML:

    <div class="cheese"><span>Cheddar</span></div><div class="cheese"><span>Gouda</span></div>

Java：

    List<WebElement> cheeses = driver.findElements(By.className("cheese"));

#### By Tag Name

根据元素标签名查找。

HTML:

    <iframe src="..."></iframe>

Java：

    WebElement frame = driver.findElement(By.tagName("iframe"));

#### By Name

查找 name 属性匹配的表单元素。

HTML:

    <input name="cheese" type="text"/>

Java：

    WebElement cheese = driver.findElement(By.name("cheese"));

#### By Link Text

查找链接文字匹配的链接元素。

HTML：

    <a href="http://www.google.com/search?q=cheese">cheese</a>>

Java：

    WebElement cheese = driver.findElement(By.linkText("cheese"));

#### By Partial Link Text

查找链接文字部分匹配的链接元素。

HTML:

    <a href="http://www.google.com/search?q=cheese">search for cheese</a>>

Java：

    WebElement cheese = driver.findElement(By.partialLinkText("cheese"));

#### By CSS

正如名字所表明的，它通过 css 来定位元素。默认使用浏览器本地支持的选择器，可参考 w3c 的 [css 选择器](http://www.w3.org/TR/CSS/#selectors)。如果浏览器默认不支持 css 查询，则使用 Sizzle。ie6、7 和 ff3.0 都使用了 Sizzle。

注意使用 css 选择器不能保证在所有浏览器里都表现一样，有些在某些浏览器里工作良好，在另一些浏览器里可能无法工作。

HTML:

    <div id="food"><span class="dairy">milk</span><span class="dairy aged">cheese</span></div>

Java：

    WebElement cheese = driver.findElement(By.cssSelector("#food span.dairy.aged"));

#### By XPATH

此处略过不译

### 用户输入 - 填充表单

我们已经了解了怎么在输入框或者文本框中输入文字，但是如何操作其他的表单元素呢？你可以切换多选框的选中状态，你可以通过“点击”以选中一个 select 的选项。操作 select 元素不是一件很难的事情：

    WebElement select = driver.findElement(By.tagName("select"));
    List<WebElement> allOptions = select.findElements(By.tagName("option"));
    for (WebElement option : allOptions) {
        System.out.println(String.format("Value is: %s", option.getAttribute("value")));
        option.click();
    }

上述代码将找到页面中第一个 select 元素，然后遍历其中的每个 option，打印其值，再依次进行点击操作以选中这个 option。这并不是处理 select 元素最高效的方式。WebDriver
有一个叫 “Select” 的类，这个类提供了很多有用的方法用于 select 元素进行交互。

    Select select = new Select(driver.findElement(By.tagName("select")));
    select.deselectAll();
    select.selectByVisibleText("Edam");

上述代码取消页面上第一个 select 元素的所有 option 的选中状态，然后选中字面值为 “Edam” 的 option。

如果你已经完成表单填充，你可能希望提交它，你只要找到 “submit” 按钮然后点击它即可。

    driver.findElement(By.id("submit")).click();

或者，你可以调用 WebDriver 为每个元素提供的 “submit” 方法。如果你对一个 form 元素调用该方法，WebDriver 将调用这个 form 的 submit 方法。如果这个元素不是一个 form，将抛出一个异常。

    element.submit();
    
### 在窗口和帧(frames)之间切换

有些 web 应用含有多个帧或者窗口。WebDriver 支持通过使用 “switchTo” 方法在多个帧或者窗口之间切换。

    driver.switchTo().window("windowName");

所有 dirver 上的方法调用均被解析为指向这个特定的窗口。但是我们如何知道这个窗口的名字？来看一个打开窗口的链接：

    <a href="somewhere.html" target="windowName">Click here to open a new window</a>

你可以将 “window handle” 传递给 “switchTo().window()” 方法。因此，你可以通过如下方法遍历所有打开的窗口：

   for (String handle : driver.getWindowHandles()) {
        driver.switchTo().window(handle);
    }

你也可以切换到指定帧：

    driver.switchTo().frame("frameName");

你可以通过点分隔符来访问子帧，也可以通过索引号指定它，例如：

    driver.switchTo().frame("frameName.0.child");

该方法将查找到名为 “frameName” 的帧的第一个子帧的名为 “child” 的子帧。所有帧的计算都会从 **top** 开始。

### 弹出框

由 Selenium 2.0 beta 1 开始，就内置了对弹出框的处理。如果你触发了一个弹出框，你可以通过如下方访问到它：

    Alert alert = driver.switchTo().alert();

该方法将返回目前被打开的弹出框。通过这个返回对象，你可以访问、关闭、读取它的内容甚至在 prompt 中输入一些内容。这个接口可以胜任 alerts,comfirms 和 prompts 的处理。

### 导航：历史记录和位置

更早的时候，我们通过 “get” 方法来访问一个页面 (driver.get("http://www.example.com"))。正如你所见，WebDriver 有一些更小巧的、聚焦任务的接口，而 navigation 就是其中一个非常有用的任务。因为加载页面是一个非常基本的需求，实现该功能的方法取决于 WebDriver 暴露的接口。它等同于如下代码：

    driver.navigate().to("http://www.example.com");

重申一下: “navigate().to()” 和 “get()” 做的事情是完全一样的。只是前者更易用。

“navigate” 接口暴露了访问浏览器历史记录的接口：

    driver.navigate().forward();
    driver.navigate().back();

需要注意的是，该功能的表现完全依赖于你所使用的浏览器。如果你习惯了一种浏览器，那么在另一种浏览器中使用它时，完全可能发生一些意外的事情。

### Cookies

在我们继续介绍更多内容之前，还有必要介绍一下如何操作 cookie。首先，你必须在 cookie 所在的域。如果你希望在加载一个大页面之前重设 cookie，你可以先访问站点中一个较小的页面，典型的是 404 页面 (http://example.com/some404page)。

    // 进到正确的域
    driver.get("http://www.example.com");
    
    // 设置 cookie，这个cookie 对整个域都有效
    Cookie cookie = new Cookie("key", "value");
    driver.manage().addCookie(cookie);
    
    // 输出当前 url 所有可用的 cookie
    Set<Cookie> allCookies = driver.manage().getCookies();
    for (Cookie loadedCookie : allCookies) {
        System.out.println(String.format("%s -> %s", loadedCookie.getName(), loadedCookie.getValue()));
    }
    
    // 你可以通过3中方式删除 cookie
    // By name
    driver.manage().deleteCookieNamed("CookieName");
    // By Cookie
    driver.manage().deleteCookie(loadedCookie);
    // Or all of them
    driver.manage().deleteAllCookies();

### 改变 UA

当使用 Firefox Driver 的时候这很容易：

    FirefoxProfile profile = new FirefoxProfile();
    profile.addAdditionalPreference("general.useragent.override", "some UA string");
    WebDriver driver = new FirefoxDriver(profile);

### 拖拽

以下代码演示了如何使用 “Actions” 类来实现拖拽。浏览器本地方法必须要启用：

    WebElement element = driver.findElement(By.name("source"));
    WebElement target = driver.findElement(By.name("target"));
    
    (new Actions(driver)).dragAndDrop(element, target).perform();

## Driver 特性和权衡

## Selenium-WebDriver’s Drivers

WebDriver 是编写测试时需要用到的方法的主要接口，这套接口有几套实现。包括：

### HtmlUnit Driver

这是目前 WebDriver 最快速最轻量的实现。顾名思义，它是基于 HtmlUnit 的。HtmlUnit 是一个由 Java 实现的没有 GUI 的浏览器。任何非 Java 的语言绑定， Selenium Server 都需要使用这个 driver。

#### 使用

    WebDriver driver = new HtmlUnitDriver();

#### 优势

- WebDriver 最快速的实现
- 纯 Java 实现，跨平台
- 支持 JavaScript

#### 劣势

- 需要模拟浏览器中 JavaScript 的行为（如下）。

#### JavaScript in the HtmlUnit Driver

没有任何一个主流浏览器支持 HtmlUnit 使用的 JavaScript 引擎（Rhino）。如果你使用 HtmlUnit，测试结果可能和真实在浏览器中跑的很不一样。

当我们说到 “JavaScript” 时通常是指 “JavaScript 和 DOM”。虽然 DOM 由 W3C 组织定义，但是每个浏览器在 DOM 和 JavaScript 的交互的实现方面都有一些怪异和不同的地方。HtmlUnit 完全实现了 DOM 规范，并且对 JavaScript 提供了良好的支持，但它的实现和真实的浏览器都不一样：虽然它模拟了浏览器中的实现，但既不同于 W3C 指定的标准，也不同于其他主流浏览器的实现。

使用 WebDriver，我们需要做出选择：如果我们启用 HtmlUnit 的 JavaScript 支持，团队可能会遇到只有在这中情况下才会遇到的问题；如果我们禁用 JavaScript，但实际上越来越多的网站都依赖于 JavaScript。我们使用了最保守的方式，默认禁用 JavaScript 支持。对于 WebDriver 和 HtmlUnit 的每个发布版本，我们都会再次评估：这个版本是否可以默认开启 JavaScript 支持。

##### 启用 JavaScript

启用 JavaScript 也非常简单：

    HtmlUnitDriver driver = new HtmlUnitDriver(true);

上述代码会使得 HtmlUnit Driver 模拟 Firefox3.6 对 JavaScript 的处理。

### Firefox Driver

我们通过一个 Firefox 的插件来控制 Firefox 浏览器。使用的配置文件是从默认安装的版本精简成只包含 Selenium WebDriver.xpi (插件) 的版本。我们还修改了一些默认配置（[see the source to see which ones](http://code.google.com/p/selenium/source/browse/trunk/java/client/src/org/openqa/selenium/firefox/FirefoxProfile.java#55)）,使得 Firefox Driver 可以运行和测试在 Windows、Mac、Linux 上。

#### 使用

    WebDriver driver = new FirefoxDriver();

#### 优势

- 在真实的浏览器里运行，且支持 JavaScript
- 比 IE Driver 快

#### 劣势

- 比 HtmlUnit Driver 慢
- 需要修改 Firefox 配置

例如你想修改 UA，但是你得到的是一个假的包含很多扩展的配置文件。这里有两种方式可以拿到真是的配置，假定配置文件是由 Firefox 配置管理器生成的：

    ProfilesIni allProfiles = new ProfilesIni();
    FirefoxProfile profile = allProfiles.getProfile("WebDriver");
    profile.setPreferences("foo.bar", 23);
    WebDriver driver = new FirefoxDriver(profile);

如果配置文件没有注册至 Firefox：

    File profileDir = new File("path/to/top/level/of/profile");
    FirefoxProfile profile = new FirefoxProfile(profileDir);
    profile.addAdditionalPreferences(extraPrefs);
    WebDriver driver = new FirefoxDriver(profile);

当我们开发 Firefox Driver 的特性时，需要评估它们是否可用。例如，直到我们认为本地方法在 Linux 的 Firefox 上是稳定的了，否则我们会默认禁用它。如需开启：

    FirefoxProfile profile = new FirefoxProfile();
    profile.setEnableNativeEvents(true);
    WebDriver driver = new FirefoxDriver(profile);

#### 信息

查看 [Firefox section in the wiki page](http://code.google.com/p/selenium/wiki/FirefoxDriver) 以获得更多新鲜信息。

### Internet Explorer Driver

这个 driver 由一个 .dll 文件控制，并且只在 windows 系统中可用。每个 Selenium 的发布版本都包含可用于测试的核心功能，兼容 XP 上的 ie6、7、8 和 Windows7 上的 ie9。

#### 使用

    WebDriver driver = new InternetExplorerDriver();

#### 优势

- 运行在真实的浏览器中，并且支持 JavaScript，包括最终用户会碰到的一些怪异的问题。

#### 劣势

- 显然它只在 Windows 系统上有效。
- 相对较慢。
- Xpath 在很多版本中都是非原生支持。Sizzle 会注入到浏览器，这使得它比其他浏览器要慢很多，也比在相同的浏览器中使用 CSS 选择器要慢。
- IE 6、7 不支持 CSS 选择器，由 Sizzle 注入替代。
- IE 8、9 虽然原生支持 CSS 选择器，但它们不完全支持 CSS3.

#### 信息

访问 [Internet Explorer section of the wiki page](http://code.google.com/p/selenium/wiki/InternetExplorerDriver) 以获得更多新鲜信息。特别注意配置部分的内容。

### Chrome Driver

Chrome Driver 由 Chromium 项目团队自己维护和支持。WebDriver 通过 chromedriver 二进制包（可以在 chromiun 的下载页面找到）来工作。你需要确保同时安装了某版本的 chrome 浏览器和 chromedriver。chromedriver 需要存放在某个指定的路径下使得 WebDriver 可以自动发现它。chromedriver 可以发现安装在默认路径下的 chrome 浏览器。这些都可以被环境变量覆盖。请查看 [wiki](http://code.google.com/p/selenium/wiki/ChromeDriver) 以获得更多信息。

#### 使用

    WebDriver driver = new ChromeDriver();

#### 优势

- 运行在真实的浏览器中，并且支持 JavaScript。
- 由于 chorme 是一个 webkit 内核的浏览器，Chrome Driver 能让你的站点在 Safari 中运行。注意自从 Chrome 使用了自己的 Javascript 引擎 V8 以后（之前是 Safari 的 Nitro 引擎），Javascript 的执行可能会一点不一样。

#### 劣势

- 比 HtmlUnit 慢

#### 信息

查看 [wiki](http://code.google.com/p/selenium/wiki/ChromeDriver) 以获得更多最新信息。更多信息可以在 [下载页面](http://seleniumhq.org/download/) 找到。

#### 运行 Chrome Driver

下载 [Chrome Driver](http://code.google.com/p/chromium/downloads/list) 并参考 [wiki](http://code.google.com/p/selenium/wiki/ChromeDriver) 上的其他建议。

### Opera Driver

查看 [wiki](http://code.google.com/p/selenium/wiki/OperaDriver)

### iPhone Driver

查看 [wiki](http://code.google.com/p/selenium/wiki/IPhoneDriver)

### Android Driver

查看 [wiki](http://code.google.com/p/selenium/wiki/AndroidDriver)

## 可选择的后端：混合 WebDriver 和 RC 技术

### WebDriver-Backed Selenium-RC




















