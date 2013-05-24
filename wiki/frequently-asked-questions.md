###selenium常见问题
---
原文：<https://code.google.com/p/selenium/wiki/FrequentlyAskedQuestions>
####Q:什么是WebDriver?
>A:WebDriver是一个用来写网页自动化测试的工具。它致力于模拟真实用户的行为并尽可能的实现HTML上的交互。
####Q:Selenium与[Sahi](http://sahi.co.in/)有什么异同？
>A:它们的目标是一样的，都是为了测试webapp。但是，它们的实现是不一样的。WebDriver是直接控制浏览器的，而不是在浏览器内运行了一个Javascript应用（这牵涉到同源策略问题）。这也意味着WebDriver可以充分利用原生平台提供的任何工具。
####Q：什么是Selenium 2.0?
>A:WebDriver是Selenium的一部分。而WebDriver为其提供了强大的API以及原生的Driver。
####Q：如何从原来的selenium api迁移到新的webdriver api?
>A:参见<http://seleniumhq.org/docs/appendix_migrating_from_rc_to_webdriver.html>
####Q:WebDriver支持哪些浏览器？
>A:目前支持[ChromeDriver](https://code.google.com/p/selenium/wiki/ChromeDriver),[InternetExplorerDriver](https://code.google.com/p/selenium/wiki/InternetExplorerDriver),[FirefoxDriver](https://code.google.com/p/selenium/wiki/FirefoxDriver),[OperaDriver](https://code.google.com/p/selenium/wiki/OperaDriver),[HtmlUnitDriver](https://code.google.com/p/selenium/wiki/HtmlUnitDriver)。它们各有什么优缺点，可以进入相应的链接进行查看。同时，它还支持移动测试，包括[AndroidDriver](https://code.google.com/p/selenium/wiki/AndroidDriver),OperaMobileDriver,IPhoneDriver。
####Q：“developer focused”是什么意思？
>A:我们认为，在一个软件开发团队内，那些能开发出别人都能使用的工具的人是真正的开发者。尽管直接使用WebDriver是很容易的，但是它也应该能作为一个构建块来产出更复杂智能的工具。正因为如此,webdriver也有一个很小的API使得你可以在你喜欢的IDE中简单的点击“autocomplete”，就能在任何浏览器内始终工作。
####Q:如何直接执行Javascript?
>A:我们相信当你使用工具时，有可能没有触发正确的时间，有可能于页面没有正确的交互，也有可能没有对一个XmlHttpRequest进行响应，这个时候就需要执行Javascript。当然，我们更加希望改进WebDriver来使其运行的连续而正确，而不是让测试人员来通过Javascript来调用。    
我们也感觉到有时因为种种限制，必须使用直接调用JavaScript。因此，对于支持JavaScript的浏览器，你可以直接将WebDriver实例强制转型为JavaScriptExecutor，然后执行。在Java语言中，类似于这样。
    
    WebDriver driver; // Assigned elsewhere
    JavascriptExecutor js =     (JavascriptExecutor) driver;
    js.executeScript("return document.title");
至于其他语言，都是很相似的方法。可以看一下UsingJavaScript章节。
####Q：为什么我执行Javascript,总是返回NULL？
>A:你需要让你的JS脚本有返回值。所以`js.executeScript("document.title");`会返回null;而`js.executeScript("return document.title");`则会返回页面的title。
####Q：使用XPATH定位元素，在有的浏览器可以，有的缺不行，为什么？
>A:简单来说，不同的浏览器对XPATH的处理略有不同。而你整好碰上了。具体参见[XpathInWebDriver](https://code.google.com/p/selenium/wiki/XpathInWebDriver)章节。
####Q：InternetExplorerDriver不能在Vista上很好的工作。我应该怎么做？
>A:InternetExplorerDriver需要所有的保护模式设置到相同的值（开启或关闭）。假如你不能修改这些，你也可以这样：  

    DesiredCapabilities capabilities = DesiredCapabilities.internetExplorer();
    capabilities.setCapability(InternetExplorerDriver.INTRODUCE_FLAKINESS_BY_IGNORING_SECURITY_DOMAINS, true);
    WebDriver driver = new InternetExplorerDriver(capabilities);    
正如常量名所示，你的所有测试可能需要分开。当然，如果你所有的站点都在同一个保护模式，那是没问题的。   
####Q：除了Java，还支持哪些语言？
>A:Python,Ruby,C#,Java都是直接支持的。并且还有PHP和Perl的webdriver实现。同时一个JS的API也正在计划中。
####Q：如何处理新弹出的浏览器窗口？
>A:WebDriver提供了处理多浏览器窗口的能力。通过使用`WebDriver.switchTo().window()`可以转向一个已知名字的窗口。假如不知道这个窗口的名字，可以使用`"WebDriver.getWindowHandles()`获取窗口的名字列表。然后就可以使用`switchTo().window()`来转向了。
####Q：WebDriver支持JavaScript弹出的alert和prompts框嘛？
>A: 使用[Alert API](http://selenium.googlecode.com/svn/trunk/docs/api/java/org/openqa/selenium/Alert.html)可以搞定:

    // Get a handle to the open alert, prompt or confirmation
    Alert alert = driver.switchTo().alert();
    // Get the text of the alert or prompt
    alert.getText();  
    // And acknowledge the alert (equivalent to clicking "OK")
    alert.accept();
####Q：WebDriver支持文件上传？
>A:答案是肯定的。你是不能直接与操作系统的浏览文件窗口直接交互的，但是做了一些神奇的工作，使得你在文件上传元素上调用`WebElement#sendKeys("/path/to/file")` 就可以正确上传。同时你要保证不要在文件上传元素上进行`WebElement#click()`操作，否则可能导致浏览器挂起。   
小提示：你是不能直接与隐藏的元素交互的，除非使他们显示出来。假如你的元素是隐藏的，可以使用类似下面的代码，来让元素可见。    

    ((JavascriptExecutor)driver).executeScript("arguments[0].style.visibility = 'visible';      arguments[0].style.height = '1px'; arguments[0].style.width = '1px'; arguments[0].style.opacity = 1", fileUploadElement); 
    
####Q：为什么在执行`sendKeys`后，没有触发onchange事件？
>A:WebDriver是将焦点一直放在调用`sendKeys`的元素上的。而onchange事件是当焦点离开元素才触发的。因此，你只需移动下焦点，简单的`click`下其他元素即可。   
####Q：能同时运行多个WebDriver的实例？
>A:HtmlUnitDriver,ChromeDriver,FirefoxDriver的每个实例都是完全独立的（其中firefox和chrome，每个实例都使用它们自己的匿名profile）。由于Windows自身的运行方式，同时只能有一个InternetExplorerDriver实例。假如你同时需要运行多个InternetExplorerDriver实例，可以考虑使用Remote!WebDriver以及虚拟机。   
####Q：我需要使用代理。我该如何配置呢？
>A：代理配置是通过`org.openqa.selenium.Proxy`类来实现的，类似下面代码所示：   

    Proxy proxy = new Proxy();
    proxy.setProxyAutoconfigUrl("http://youdomain/config");
    
    // We use firefox as an example here.
    DesiredCapabilities capabilities = DesiredCapabilities.firefox();
    capabilities.setCapability(CapabilityType.PROXY, proxy);
    
    // You could use any webdriver implementation here
    WebDriver driver = new FirefoxDriver(capabilities);
####Q：使用HtmlUnitDriver该如何实现权限验证？
>A:当创建HtmlUnitDriver时，重写`modifyWebClient`方法即可。例如：    

    WebDriver driver = new HtmlUnitDriver() {
      protected WebClient modifyWebClient(WebClient client) {
        // This class ships with HtmlUnit itself
        DefaultCredentialsProvider creds = new DefaultCredentialsProvider();
    
        // Set some example credentials
        creds.addCredentials("username", "password");
    
        // And now add the provider to the webClient instance
        client.setCredentialsProvider(creds);
    
        return client;
      }
    };
####Q：WebDriver是线程安全的吗？
>A:WebDriver不是线程安全的。话说回来，如果你串行的访问driver实例，你就可以在多个线程之间共享一个driver引用。这个不是推荐的方法。其实你可以为每个线程实例化一个WebDriver。    
####Q：如何向一个可编辑的iframe里输入？
>A:假设那个iframe的name是“foo”:   

    driver.switchTo().frame("foo");
    WebElement editable = driver.switchTo().activeElement();
    editable.sendKeys("Your text here");
有时，这方法不管用。那是因为iframe没有任何内容。在Firefox中，你可以在`sendKeys`之前做以下的操作：    

    ((JavascriptExecutor) driver).executeScript("document.body.innerHTML = '<br>'");
因为iframe内默认是没有任何内容的，所以就不知道该往哪儿进行键盘输入，上面的操作是必须的。这个操作只是插入了一个空标签，就把一切搞定了。   
记得在做完iframe内的操作后，switch出来（因为进一步都交互都是使用下面的frame的）。    

    driver.switchTo().defaultContent();
####Q：在Linux系统上WebDriver无法启动Firefox，并抛出`java.net.SocketException`。
>A:在Linux系统上运行WebDriver,Firefox无法启动并抛出如下错误：   

    Caused by: java.net.SocketException: Invalid argument
            at java.net.PlainSocketImpl.socketBind(Native Method)
            at java.net.PlainSocketImpl.bind(PlainSocketImpl.java:365)
            at java.net.Socket.bind(Socket.java:571)
            at org.openqa.selenium.firefox.internal.SocketLock.isLockFree(SocketLock.java:99)
            at org.openqa.selenium.firefox.internal.SocketLock.lock(SocketLock.java:63)
这可能是因为机器上的IPv6设置导致的。执行下面的脚本：  

    sudo sysctl net.ipv6.bindv6only=0
为了使用相同的调用，就能让socket同时绑定到主机的IPv6和IPv4地址。更长远的解决方案是编辑`/etc/sysctl.d/bindv6only.conf`禁用这个行为。   
####Q:WebDriver找不到元素/页面没有完全加载就返回了。
>A:这个问题可以从各种方式显现出来：  

- 使用WebDriver.findElement(...)抛出ElementNotFoundException，但当检查DOM（使用Firebug等工具）时，元素明明就在那里。
- 调用Driver.get时，一旦HTML加载，方法就立即返回了。但是onload触发的JavaScript代码没有加载。因此页面是没有加载完全的并且一些元素是无法找到的。
- click一个元素或者链接时，触发了一个新建元素的操作。然而，click操作返回后，直接调用findElement并不能找到这个新建的元素。click操作不应该是被阻塞的吗？
- 我怎么才能知道一个页面是否已经完全加载了？   

**解析**：WebDriver基本上都是blocking API。但是，在一些情况下，是允许页面没有完成加载就让get请求返回的。经典的样例就是，当页面加载后才开始执行JavaScript(onload触发)。浏览器（比如Firefox）会在基本的HTML内容加载后通知WebDriver，而就是此刻WebDriver才返回了。要想知道JavaScript什么时候执行完成是困难的（即使是能知道），因为JS代码可能在将来的某一刻才执行并且依赖于服务器的响应。这种情况同样也适用于click操作，在支持原生事件（Window,Linux）的系统平台上，click操作会转化为在操作系统层面的一个在元素坐标点进行的鼠标左键点击，WebDriver是无法很好的追踪到这个鼠标左键点击引起的一连串操作的。WebDriver并不了解这一切，所以WebDriver不可能等到所有的条件到来才执行测试流程。这样的情况下，blocking API是不完美的。通常，我们最为关心的是在接下来的交互中需要使用到元素是否已经显示并且可用。    
**解决方案**：使用Wait类来等待一个元素出现。这个类会一遍又一遍的调用findElement方法，忽略NoSuchElementException，直到找到元素为止（或者超时失效）。既然这个行为是很多用户希望默认实现的，我们实现了一个隐式等待元素出现的机制。这个可以使用[WebDriver.manage().timeouts()](http://selenium.googlecode.com/svn/trunk/docs/api/java/org/openqa/selenium/WebDriver.Timeouts.html)来实现.    
####Q:如何能触发页面上的任意事件？
>A:WebDriver致力于模拟用户交互，因此API直接反应了用户与各种元素的交互。   
触发一个特定的事件不能由API直接完成，但是可以直接通过执行JavaScript来调用元素上的各种方法。     
####Q:为什么不能与隐藏的元素进行交互？
>A:既然用户不能看到隐藏元素的文本信息，WebDriver也一样。
然而，执行JavaScript来直接调用隐藏元素的getText是允许的。

    WebElement element = ...;
    ((JavascriptExecutor) driver).executeScript("return arguments[0].getText();", element);
####Q:如何启动一个带插件的Firefox？
>A:

    FirefoxProfile profile = new FirefoxProfile()
    profile.addExtension(....);
    
    WebDriver driver = new FirefoxDriver(profile);
####Q: 要是WebDriver有...功能，我会更喜欢它。
>A: 如果你希望WebDriver有什么功能，或者发现有什么BUG。你可以添加一个issue到WebDriver主页。    
####Q: 有时Selenium server启动一个新session的时候要花费很长的时间？
>A:如果运行在Linux上，你需要增加用于安全随机数生成所需要的熵数量。大多数的Linux发行版都可以通过安装一个叫“randomsound”的包来配置。  
在Windows(XP)系统上，你可以看下<http://bugs.sun.com/view_bug.do?bug_id=6705872>,这通常需要从你的临时文件夹清理大量的数据文件。
####Q: 在Selenium WebDriver的API中哪个与TextPresent对等?
>A:    

    driver.findElement(By.tagName("body")).getText()
    
会给出页面的文本内容。关于verifyTextPresent/assertTextPresent,你需要使用Test framework的Assert来验证。关于waitForTextPresent, 你需要使用WebDriverWait类来解决。

####Q:socket lock感觉是一个糟糕的设计。我如何能更好的实现。
>A:守护Firefox启动的socket lock在设计时，有以下的限制：   

- socket lock需要在所有语言绑定之间共享。ruby,java以及其他语言的绑定可以在相同的机器同时共存。
- 启动firefox的某些关键部分必须在机器上独占锁定。
- socket lock本身不是瓶颈。启动firefox才是。

`SocketLock`是`Lock`接口的一个实现. 这给你自己的接口实现提供了一个可插拔的策略。为了切换到一个不同的实现，你需要继承FirefoxDriver并且重写“obtainLock”方法。

####Q: 当我使用Python的send_keys方法时，为什么出现一个UnicodeEncodeError？
>A: 你很可能没有设置系统的Locale。比如设置LANG=en_US.UTF-8和LC_CTYPE="en_US.UTF-8"就可以了.