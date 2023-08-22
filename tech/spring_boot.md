*本文翻译自Spring Boot官方文档的Getting Started部分，原文可[点击此处](http://docs.spring.io/spring-boot/docs/1.3.6.RELEASE/reference/htmlsingle/#getting-started)查看。*

如果你刚开始学习使用Spring Boot，或者Spring框架，那么这篇文章正是为你准备的！通过这篇文章，我们将介绍关于Spring Boot的一些基本知识。你将从接下来的章节中了解到Spring Boot的安装命令，我们将一起构建我们的第一个Spring Boot应用，然后我们将一起讨论一些核心原则。

## Spring Boot简介

Spring Boot可以帮助我们轻易创建出可独立运行的，基于Spring的生产级应用，这些应用甚至可以直接运行而不依赖其他环境(仅需要JVM)。我们用统一的方式来看待Spring平台以及第三方库，因此你在使用的过程中不会有太多混乱的情况出现，并且大部分Spring Boot应用几乎不需要处理烦人的Spring配置。
使用Spring Boot你可以创建一个可直接用`java -jar`命令运行的包，当然也可以创建传统的war包，我们还提供了一个命令行工具用来运行“spring脚本”。

我们的主要目的是：
- 提供一个急速的并且随手可得的Spring项目开发体验。
- 提供稳定的开箱即得的开发方式，同时又能迅速的在默认配置的基础上做出按需的配置。
- 提供大量的公共的非功能性特性(例如：嵌入式服务器，权限管理，性能监控，健康检查，外部配置等)。
- 绝对不需要代码生成并且不需要XML配置文件。

## 系统要求

默认情况下Spring Boot 1.3.6.RELEASE要求[Java 7](http://www.java.com/)和Spring Framework 4.2.7.RELEASE或更高版本。你也可以使用Java 6，但是需要一些额外的配置。我们为Maven (3.2+)和Gradle (1.12+)提供了构建支持。
> 虽然你可以在Java 6或Java 7环境下使用Spring Boot，但我们通常建议如果有可能尽量使用Java 8。

### Servlet容器

默认包含下列嵌入式服务器：

|        Name      | Servlet Version |  Java Version  |
| ----------- | ------------- | ----------- |
|    Tomcat 8    |          3.1          |      Java 7+     |
|    Tomcat 7    |          3.0          |      Java 6+     |
|       Jetty 9      |          3.1          |      Java 7+     |
|      Jetty 8       |          3.0          |      Java 6+     |
| Undertow 1.1 |          3.1          |      Java 7+     |

你可以将Spring Boot应用部署到任何的Servlet 3.0+兼容的容器中。

## 安装Spring Boot

Spring Boot可以使用“传统”的Java开发工具或者作为一个命令行工具安装。但无论如何，你需要[Java SDK v1.6](http://www.java.com/)以上的开发环境。 你可以用以下命名来查看你的Java版本：

```bash
$ java -version
```

如果你是一个Java开发的新手或者你只是想体验一下Spring Boot，你可能首先想试试[Spring Boot CLI](http://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#getting-started-installing-the-cli)，你也可以继续阅读下面的内容来了解“传统”方式的使用。

> 虽然Spring Boot兼容Java 1.6，但如果可能的话，请尽量使用最新版本的Java环境。

### 安装指令

你可以像任何标准的Java库一样的方式使用Spring Boot，只需要简单的将相关的spring-boot-\*.jar文件引入到你的classpath中。Spring Boot不要求任何特殊的工具集成，所以你可以使用任何IDE或者文本编辑器，Spring Boot应用没有任何特殊的地方，所以你可以像其他Java程序一样运行和调试。
虽然你* 可以 *简单的把Spring Boot的jar文件复制到你的项目中，但通常的做法是使用一些支持依赖管理的构建工具(例如Maven或者Gradle)。

#### Maven安装

Spring Boot兼容Apache Maven 3.2或更高版本。如果你没有安装Maven，你可以按照[maven.apache.org](http://maven.apache.org/)中的方法安装。

> 在很多操作系统上都可以通过相应的包管理器来安装Maven。如果你是OSX Homebrew的用户，你可以通过命令`brew install maven`来安装，而Ubuntu用户可以运行`sudo apt-get install maven`来安装。

Spring Boot的依赖使用`org.springframework.boot`作为`groupId`，通常你的Maven POM文件需要继承自`spring-boot-starter-parent`项目，然后声明一个或多个[“Starter POMs”](http://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#using-boot-starter-poms)依赖。Spring Boot还提供了一个可选的[Maven plugin](http://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#build-tool-plugins-maven-plugin)用来创建可执行的jar包。

一个常规的pom.xml如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.example</groupId>
    <artifactId>myproject</artifactId>
    <version>0.0.1-SNAPSHOT</version>

    <!-- 继承Spring Boot默认配置 -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.3.6.RELEASE</version>
    </parent>

    <!-- 添加一个典型的web项目配置 -->
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>

    <!-- 可执行jar包打包插件 -->
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>
```

> 使用`spring-boot-starter-parent`是一个使用Spring Boot非常好的方式，但并不总是所有情况下都是最适合的方式，有时你可能需要你的项目继承自其它的父项目POM，或者你只是单纯的不喜欢我们的POM设置，我们下面有专门的一个章节来介绍不依赖我们的父项目POM如何使用Spring Boot。

#### Gradle安装

Spring Boot兼容Gradle 1.12或更高版本。如果你还没有安装Gradle，你可以按照[www.gradle.org/](http://www.gradle.org/)中的方法安装。
Spring Boot依赖使用`org.springframework.boot`作为`group`。
通常你的项目需要声明一个或多个[“Starter POMs”](http://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#using-boot-starter-poms)依赖。Spring Boot提供了一个[Gradle plugin](http://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#build-tool-plugins-gradle-plugin)来帮助你创建可执行的jar包。

> **Gradle Wrapper**

> Gradle Wrapper提供了一个很优雅的方式将Gradle“包含”到你需要编译的项目中，它是一个可以和代码一起提交的小型的脚本和库，帮助你启动一个编译进程。详情参看[这里](http://www.gradle.org/docs/current/userguide/gradle_wrapper.html)。

一个常规的build.gradle文件如下：

```groovy
buildscript { 
    repositories { 
        jcenter() 
        maven { url "http://repo.spring.io/snapshot" } 
        maven { url "http://repo.spring.io/milestone" } 
    } 
    dependencies { 
        classpath("org.springframework.boot:spring-boot-gradle-plugin:1.3.6.RELEASE") 
    }
}

apply plugin: 'java'
apply plugin: 'spring-boot'

jar { 
    baseName = 'myproject' version = '0.0.1-SNAPSHOT'
}

repositories { 
    jcenter() 
    maven { url "http://repo.spring.io/snapshot" } 
    maven { url "http://repo.spring.io/milestone" }
}

dependencies { 
    compile("org.springframework.boot:spring-boot-starter-web")
    testCompile("org.springframework.boot:spring-boot-starter-test")
}
```

### 安装Spring Boot CLI

Spring Boot CLI是一个命令行工具，可以帮助你快速建立Spring项目原型。它允许你运行[Groovy](http://groovy.codehaus.org/)脚本，与Java语法类似但不需要太多的脚手架式的语法。

你并不需要在你的Spring Boot项目中使用CLI工具，但它绝对是落地一个Spring应用最快的方式。

#### 手动安装

你可以在下面的Spring软件源地址下载Spring CLI：
- [spring-boot-cli-1.3.6.RELEASE-bin.zip](http://repo.spring.io/release/org/springframework/boot/spring-boot-cli/1.3.6.RELEASE/spring-boot-cli-1.3.6.RELEASE-bin.zip)
- [spring-boot-cli-1.3.6.RELEASE-bin.tar.gz](http://repo.spring.io/release/org/springframework/boot/spring-boot-cli/1.3.6.RELEASE/spring-boot-cli-1.3.6.RELEASE-bin.tar.gz)

你也可以选择最前沿的[snapshot分发包](http://repo.spring.io/snapshot/org/springframework/boot/spring-boot-cli/)。

下载完成后的压缩包中可以找到[INSTALL.txt](http://raw.github.com/spring-projects/spring-boot/v1.3.6.RELEASE/spring-boot-cli/src/main/content/INSTALL.txt)，按照其中的指令进行安装。大体上如下：在`.zip`文件的`bin/`目录下有一个`spring`脚本(Windows下是`spring.bat`)，或者你也可以直接通过`java -jar`命令运行`.jar`文件(使用上述的脚本可以帮助你确保classpath正确设置) 。

#### 使用SDKMAN! 安装

SDKMAN! (The Software Development Kit Manager)可以用来管理多种SDK的多个版本，包括Groovy和Spring Boot CLI。从[sdkman.io](http://sdkman.io/)获取SDKMAN!然后通过下面的命令来安装Spring Boot：

```bash
$ sdk install springboot
$ spring --version
Spring Boot v1.3.6.RELEASE
```

如果你在开发有关CLI的特性，并且想要方便的获取你刚刚构建的版本，可以使用下面的额外指令：

```bash
$ sdk install springboot dev /path/to/spring-boot/spring-boot-cli/target/spring-boot-cli-1.3.6.RELEASE-bin/spring-1.3.6.RELEASE/
$ sdk default springboot dev
$ spring --version
Spring CLI v1.3.6.RELEASE
```

这将会安装一个名叫`dev`实例的`spring`本地实例，这个实例指向你的目标构建路径，这样你每次重新构建Spring Boot，`spring`命令就会保持同步更新了。
你可以用下面的命令来查看：

```bash
$ sdk ls springboot
================================================================================
Available Springboot Versions
================================================================================
> + dev* 1.3.6.RELEASE
================================================================================
+ - local version
* - installed
> - currently in use
================================================================================
```

#### OSX Homebrew安装

如果你在使用Mac并且使用[Homebrew](http://brew.sh/)，安装Spring Boot CLI需要做的所有事情就是运行下面的命令：

```bash
$ brew tap pivotal/tap
$ brew install springboot
```

Homebrew会将`spring`安装到`/usr/local/bin`。

> 如果你找不到这个formula，可能你安装的brew过期了，运行`brew update`然后重试。

#### MacPorts installation

如果你使用Mac并且使用[MacPorts](http://www.macports.org/), 安装Spring Boot CLI需要做的所有事情就是运行下面的命令：

```bash
$ sudo port install spring-boot-cli
```

#### 命令行自动补全

Spring Boot CLI有一个配套的脚本为[BASH](http://en.wikipedia.org/wiki/Bash_%28Unix_shell%29)和[zsh](http://en.wikipedia.org/wiki/Zsh)提供了自动补全功能。 你可以在任意的shell中用`source`来执行这个脚本(同样名叫`spring`)，或者将其放在你个人或者系统级的bash自动补全初始化目录中。在Debian系统中，系统级的脚本位于`/shell-completion/bash`中，当启动一个新的shell时，目录中的脚本会全部执行。你可以手动运行这个脚本，例如你使用SDKMAN!安装的话，就使用以下的命令：

```bash
$ . ~/.sdkman/springboot/current/shell-completion/bash/spring
$ spring <HIT TAB HERE>
  grab help jar run test version
```

> 如果你使用Homebrew或MacPorts安装Spring Boot CLI，命令行自动补全脚本会自动注册到你的shell中。

#### 快速创建一个Spring CLI示例

你可以使用这里的一个非常简单的web应用来测试你的安装。创建一个名为app.groovy的文件：

```groovy
@RestController
class ThisWillActuallyRun { 

    @RequestMapping("/") 
    String home() { 
        "Hello World!" 
    }

}
```

然后在shell中运行：

```bash
$ spring run app.groovy
```

> 如果你第一次运行，会花费一些时间来下载依赖，后面再次运行的时候会快的多。

在你最喜欢的浏览器中打开[localhost:8080](http://localhost:8080/)然后你应该能看到下面的输出：

```
Hello World!
```

### 从早期的Spring Boot版本升级

如果你正在从早期的Spring Boot发行版升级，查看[project wiki](http://github.com/spring-projects/spring-boot/wiki)上的“release notes”。你会在每个发行版本的“new and noteworthy”特性列表旁边找到升级的指令。

要升级已经存在的CLI，请使用相应的包管理器命令(例如`brew upgrade`)，或者如果你的CLI是手动安装的，参考[标准指令](http://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#getting-started-manual-cli-installation)，记得更新你的PATH环境变量，清除老旧的引用地址。

## 开发你的第一个Spring Boot应用

现在我们来开发一个简单的“Hello World” Java web应用，并且用上一些Spring Boot的关键特性。我们会使用Maven来构建这个项目，大部分的IDE都支持Maven。

> 网址[spring.io](http://spring.io/)下包含了很多使用了Spring Boot的“Getting Started”指南，如果你在寻找特定问题的解决方案，你可以先去那里看看。你可以从这个网址[start.spring.io](https://start.spring.io/)进入来简化步骤，在依赖搜索中选择web starter，这将自动生成一个新的项目结构，这样你就可以[直接开始编码](http://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#getting-started-first-application-code)。 你还可以查看[详细文档](https://github.com/spring-io/initializr)。

在开始之前，打开一个终端并验证你的Java和Maven版本：

```bash
$ java -version
java version "1.7.0_51"
Java(TM) SE Runtime Environment (build 1.7.0_51-b13)
Java HotSpot(TM) 64-Bit Server VM (build 24.51-b03, mixed mode)
```
```bash
$ mvn -v
Apache Maven 3.2.3 (33f8c3e1027c3ddde99d3cdebad2656a31e8fdf4; 2014-08-11T13:58:10-07:00)
Maven home: /Users/user/tools/apache-maven-3.1.1
Java version: 1.7.0_51, vendor: Oracle Corporation
```

> 你需要为这个示例创建一个单独的目录，后续的所有指令都假设你创建了合适的目录，并且你的“当前目录”就在你的项目目录中。

### 创建POM

我们需要创建一个Maven `pom.xml`文件，这个`pom.xml`文件将作为你项目的构建配置。打开你最常用的文本编辑器然后添加如下内容：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd"> 
    <modelVersion>4.0.0</modelVersion> 

    <groupId>com.example</groupId> 
    <artifactId>myproject</artifactId>
    <version>0.0.1-SNAPSHOT</version>

    <parent> 
        <groupId>org.springframework.boot</groupId> 
        <artifactId>spring-boot-starter-parent</artifactId> 
        <version>1.3.6.RELEASE</version> 
    </parent> 

    <!-- Additional lines to be added here... -->

</project>
```

这样你就有了一个可以构建的项目，你可以运行`mvn package`来测试(你现在可以暂时忽略“jar will be empty - no content was marked for inclusion!”的警告信息)。

> 这时候你就可以把项目导入一个IDE了(大部分现代Java IDE都内置了Maven的支持)。但为了简单起见，我们接下来还是使用纯文本编辑器来操作这个示例。

### 添加classpath依赖

Spring Boot提供了大量的“Starter POMs”，让你可以很方便的添加需要的jar包到你的classpath。我们的示例程序已经在POM的parent中使用了`spring-boot-starter-parent`，这是一个特殊的starter，它提供了很多有用的默认Maven配置，以及依赖管理的支持，使得你在使用一些依赖的时候可以省略`version`版本号。


其他的“Starter POMs”可以让我们很方便的使用开发某种特定应用时需要的依赖。如果我们想开发一个web应用，我们就引入一个`spring-boot-starter-web`的依赖，在这之前我们先来看看我们现在已经引入的依赖：

```bash
$ mvn dependency:tree

[INFO] com.example:myproject:jar:0.0.1-SNAPSHOT
```
`mvn dependency:tree`命令可以打印出你的项目的依赖树，如你所见`spring-boot-starter-parent`自身并没有提供任何的依赖包，我们现在编辑我们的`pom.xml`，加进`spring-boot-starter-web`依赖：
```xml
<dependencies> 
    <dependency> 
        <groupId>org.springframework.boot</groupId> 
        <artifactId>spring-boot-starter-web</artifactId> 
    </dependency>
</dependencies>
```

如果你再次运行`mvn dependency:tree`命令，你会发现现在出现了一些新的依赖，包括Tomcat和Spring Boot。

### 敲代码

为了完成我们的应用，我们需要创建一个Java文件。Maven默认会从`src/main/java`中找到源代码并编译，所以你需要按照这个格式创建目录结构，然后添加一个叫做`src/main/java/Example.java`的文件：

```java
import org.springframework.boot.*;
import org.springframework.boot.autoconfigure.*;
import org.springframework.stereotype.*;
import org.springframework.web.bind.annotation.*;

@RestController
@EnableAutoConfiguration
public class Example {

    @RequestMapping("/")
    String home() { 
        return "Hello World!"; 
    } 

    public static void main(String[] args) throws Exception { 
        SpringApplication.run(Example.class, args); 
    }

}
```

这里没有多少代码，后面还会添加很多代码进来，但现在让我们从最重要的部分开始吧。

#### 注解@RestController和@RequestMapping

我们的`Example`类里面的第一个注解是`@RestController`，它是一个*构造型(stereotype)*注解，用来标记并且告诉Spring这个类扮演一个特殊的角色，在这个例子里面，我们的类是一个web应用的`@Controller`，当Spring要处理web请求的时候会找到这个类。

下面的`@RequestMapping`注解标注路由(routing)信息，用来告诉Spring任何路径为“/”的HTTP请求应该被映射到`home`方法上。因为类被标注了`@RestController`注解所以这里方法返回的字符串会被Spring直接原封不动的传递给请求者。

> 这里的`@RestController`和`@RequestMapping`注解是Spring MVC提供的注解(它们并不是由Spring Boot提供的特殊注解)，参考[MVC相关文档](http://docs.spring.io/spring/docs/4.2.7.RELEASE/spring-framework-reference/htmlsingle#mvc)。

#### 注解@EnableAutoConfiguration

上面的第二类级注解是@EnableAutoConfiguration，这个注解会让Spring Boot基于你已添加的jar依赖来“猜测”你想怎么样来配置Spring。如果你添加了`spring-boot-starter-web`因此引入了Tomcat和Spring MVC的依赖，自动配置系统会假设你要开发一个web应用并依此帮你配置好Spring。

> **Starter POMs和自动配置(Auto-Configuration)**

> 自动配置在设计上跟“Starter POMs”十分契合，但这两个概念并没有直接绑定关系，你可以自由的选择外部jar依赖，Spring Boot还是可以很好帮助你自动配置你的应用。

#### “main”方法

我们的应用最后一部分是`main`方法，这是一个遵循Java惯例的标准应用入口方法，我们的`main`方法通过执行`run`方法代理了Spring Boot的`SpringApplication`类。`SpringApplication`将会运行我们的应用，启动自动配置的Tomcat服务器。我们需要将`Example.class`作为参数传入`run`方法，以告诉Spring这是我们的首选组件，命令行参数也会通过`args`数组传入。

### 运行示例

到这里我们的应用应该可以顺利运行了，如果我们使用了`spring-boot-starter-parent`的POM，我们将会有一个非常有用的`run goal`，可以用来直接启动我们的应用，在项目根目录键入命令`mvn spring-boot:run`来启动应用：

```bash
$ mvn spring-boot:run

  .   ____          _            __ _ _
 /\\\\ / ___'_ __ _ _(_)_ __  __ _ \\ \\ \\ \\
( ( )\\___ | '_ | '_| | '_ \\/ _` | \\ \\ \\ \\
 \\\\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot :: (v1.3.6.RELEASE)
....... . . .
....... . . . (log output here)
....... . . .
........ Started Example in 2.222 seconds (JVM running for 6.514)
```

打开浏览器并访问[localhost:8080](http://localhost:8080/)你应该可以看到如下的输出：

```
Hello World!
```

如果想退出应用可以按`ctrl-c`。

### 创建一个可执行jar文件

让我们创建一个完全自包含的并且可以在生产环境运行的可执行jar文件，以此来结束我们的示例。可执行jar(有时也称为“fat jar”)是一个包含了运行应用所需的所有依赖的包文件。

> **可执行jar和Java**

> Java没有提供任何标准的机制来加载嵌套的jar文件(jar包中包含了其他jar文件)，如果你想要分发一个自包含的应用，这将成为一个阻碍。

> 为了解决这个问题，很多开发者使用“超级”jar，简单的从所有jar文件中取出所有类并一起打包在一个单独的jar文件中。这种做法存在的问题是你很难弄清楚你的应用实际上在使用的是哪个库，如果不同的jar中存在同名的文件(但内容不同)也将带来潜在的问题。

> Spring Boot采取了一个[不同的方式](http://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#executable-jar)，允许你真正的嵌套jar文件。

想要创建一个可执行jar文件，我们需要在`pom.xml`中添加`spring-boot-maven-plugin`，在`dependencies`段下面添加如下：

```xml
<build> 
    <plugins> 
        <plugin> 
            <groupId>org.springframework.boot</groupId> 
            <artifactId>spring-boot-maven-plugin</artifactId> 
        </plugin> 
    </plugins>
</build>
```

> `spring-boot-starter-parent`的POM包含`<executions>`配置，绑定了`repackage` goal，如果你没有使用parent POM，你需要自己声明这个配置，参考[插件文档](http://docs.spring.io/spring-boot/docs/1.3.6.RELEASE/maven-plugin/usage.html)。

保存你的`pom.xml`并且在命令行中运行`mvn package`：

```bash
$ mvn package

[INFO] Scanning for projects...
[INFO]
[INFO] ------------------------------------------------------------------------
[INFO] Building myproject 0.0.1-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] .... ..
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ myproject ---
[INFO] Building jar: /Users/developer/example/spring-boot-example/target/myproject-0.0.1-SNAPSHOT.jar
[INFO]
[INFO] --- spring-boot-maven-plugin:1.3.6.RELEASE:repackage (default) @ myproject ---
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
```

这时你在`target`目录中就能看到`myproject-0.0.1-SNAPSHOT.jar`，这个文件的大小应该在10Mb左右。如果你想查看文件内部的情况，可以使用`jar tvf`：

```bash
$ jar tvf target/myproject-0.0.1-SNAPSHOT.jar
```

在`target`目录中应该还可以看到一个小得多的文件，名为`myproject-0.0.1-SNAPSHOT.jar.original`，这是在Spring Boot执行repackag之前Maven创建的原始jar文件。

要运行应用，我们可以使用`java -jar`命令：

```bash
$ java -jar target/myproject-0.0.1-SNAPSHOT.jar

  .   ____          _            __ _ _
 /\\\\ / ___'_ __ _ _(_)_ __  __ _ \\ \\ \\ \\
( ( )\\___ | '_ | '_| | '_ \\/ _` | \\ \\ \\ \\
 \\\\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot :: (v1.3.6.RELEASE)
....... . . .
....... . . . (log output here)
....... . . .
........ Started Example in 2.536 seconds (JVM running for 2.864)
```

和之前一样，如果想退出应用，按`ctrl-c`。

## 接下来阅读什么内容

希望这个章节给你提供了一些Spring Boot的基础，并且能帮助你创建好自己的应用，打开新世界的大门。如果你是一个任务导向型的开发者，你可能想跳转到[spring.io](http://spring.io/)并且查看[getting started](http://spring.io/guides/)指南获取特定Spring功能使用问题的解决方案。我们也有Spring Boot的*[How-to](http://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#howto)*文档可以参考。

在[Spring Boot repository](http://github.com/spring-projects/spring-boot)中也有[一些样例](http://github.com/spring-projects/spring-boot/tree/v1.3.6.RELEASE/spring-boot-samples)可以运行。所有的样例都是和余下的代码相独立的，你不需要编译其他的代码。

另外，逻辑上接下来应该阅读的内容是*[“使用Spring Boot”](http://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#using-boot)*，但如果你实在是缺乏耐心，你也可以直接跳到*[Spring Boot features](http://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#boot-features)*。
.