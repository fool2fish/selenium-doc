# 高级用户交互

原文：<https://code.google.com/p/selenium/wiki/AdvancedUserInteractions>

##入门

高级用户交互API提供了一个更新更完善的机制来定义并描述用户在一个网页上的各种操作。这些操作包括：拖拽、按住CTRL键选择多个元素等等。    

### 快速上手

为了生成一连串的动作，我们使用Actions来建立。首先，我们先配置操作：   

    Actions builder = new Actions(driver);
    
    builder.keyDown(Keys.CONTROL)
    .click(someElement)
    .click(someOtherElement)
    .keyUp(Keys.CONTROL);
    
然后，获得操作（Action）:    

    Action selectMultiple = builder.build();

最后，执行这个动作：   

    selectMultiple.perform();   

这一系列的动作应该尽量的短。在使用中最好在执行一个简短的动作后验证页面是否处于正确的状态，然后再执行下面的动作。下一节将会列出所有可用的动作（Action），并且说明它们如何进行扩展。     

### 键盘交互（Keyboard interactions）

键盘交互是发生在一个特定的页面元素的，而webdriver会确保这个页面元素在执行键盘动作时处于正确的状态。这个正确的状态，包括页面元素滚动到可视区域并定位到这个页面元素。   
既然这个新的API是面向用户（user-oriental）的接口，那么对于一个用户，在对一个元素输入文本前做显式的交互就更加的符合逻辑。这意味着，当想定位到相邻的页面元素时，可能需要点击一下元素或按下Tab（`Keys.TAB`）键。  
The new interactions API will (first) support keyboard actions without a provided element. The additional work to focus on an element before sending it keyboard events will be added later on.     

### 鼠标交互（Mouse interactions）

鼠标操作有一个上下文-鼠标的当前位置。因此，当为几个鼠标操作设定一个上下文时，第一个操作的上下文就是元素的相对位置，下一个操作的上下文就上一个操作后的鼠标相对位置。   

## 支持情况

这个针对操作以及动作生成器的API已经（绝大部分）完成。HtmlUnit和Firefox已经完全支持，Opera和IE正在支持中。    

## 大纲

### 单个动作

所有的动作都实现了`Action`接口，这个接口只有一个方法：`perform（）`。每个动作所需要的信息都通过Constructor传入。当调用这个动作的时候，动作知道如何与页面交互（如，找到活动的元素并输入文本或者计算出在屏幕上的点击坐标）并且调用底层实现来实现这个交互。   
下面是一些动作：   

- ButtonReleaseAction - 释放鼠标左键
- ClickAction - 相当于 WebElement.click()
- ClickAndHoldAction - 按下鼠标左键并保持
- ContextClickAction - 一般就是鼠标右键，通常调出右键菜单用。
- DoubleClickAction - 双击某个元素
- KeyDownAction - 按下修饰键（SHIFT，CTRL，ALT，WIN）
- KeyUpAction - 释放修饰键
- MoveMouseAction - 移动鼠标从当前位置到另外的元素.
- MoveToOffsetAction - 移动鼠标到一个元素的偏移位置（偏移可以为负，元素是鼠标刚移动到的那个元素）。
- SendKeysAction - 相当于 WebElement.sendKey(...)

`CompositeAction`包含一系列的动作，当被调用的时候，它会调用它所包含的所有动作的perform方法。通常，这个动作通常都不是直接建立的，一般是使用`ActionChainsGenerator`。   

### 生成动作链

`Actions`链生成器实现了创建者模式来新建一个包含一组动作的`CompositeAction`。使用Actions生成器可以很容易的生成动作并调用`build（）`方法来获得复杂的操作。    

    Actions builder = new Actions(driver);
    
    Action dragAndDrop = builder.clickAndHold(someElement)
       .moveToElement(otherElement)
       .release(otherElement)
       .build();
    
    dragAndDrop.perform();

有一个对`Actions`进行扩展的计划，给`Actions`类添加一个方法，这个方法可以追加任何动作到其拥有的动作列表上。这将允许添加扩展的动作，而不用人工创建CompositeAction。关于扩展`Actions`,请往下看。    

### 扩展Action接口的指导

Action接口只有一个方法-`perform()`。除了实际的交互本身，所有的条件判断也都应该在这个这个方法里实现。在动作创建和动作实际执行这段时间内，很可能页面的状态已经发生了变化，比如元素的可视情况已经坐标已经不能找到了。   

## 实现细节

为了达到每个操作的执行与具体实现的分离，所有的动作都依赖2个接口：`Mouse`和`Keyboard`。这些接口被所有支持高级用户接口API的driver实现了。需要注意的是，这些接口是为了让动作使用的，而不是最终用户。本节的信息，主要是针对想扩展WebDriver的开发者的。   

### 一字警告

`Keyboard`和`Mouse`接口是设计用来支持各种Action类的。有鉴于此，它们的API没有Actions链生成器API稳定。直接使用这些接口可能达不到期望的结果，因为Action类做了额外的工作来确保在实际事件触发时处于正确的环境条件。这些准备工作包括定位在正确的元素上或者鼠标交互前保证元素是可见的。    

### Keyboard接口

Keyboard接口包含3个方法：    

- void sendKeys(CharSequence... keysToSend) - 与 sendKeys(...)相似.
- void pressKey(Keys keyToPress) - 按下一个键并保持。键仅限于修饰键(Control, Alt and Shift).
- void releaseKey(Keys keyToRelease) - 释放修饰键.

至于如何在调用之间保存修饰键的状态是Keyboard接口实现类的职责。只有活跃的元素才会接收到这些事件。   

### Mouse接口

`Mouse`接口包含以下方法（有可能不久之后会有变化）：

- void click(WebElement onElement) - 同click()方法一样.
- void doubleClick(WebElement onElement) - 双击一个元素.
- void mouseDown(WebElement onElement) - 在一个元素上按下左键并保持
Action selectMultiple = builder.build();
- void mouseUp(WebElement onElement) - 在一个元素上释放左键.
- void mouseMove(WebElement toElement) - 从当前位置移动到一个元素
- void mouseMove(WebElement toElement, long xOffset, long yOffset) - 从当前位置移动到一个元素的偏移坐标
- void contextClick(WebElement onElement) - 在一个元素上做一个右键操作

### Native events（原生事件） VS synthetic events（合成事件）

WebDriver提供的高级用户接口，要么是直接模拟的Javascript事件（即合成事件），要么就是让浏览器生成Javascript事件（即原生事件）。原生事件能更好的模拟用户交互，而合成事件则是平台独立的，这使得使用了替代的窗口管理器的linux系统显得尤其重要，具体参加[native events on Linux](https://code.google.com/p/selenium/wiki/NativeEventsOnLinux)。原生事件无论什么时候总是应该尽可能的使用。   

下面的表格展示了浏览器对事件的支持情况。   

<table border="1px">
<tr>
<td>浏览器</td><td>操作系统</td><td>原生事件</td><td>合成事件</td>
</tr>
<tr>
<td>Firefox</td><td>Linux</td><td>支持</td><td>支持（默认）</td>
</tr>
<tr>
<td>Firefox</td><td>Windows</td><td>支持（默认）</td><td>支持</td>
</tr>
<tr>
<td>Internet Explorer</td><td>Windows</td><td>支持（默认）</td><td>不支持</td>
</tr>
<tr>
<td>Chrome</td><td>Linux/Windows</td><td>支持*</td><td>不支持</td>
</tr>
<tr>
<td>Opera</td><td>Linux/Windows</td><td>支持（默认）</td><td>不支持</td>
</tr>
<tr>
<td>HtmlUnit</td><td>Linux/Windows</td><td>支持（默认）</td><td>不支持</td>
</tr>
</table>

ChromeDriver提供了2种模式来支持原生事件：Webkit事件和原始事件。其中Webkit事件是使用Webkit函数来触发的Javascript事件，而原始事件模式则使用的是操作系统级别的事件。     

FirefoxDriver中，原生事件可以使用FirefoxProfile来进行开关控制。    

    FirefoxProfile profile = new FirefoxProfile();
    profile.setEnableNativeEvents(true);
    FirefoxDriver driver = new FirefoxDriver(profile);

#### 例子

以下是原生事件与合成事件表现不同的一些例子：   

- 使用合成事件，点击隐藏在其他元素下面的元素是可以的。使用原生事件，浏览器会将点击事件作用在所给坐标最外层的元素上，就像是用户点击在特定的位置一样。   
- 当一个用户，按下TAB键希望焦点从当前元素定位到下一个元素，浏览器是可以做到的。使用合成事件的话，浏览器并不知道TAB键被按下了，因此也不会改变焦点。而使用原生事件，浏览器则会表现正确。   
