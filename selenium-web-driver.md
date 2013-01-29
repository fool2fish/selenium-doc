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



