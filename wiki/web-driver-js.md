# WebDriverJS

WebDriver 的 JavaScript 语言绑定。本文包含以下内容：

- 介绍
- 快速上手
    - 在 Node 中运行
    - 在浏览器中运行
- 设计细节
    - 管理异步 API
    - 同服务端通讯
    - /xdrpc
- 未来计划

## 介绍

WebDriver 的 JavaScript 绑定（WebDriverJS），可以使 JavaScript 开发人员避免上下文切换的开销，并且可以让他们使用和项目开发代码一样的语言来编写测试。WebDriverJS 既可以在服务端运行，例如 Node，也可以在浏览器中运行。

**警告：** WebDriverJS 要求开发者习惯异步编程。对于那些 JavaScript 新手来说可能会发现 WebDriverJS 有点难上手。

## 快速上手

### 在 Node 中运行

虽然 WebDriverJS 可以在 Node 中运行，但它至今还没有实现本地驱动的支持（也就是说，你的测试必须使用一个远程的 WebDriver 服务）。并且，你必须编译 Selenium 服务端，将其添加到 WebDriverJS 模块。进入 Selenium 客户端的根目录，执行：

    $ ./go selenium-server-standalone //javascript/node:webdriver
    
当两个目标都被编译好以后，启动服务和 Node，开始编写测试代码：

    $ java -jar build/java/server/src/org/openqa/grid/selenium/selenium-standalone.jar &
    $ node
    
    var webdriver = require('./build/javascript/node/webdriver');
    
    var driver = new webdriver.Builder().
        usingServer('http://localhost:4444/wd/hub').
        withCapabilities({
          'browserName': 'chrome',
          'version': '',
          'platform': 'ANY',
          'javascriptEnabled': true
        }).
        build();
    
    driver.get('http://www.google.com');
    driver.findElement(webdriver.By.name('q')).sendKeys('webdriver');
    driver.findElement(webdriver.By.name('btnG')).click();
    driver.getTitle().then(function(title) {
      require('assert').equal('webdriver - Google Search', title);
    
    });
    
    driver.quit();

### 在浏览器中运行

除了 Node，WebDriverJS 也可以直接在浏览器中运行。编译比Node方式少很多依赖的浏览器模块，运行：

    $ ./go //javascript/webdriver:webdriver

为了和可能不在同一个域下的 WebDriver 的服务端进行通信，客户端使用的是修改过的 [JsonWireProtocol](https://code.google.com/p/selenium/wiki/JsonWireProtocol) 和 [cross-origin resource sharing](https://code.google.com/p/selenium/wiki/WebDriverJs#Cross-Origin_Resource_Sharing)：

    <!DOCTYPE html>
    <script src="webdriver.js"></script>
    <script>
      var client = new webdriver.http.CorsClient('http://localhost:4444/wd/hub');
      var executor = new webdriver.http.Executor(client);
    
      // 启动一个新浏览器，这个浏览器可以被这段脚本控制
      var driver = webdriver.WebDriver.createSession(executor, {
        'browserName': 'chrome',
        'version': '',
        'platform': 'ANY',
        'javascriptEnabled': true
      });
    
      driver.get('http://www.google.com');
      driver.findElement(webdriver.By.name('q')).sendKeys('webdriver');
      driver.findElement(webdriver.By.name('btnG')).click();
      driver.getTitle().then(function(title) {
        if (title !== 'webdriver - Google Search') {
          throw new Error(
              'Expected "webdriver - Google Search", but was "' + title + '"');
        }
      });
    
      driver.quit();
    </script>

#### 控制宿主浏览器

启动一个浏览器运行 WebDriver 来测试另一个浏览器看起来比较冗余（相比在 Node 中运行而言）。但是，使用 WebDriverJS 在浏览器中运行自动化测试是浏览器真实在跑这些脚本的。这只要服务端的 url 和浏览器的 session id 是已知的就可以实现。这些值可能会直接传递给 builder，它们也可以通过从页面 url 的查询字符串中解析出来的 wdurl 和 wdsid 定义 。

    <!-- Assuming HTML URL is /test.html?wdurl=http://localhost:4444/wd/hub&wdsid=foo1234 -->
    <!DOCTYPE html>
    <script src="webdriver.js"></script>
    <input id="input" type="text"/>
    <script>
      // Attaches to the server and session controlling this browser.
      var driver = new webdriver.Builder().build();
    
      var input = driver.findElement(webdriver.By.tagName('input'));
      input.sendKeys('foo bar baz').then(function() {
        assertEquals('foo bar baz',
            document.getElementById('input').value);
      });
    </script>

##### 警告

在浏览器中使用 WebDriverJS 有几个需要注意的地方。首先，webdriver.Builder 类只能用于已存在的 session。为了获得一个新的 session，你必须像上面的例子那样手工创建。其次，有一些命令可能会影响运行 WebDriverJS 脚本的页面。

- webdriver.WebDriver#quit: quit 命令将终止整个浏览器进程，包括在运行 WebDriverJS 的窗口。除非你确定要这样做，否则不要使用这个命令。
- webdriver.WebDriver#get: WebDriver 的接口被设计为尽量接近用户的操作。这意味着无论 WebDriver 客户端当前聚焦在哪个帧，导航命令（如：driver.get(url)）总是指向最高层的帧。在操作宿主浏览器时，WebDriverJS 脚本可以通过使用 .get 命令导航离开当前页面，而当前页面仍然获得焦点。 如果要自动操作一个宿主浏览器但仍想在页面间跳转，请把WebDriver客户端的焦点设在另一个窗口上(这和Selenium RC 的多窗口模式的概念非常相似):


    <!DOCTYPE html>
    <script src="webdriver.js"></script>
    <script>
      var testWindow = window.open('', 'slave');
    
      var driver = new webdriver.Builder().build();
      driver.switchTo().window('slave');
      driver.get('http://www.google.com');
      driver.findElement(webdriver.By.name('q')).sendKeys('webdriver');
      driver.findElement(webdriver.By.name('btnG')).click(); 
    </script>

#### 调试 Tests

你可以使用 WebDriver 的服务来调试在浏览器中使用  WebDriverJS 运行的测试。

    $ ./go selenium-server-standalone
    $ java -jar \
        -Dwebdriver.server.session.timeout=0 \
        build/java/server/src/org/openqa/grid/selenium/selenium-standalone.jar &

启动服务后，访问 WebDriver 的控制面板： http://localhost:4444/wd/hub。你可以使用这个控制面板查看，创建或者删除 sessions。选择一个要调试的 session 后，点击 “load script” 按钮。在弹出的对话框中，输入你的 WebDriverJS 测试的地址：服务端将在你的浏览器中打开这个页面，这个页面的 url 含有额外的参数用于 WebDriverJS 客户端和服务端的通讯。

##### 支持的浏览器

- IE 8+
- Firefox 4+
- Chrome 12+
- Opera 12.0a+
- Android 4.0+

## 设计细节

### 管理异步 API

不同于其他那些提供了阻塞式 API 的语言绑定，WebDriverJS 完全是异步的。为了追踪每个命令的执行状态， WebDriverJS 对 “promise” 进行了扩展。promise 是一个这样的对象，它包含了在未来某一点可用的一个值。JavaScript 有几个 promise 的实现，WebDriverJS 的 promise 是基于 CommonJS 的 [Promise/A](http://www.google.com/url?q=http%3A%2F%2Fwiki.commonjs.org%2Fwiki%2FPromises%2FA&sa=D&sntz=1&usg=AFQjCNGC0NMXO-81exam-S5HjTuOxaV_mw) 提议，它定义了 promise 是任意对象上的 then 函数属性。 

    /**
     * Registers listeners for when this instance is resolved.
     *
     * @param {?function(*)} callback The function to call if this promise is
     *     successfully resolved. The function should expect a single argument: the
     *     promise's resolved value.
     * @param {?function(*)=} opt_errback The function to call if this promise is
     *     rejected. The function should expect a single argument: the failure
     *     reason. While this argument is typically an {@code Error}, any type is
     *     permissible.
     * @return {!Promise} A new promise which will be resolved
     *     with the result of the invoked callback.
     */
    Promise.prototype.then = function(callback, opt_errback) {
    };

通过使用 promises，你可以将一连串的异步操作连接起来，确保每个操作执行时，它之前的操作都已经完成：

    var driver = new webdriver.Builder().build();
    driver.get('http://www.google.com').then(function() {
      return driver.findElement(webdriver.By.name('q')).then(function(searchBox){
        return searchBox.sendKeys('webdriver').then(function() {
          return driver.findElement(webdriver.By.name('btnG')).then(function(submitButton) {
            return submitButton.click().then(function() {
              return driver.getTitle().then(function(title) {
                assertEquals('webdriver - Google Search', title);
              });
            });
          });
        });
      });
    });

不幸的是，上述范例非常冗长，难以辨别测试的意图。为了提供一套不降低测试可读性的干净利落的异步操作 API, WebDriverJS 引入了一个 promise “管理器” 来调度和执行所有的命令。

简言之，promise 管理器处理用户自定义任务的调度和执行。管理器保存了一个任务调度的列表，当列表中的某个任务执行完毕后，依次执行下一个任务。如果一个任务返回了一个 promise，管理器将把它当做一个回调注册，在这个 promise 完成后恢复其运行。WebDriver 将自动使用管理器，所以用户不需要使用链式调用。因此，之前的 google 搜索的例子可以简化成：

    var driver = new webdriver.Builder().build();
    driver.get('http://www.google.com');
    
    var searchBox = driver.findElement(webdriver.By.name('q'));
    searchBox.sendKeys('webdriver');
    
    var submitButton = driver.findElement(webdriver.By.name('btnG'));
    submitButton.click();
    
    driver.getTitle().then(function(title) {
      assertEquals('webdriver - Google Search', title);
    });

#### On Frames and Callbacks

就内部而言，promise 管理器保存了一个调用栈。在管理器执行循环的每一圈，它将从最顶层帧的队列中取一个任务来执行。任何被包含在之前命令的回调中的命令将被排列在一个新帧中，以确保它们能在所有早先排列的任务之前运行。这样做的结果是，如果你的测试是 written-in line，所有的回调都使用函数字面量定义，命令将按照它们在屏幕上出现的垂直顺序来执行。例如，考虑以下 WebDriverJS 测试用例：

    driver.get(MY_APP_URL);
    driver.getTitle().then(function(title) {
      if (title === 'Login page') {
        driver.findElement(webdriver.By.id('user')).sendKeys('bugs');
        driver.findElement(webdriver.By.id('pw')).sendKeys('bunny');
        driver.findElement(webdriver.By.id('login')).click();
      }
    });
    driver.findElement(webdriver.By.id('userPreferences')).click();

这个测试用例可以使用 WebDriver 的 Java API 重写如下：

    driver.get(MY_APP_URL);
    if ("Login Page".equals(driver.getTitle())) {
      driver.findElement(By.id("user")).sendKeys("bugs");
      driver.findElement(By.id("pw")).sendKeys("bunny");
      driver.findElement(By.id("login")).click();
    }
    driver.findElement(By.id("userPreferences")).click();

#### 错误处理

既然所有 WebDriverJS 的操作都是异步执行的，我们就不能使用 try-catch 语句。取而代之的是，你必须为所有命令的 promise 返回注册一个错误处理的函数。这个错误处理函数可以抛出一个错误，在这种情况下，它将被传递给链中的下一个错误处理，或者他将返回一个不同的值来抑制这个错误并切换回回调处理链。

如果错误处理器没有正确的处理被拒绝的 promise（不只是哪些来自于 WebDriver 命令的），则这个错误会传播至错误处理链的父级帧。如果一个错误没有被抑制而传播到了顶层帧，promise 管理器要么触发一个 uncaughtException 事件（如果有注册监听的话），或者将错误抛给全局错误处理器。在这两种情况下，promise 管理器都将抛弃所有队列中后续的命令。

    // 注册一个事件监听未处理的错误
    webdriver.promise.Application.
        getInstance().
        on('uncaughtException', function(e) {
          console.error('There was an uncaught exception: ' + e.message);
        });
    
    driver.switchTo().window('foo').then(null, function(e) {
      // 忽略 NoSuchWindow 错误，让其他类型的错误继续向上冒泡
      if (e.code !== bot.ErrorCode.NO_SUCH_WINDOW) {
        throw e;
      }
    });
    // 如果上面的错误不被抑制的话，这句将永远不会执行
    driver.getTitle();

### 同服务端通讯

当在服务端环境中运行时，客户端不受安全沙箱的约束，可以简单的发送 http 请求（例如：node 的 http.ClientRequest）。当在浏览器端运行时，WebDriverJS 客户端就会收到同源策略的约束。为了和可能不在同一个域下的服务端通讯，WebDriverJS 客户端使用的是修改过的 JsonWireProtocol 和 cross-origin resource sharing。

#### Cross-Origin Resource Sharing

如果一个浏览器支持 cross-origin resource sharing (CORS), WebDriverJS 将使用 cross-origin XMLHttpRequests (XDR) 发送命令给服务端。服务端要想支持 XDR，就需要响应 preflight 请求，并带有合适的 access-control 头。

    Access-Control-Origin: *
    Access-Control-Allow-Methods: DELETE,GET,HEAD,POST
    Access-Control-Allow-Headers: Accept,Content-Type

在编写本文时，已有 Firefox 4+, Chrome 12+, Safari 4+, Mobile Safari 3.2+, Android 2.1+, Opera 12.0a, 和 IE8+ 支持 CORS。不幸的是，这些浏览器的实现并不一致，也不是完全都遵循 W3C 的规范。

- IE 的 XDomainRequest 对象，比其 XMLHttpRequest 对象的功能要弱。XDomainRequest 只能发送哪些标准的 form 表单可以发送的请求。这限制了 IE 只能发送 get 和 post 请求（wire 协议要求支持 delete 请求）。
- WebKit 的 CORS 实现禁止了跨域请求的重定向，即使 access-control 头被正确设置了也是如此。
- 如果返回一个服务端错误（4xx 或 5xx），IE 和 Opera 的实现将触发 XDomainRequest/XMLHttpRequest 对象的错误处理，但是拿不到服务端返回的信息。这使得它们无法处理以标准的 JSON 格式返回的错误信息。

为了弥补这些短处，当在浏览器中运行时，WebDriverJS 将使用修改过的 JsonWireProtocol 和通过 /xdrpc 路由所有的命令。

#### /xdrpc

**POST /xdrpc**

作为命令的代理，所有命令相关的内容必须被编码成 JSON 格式。命令的执行结果将在 HTTP 200 响应中作为一个标准的响应结果返回。客户端依赖于响应的转台吗以确认命令是否执行成功。

**参数：**

- method - {string} http 方法
- path - {string} 命令路径
- data - {Object} JSON 格式的命令参数

**返回：**

{*} 命令执行的结果。

举个例子，考虑以下 /xdrpc 命令：

    POST /xdrpc HTTP/1.1
    Accept: application/json
    Content-Type: application/json
    Content-Length: 94
    
    {"method":"POST","path":"/session/123/element/0a/element","data":{"using":"id","value":"foo"}}
    
服务端将编码这个命令并重新分发：

    POST /session/123/element/0a/element HTTP/1.1
    Accept: application/json
    Content-Type: application/json
    Content-Length: 28
    
    {"using":"id","value":"foo"}
    
不管是否成功，命令的执行结果都将作为一个标准的 JSON 返回：

    HTTP/1.1 200 OK
    Content-Type: application/json
    Content-Length: 60
    
    {"status":7,"value":{"message":"Unable to locate element."}}

## 未来计划

以下是一些预期要做的事情。但什么时候完成，在现在仍然未知。如果你有兴趣参与开发，请加入 selenium-developers@googlegroups.com。当然，这是一个开源软件，你完全不需要等待我们。如果你有好主意，就马上开工吧：）

- 使用 AutomationAtoms 实现一个纯 JavaScript 的命令执行器。这将允许开发者使用 js 编写非常轻量的测试代码，并且可以运行在任何服浏览器中（当然，仍然会收到同源策略的限制）。
- 基于扩展实现一个 SafariDriver。
- 为 Node 提供本地浏览器支持，而不需要通过 WebDriver Server 运行。















