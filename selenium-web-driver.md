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









