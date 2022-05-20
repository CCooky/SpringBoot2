| 课程单元                                   | 学习目标                                                     |
| ------------------------------------------ | ------------------------------------------------------------ |
| <font color="#ff0000"><b>基础篇</b></font> | 能够创建SpringBoot工程<br/>基于SpringBoot实现ssm/ssmp整合    |
| <font color="#cc0000"><b>应用篇</b></font> | 能够掌握SpringBoot程序多环境开发<br/>能够基于Linux系统发布SpringBoot工程<br/>能够解决线上灵活配置SpringBoot工程的需求<br/>能够基于SpringBoot整合任意第三方技术 |
| <font color="#990000"><b>原理篇</b></font> | 掌握SpringBoot内部工作流程<br/>理解SpringBoot整合第三方技术的原理<br/>实现自定义开发整合第三方技术的组件 |
| **番外篇**                                 | 掌握SpringBoot整合非常见的第三方技术<br/>掌握相同领域的更多的解决方案，并提升同领域方案设计能力 |

# SpringBoot基础篇

在基础篇中，能够使用SpringBoot搭建基于SpringBoot的web项目开发，所以内容较少，主要包含如下内容：

- SpringBoot快速入门
- SpringBoot基础配置
- 基于SpringBoot整合SSMP

## SpringBoot快速入门

**均是先创建空的工程啊！下面创建的都是说的模块。**

### 1.springboot项目创建（idea联网版）

**步骤①**：创建新模块，选择Spring Initializr，并配置模块相关基础信息

<img src="images/image-20220325194120551.png" alt="image-20220325194120551" style="zoom: 67%;" />

**步骤②**：选择当前模块需要使用的技术集

<img src="images/image-20220325194226887.png" alt="image-20220325194226887" style="zoom:67%;" />

按照要求，左侧选择web，然后在中间选择Spring Web即可，选完右侧就出现了新的内容项，这就表示勾选成功了

<font color="#ff0000"><b>关注</b></font>：此处选择的SpringBoot的版本使用默认的就可以了，需要说一点，SpringBoot的版本升级速度很快，可能昨天创建工程的时候默认版本是2.5.4，今天再创建工程默认版本就变成2.5.5了，差别不大，无需过于纠结，回头可以到配置文件中修改对应的版本

生成的目录结构如下：（已经设置隐藏了一些文件）

<img src="images/image-20220325194819558.png" alt="image-20220325194819558" style="zoom:80%;" />

**步骤③**：开发控制器类。我们直接新建一个包，写controller层代码。

```java
//Rest模式
@RestController
@RequestMapping("/books")
public class BookController {
    @GetMapping
    public String getById(){
        System.out.println("springboot is running...");
        return "springboot is running...";
    }
}
```

入门案例制作的SpringMVC的控制器基于Rest风格开发，当然此处使用原始格式制作SpringMVC的程序也是没有问题的，上例中的**@RestController**与**@GetMapping注解**是基于Restful开发的典型注解

**步骤④**：运行自动生成的Application类

<img src="images/image-20220102205513372.png" alt="image-20220102205513372" style="zoom:80%;" />

```
访问路径：	http://localhost:8080/books
```

不难看出，运行的信息中包含了8080的端口，Tomcat这种熟悉的字样，难道这里启动了Tomcat服务器？是的，这里已经启动了。那服务器没有配置，哪里来的呢？后面再说。现在就可以通过浏览器访问请求的路径。66666

<img src="images/image-20220325195639084.png" alt="image-20220325195639084" style="zoom:80%;" />

是不是感觉很神奇？目前的效果其实依赖的底层逻辑还是很复杂的，但是从开发者角度来看，目前只有两个文件展现到了开发者面前

- pom.xml

  ​	这是maven的配置文件，描述了当前工程构建时相应的配置信息

  ```XML
  <?xml version="1.0" encoding="UTF-8"?>
  <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
      <modelVersion>4.0.0</modelVersion>
      <parent>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-parent</artifactId>
          <version>2.6.2</version>
          <relativePath/> <!-- lookup parent from repository -->
      </parent>
  
      <groupId>com.CCooky</groupId>
      <artifactId>demo1</artifactId>
      <version>0.0.1-SNAPSHOT</version>
      <name>demo1</name>
      <description>demo1</description>
  
      <properties>
          <java.version>1.8</java.version>
      </properties>
  
      <dependencies>
          <dependency>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-starter-web</artifactId>
          </dependency>
  
          <dependency>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-starter-test</artifactId>
              <scope>test</scope>
          </dependency>
      </dependencies>
  
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

  配置中有两个信息需要关注，一个是parent，也就是当前工程继承了另外一个工程，干什么用的后面再说，还有依赖坐标，干什么用的后面再说

- Application类

  ```JAVA
  @SpringBootApplication
  public class Application {
      public static void main(String[] args) {
          SpringApplication.run(Application.class, args);
      }
  }
  ```

  ​	这个类功能很简单，就一句代码，前面运行程序就是运行的这个类.

通过上面的制作，我们不难发现，SpringBoot程序简直太好写了，几乎什么都没写，功能就有了，这也是SpringBoot技术为什么现在这么火的原因，和Spirng程序相比，SpringBoot程序在开发的过程中各个层面均具有优势。

| **类配置文件**         | **Spring**   | **SpringBoot** |
| ---------------------- | ------------ | -------------- |
| pom文件中的坐标        | **手工添加** | **勾选添加**   |
| web3.0配置类           | **手工制作** | **无**         |
| Spring/SpringMVC配置类 | **手工制作** | **无**         |
| 控制器                 | **手工制作** | **手工制作**   |

一句话总结一下就是<font color="#ff0000"><b>能少写就少写</b></font>，<font color="#ff0000"><b>能不写就不写</b></font>，这就是SpringBoot技术给我们带来的好处。

### 2.springboot项目创建（官网版）

SpringBoot官网和Spring的官网是在一起的，都是  spring.io  。你可以通过项目一级一级的找到SpringBoot技术的介绍页，然后在页面中间部位找到如下内容

<img src="images/image-20220102205824712.png" alt="image-20220102205824712" style="zoom:67%;" />

**步骤①**：点击Spring Initializr后进入到创建SpringBoot程序的界面上，下面是输入信息的过程，和前面的一样，只是界面变了而已，根据自己的要求，在左侧选择对应信息和输入对应的信息即可

<img src="images/image-20220102205855293.png" alt="image-20220102205855293" style="zoom: 80%;" />

**步骤②**：右侧的ADD DEPENDENCIES用于选择使用何种技术，和之前勾选的Spring WEB是在做同一件事，仅仅是界面不同而已，点击后打开网页版的技术选择界面

<img src="images/image-20220102205921568.png" alt="image-20220102205921568" style="zoom:80%;" />

**步骤③**：所有信息设置完毕后，点击下面左侧按钮，生成一个文件包

**步骤④**：保存后得到一个压缩文件，这个文件打开后就是创建的SpringBoot工程文件夹了

**步骤⑤**：解压缩此文件后，得到工程目录，在Idea中导入即可使用，和之前创建的东西完全一样。下面就可以自己创建一个Controller测试一下是否能用了。

<font color="#f0f"><b>温馨提示</b></font>

​	做到这里其实可以透漏一个小秘密，Idea工具中创建SpringBoot工程其实连接的就是SpringBoot的官网，走的就是这个过程，只不过Idea把界面给整合了一下，读取到了Spring官网给的信息，然后展示到了Idea的界面中而已。

<img src="images/image-20220102210110630.png" alt="image-20220102210110630" style="zoom:80%;" />

### 3.springboot项目创建（阿里云版）

前面都是基于国外的官网，有的时候网速很慢，不太行。

创建工程时，切换选择starter服务路径，然后手工收入阿里云提供给我们的使用地址即可。地址：http://start.aliyun.com

<img src="images/image-20220102210325134.png" alt="image-20220102210325134" style="zoom:80%;" />

阿里为了便于自己开发使用，因此在依赖坐标中添加了一些阿里相关的技术，也是为了推广自己的技术吧，所以在依赖选择列表中，你有了更多的选择。不过有一点需要说清楚，阿里云地址默认创建的SpringBoot工程版本是<font color="#ff0000"><b>2.4.1</b></font>，所以如果你想更换其他的版本，创建项目后手工修改即可，别忘了刷新一下，加载新版本信息

<img src="images/image-20220102210348015.png" alt="image-20220102210348015" style="zoom:80%;" />

阿里云提供的地址更符合国内开发者的使用习惯，里面有一些SpringBoot官网上没有给出的坐标。

​	<font color="#ff0000"><b>注意</b></font>：阿里云提供的工程创建地址初始化完毕后和实用SpringBoot官网创建出来的工程略有区别。主要是在配置文件的形式上有区别。

### 4.自己创建SpringBoot

其实很简单的哦，创建一个Maven模块，然后导入springboot的依赖，按照他的格式一模一样写一个启动类，一个测试类。不久没了嘛。

第一步：创建一个java的maven模块

<img src="images/image-20220325205548147.png" alt="image-20220325205548147" style="zoom:80%;" />

第二步：导入springboot的依赖。

一个parent、一个dependency、一个build

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.6.2</version>
    <relativePath/> <!-- lookup parent from repository -->
</parent>

<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>

<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

第三步：创建启动类，测试类

```java
@SpringBootApplication
public class Demo1Application {

    public static void main(String[] args) {
        SpringApplication.run(Demo1Application.class, args);
    }

}
```

```java
@SpringBootTest
public class ApplicationTests {
    @Test
    public void test1(){
    }
}
```

第四步：创建controller层进行测试。

```java
// Rest风格
@RestController
@RequestMapping("/books")
public class BooksController {
    @GetMapping
    public String getById(){
        System.out.println("springboot is running.....");
        return "springboot is runing.....";
    }
}
```

<img src="images/image-20220325210602601.png" alt="image-20220325210602601" style="zoom:67%;" />

### SpringBoot搭建细节

SpringBoot是由Pivotal团队提供的全新框架，其设计目的是用来<font color="#ff0000"><b>简化Spring应用的初始搭建以及开发过程</b></font>。其中简化了了哪些东西呢？其实就是针对原始的Spring程序制作的两个方面进行了简化：

Spring程序缺点

- 依赖设置繁琐
  - 以前写Spring程序，使用的技术都要自己一个一个的写，现在不需要了，如果做过原始SpringMVC程序的小伙伴应该知道，写SpringMVC程序，最基础的spring-web和spring-webmvc这两个坐标时必须的，就这还不包含你用json啊等等这些坐标，现在呢？一个坐标搞定。除此之外，之前我们Spring程序最少有一个配置文件或配置类把，做web开发的话，必须要web的配置文件吧，现在都没得了
- 配置繁琐
  - 以前写配置类或者配置文件，然后用什么东西就要自己写加载bean这些东西，现在呢？什么都没写，照样能用。写bean真的是脑壳痛都。

这些简化操作在SpringBoot中有专业的用语，也是**SpringBoot程序的核心功能及优点：**

- 起步依赖（简化依赖配置）
  - 依赖配置的书写简化就是靠这个起步依赖达成的
- 自动配置（简化常用工程相关配置）
  - 配置过于繁琐，使用自动配置就可以做响应的简化，但是内部还是很复杂的，后面具体展开说
- 辅助功能（内置服务器，……）
  - 除了上面的功能，其实SpringBoot程序还有其他的一些优势，比如我们没有配置Tomcat服务器，但是能正常运行，这是SpringBoot程序的一个可以感知到的功能，也是SpringBoot的辅助功能之一。一个辅助功能都能做的这么6，太牛了

​	这些简化操作具体都在哪些方面进行体现的，一共分为4个方面：

- parent
- starter
- 引导类
- 内嵌tomcat

#### parent

==对版本号进行大一统，避免自己写冲突。==

举一个很简单的例子，其实就是Maven里面的依赖传递。核心是< parent>标签。

<img src="images/image-20220325215917573.png" alt="image-20220325215917573" style="zoom:80%;" />

在我们的父模块下，定义好需要的依赖，并且指定好坐标版本。我们新建的子模块只需要去继承他就行了。

（特别是坐标版本，这一点超级重要的，因为我们开发会用到超级多依赖，就会出现版本冲突，例如我Mybatis需要1.0版本日志，Junit需要1.3版本的日志，那怎么办呢？就需要自己调，以前这是要搞死人的啊）

在我们SpringBoot框架里面，这个父模块就是：

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.6.2</version>
    <relativePath/> <!-- lookup parent from repository -->
</parent>
```

点击进去查看这个pom文件，他又有一个parent，然后下面的东西几百行都不是我们想要的：

<img src="images/image-20220326093405830.png" alt="image-20220326093405830" style="zoom:80%;" />

点击进去查看`spring-boot-dependencies`这个pom文件，哇，终于找到了，核心地方就是这里：

<img src="images/image-20220326093543568.png" alt="image-20220326093543568" style="zoom:80%;" />

他在properties定义了统一的版本号，那为什么我们自己引入依赖的时候，没有用 ${ }去引入这个属性呢，往下翻你会看到他定义了< dependencyManagement>：

<img src="images/image-20220326093828916.png" alt="image-20220326093828916" style="zoom:80%;" />

接下来，我们看一下阿里云他是怎么做的！

打开pom文件，发现他并没有继承parent，而是写了这样一个东西：直接引入了我们的`spring-boot-dependencies`，哦，到这里大家就应该明白了，springboot的核心就是这个资源，这两种只是使用的方式不一样而已。

<img src="images/image-20220326094354403.png" alt="image-20220326094354403" style="zoom:80%;" />

**总结**

1. 开发SpringBoot程序要继承spring-boot-starter-parent
2. spring-boot-starter-parent中定义了若干个依赖管理
3. 继承parent模块可以避免多个依赖使用相同技术时出现依赖版本冲突
4. 继承parent的形式也可以采用引入依赖的形式实现效果

#### starter

parent中定义了若干个依赖版本管理，但是也没有使用，那这个设定也就不生效啊，究竟谁在使用这些定义呢？就是这个starter。

SpringBoot关注到开发者在实际开发时，对于依赖坐标的使用往往都有一些固定的组合方式，比如使用spring-webmvc就一定要使用spring-web。每次都要固定搭配着写，非常繁琐，而且格式固定，没有任何技术含量。

​	SpringBoot一看这种情况，看来需要给开发者带来一些帮助了。安排，把所有的技术使用的固定搭配格式都给开发出来，以后你用某个技术，就不用一次写一堆依赖了，还容易写错，我给你做一个东西，代表一堆东西，开发者使用的时候，直接用我做好的这个东西就好了，对于这样的固定技术搭配，SpringBoot给它起了个名字叫做<font color="#ff0000"><b>starter</b></font>。

打开pom文件，springboot会自动给我们两个依赖，一个是web的，一个是web测试的starter。

<img src="images/image-20220326151933496.png" alt="image-20220326151933496" style="zoom:80%;" />

点进去看一下，哦呼，可以发现我们所有需要的资源都在这里，有SpringMVC的也有Spring集成Web的，往上面还有几个starter，同理可知，这几个starter里面肯定也引入了一大堆他所需要的依赖，所以说，我们自己不用导入想使用的资源了，早就已经有了。

<img src="images/image-20220326152156289.png" alt="image-20220326152156289" style="zoom: 80%;" />

**starter与parent的区别**

​	朦朦胧胧中感觉starter与parent好像都是帮助我们简化配置的，但是功能又不一样，梳理一下。

​	<font color="#ff0000"><b>starter</b></font>是一个坐标中定了若干个坐标，以前写多个的，现在写一个，<font color="#ff0000"><b>是用来减少依赖配置的书写量的</b></font>

​	<font color="#ff0000"><b>parent</b></font>是定义了几百个依赖版本号，以前写依赖需要自己手工控制版本，现在由SpringBoot统一管理，这样就不存在版本冲突了，<font color="#ff0000"><b>是用来减少依赖冲突的</b></font>。

**实际开发应用方式**

- 实际开发中如果需要用什么技术，先去找有没有这个技术**对应的starter**

  - 如果有对应的starter，直接写starter，而且无需指定版本，版本由parent提供
  - 如果没有对应的starter，手写坐标即可
- 实际开发中如果发现坐标出现了冲突现象，确认你要使用的可行的版本号，使用手工书写的方式添加对应依赖，覆盖SpringBoot提供给我们的配置管理


<font color="#f0f"><b>温馨提示</b></font>

​	SpringBoot官方给出了好多个starter的定义，方便我们使用，而且名称都是如下格式

```JAVA
命名规则：spring-boot-starter-技术名称
```

​	所以以后见了spring-boot-starter-aaa这样的名字，这就是SpringBoot官方给出的starter定义。那非官方定义的也有吗？有的，具体命名方式到整合章节再说

#### 引导类

​	配置说完了，我们发现SpringBoot确实帮助我们减少了很多配置工作，那程序是如何运行的。目前程序运行的入口就是SpringBoot工程创建时自带的带有main方法那个类了。

<img src="images/image-20220326153128696.png" alt="image-20220326153128696" style="zoom:80%;" />

我们Crtl+Alt+V看一下这个run方法的运行结果是什么？

<img src="images/image-20220326200018698.png" alt="image-20220326200018698" style="zoom: 80%;" />

哇哇！！这不是应用上下文对象嘛，不就是Spring的那个最终的东西嘛，并且我们还可以从里面拿到Bean。所以说到这里就明白了。这个方法就是用来产生Spring容器对象的。说到底SpringBoot就是为了简化加速Spring开发的，具体实现还是Spring那一套。

哦豁，还有一个问题，我们给自己写的类加上了@Controller注解，那springboot是怎么知道的呢？据推理肯定他自己配置了一个包扫描器，对滴，其实就是这样，并且他的包扫描器是扫描当前包及其子包下所有内容。

然后如何让SpringBoot知道这是我们的引导类呢，就是方法上面的那个注解`@SpringBootApplication`，里面的实现我们也可以点进去看一下，里面有很多注解，等到后面原理篇再详细讲解。

#### 内嵌tomcat

程序现在已经运行了，通过引导类的main方法运行了起来。但是运行java程序不应该是执行完就结束了吗？但是我们现在明显是启动了一个web服务器啊，不然网页怎么能正常访问呢？这个服务器是在哪里写的呢？

我们去Pom文件里面找一下：

<img src="images/image-20220326201659667.png" alt="image-20220326201659667" style="zoom:80%;" />

原来是tomcat这个starter，在web-starter里面引入了这个服务器。那问题来了，之前我们不是把java程序扔到web服务器里面跑嘛，怎么现在给java程序内嵌了一个服务器呢？？？

**内嵌Tomcat运行原理**

​	Tomcat服务器是一款软件，而且是一款使用java语言开发的软件，熟悉的小伙伴可能有印象，tomcat安装目录中保存有jar，好多个jar。

​	下面的问题来了，既然是使用java语言开发的，运行的时候肯定符合java程序运行的原理，java程序运行靠的是什么？对象呀，一切皆对象，万物皆对象。那tomcat运行起来呢？也是对象。

​	如果是对象，那Spring容器是用来管理对象的，这个对象能不能交给Spring容器管理呢？哦呼肯定啊，是个对象都可以交给Spring容器管理，行了，这下通了。tomcat服务器运行其实是以对象的形式在Spring容器中运行的，怪不得我们没有安装这个tomcat，而且还能用。闹了白天这东西最后是以一个对象的形式存在，保存在Spring容器中悄悄运行的。具体运行的是什么呢？其实就是上前面提到的那个tomcat内嵌核心。

**更换内嵌Tomcat**

那既然是个对象，如果把这个对象从Spring容器中去掉是不是就没有web服务器的功能呢？是这样的，通过依赖排除可以去掉这个web服务器功能，

<img src="images/image-20220326202130193.png" alt="image-20220326202130193" style="zoom:80%;" />

然后我们可以更改自己想要使用的服务器，SpringBoot提供了3款内置的服务器，

- tomcat(默认)：apache出品，粉丝多，应用面广，负载了若干较重的组件

- jetty：更轻量级，负载性能远不及tomcat

- undertow：负载性能勉强跑赢tomcat

  想用哪个，加个坐标就OK。前提是把tomcat排除掉，因为tomcat是默认加载的。

  <img src="images/image-20220326202309339.png" alt="image-20220326202309339" style="zoom:80%;" />

### 教你一招：在Idea中隐藏指定文件/文件夹

创建SpringBoot工程时，使用SpringBoot向导也好，阿里云也罢，其实都是为了一个目的，得到一个标准的SpringBoot工程文件结构。这个时候就有新的问题出现了，标准的工程结构中包含了一些未知的文件夹，在开发的时候看起来特别别扭，这一节就来说说这些文件怎么处理。

​	处理方案无外乎两种，如果你对每一个文件/目录足够了解，没有用的完全可以删除掉，或者不删除，但是看着别扭，就设置文件为看不到就行了。删除不说了，直接Delete掉就好了，这一节说说如何隐藏指定的文件或文件夹信息。

​	既然是在Idea下做隐藏功能，肯定隶属于Idea的设置，设置方式如下。

**步骤①**：打开设置，【Files】→【Settings】

<img src="images/image-20220102210654947.png" alt="image-20220102210654947" style="zoom:80%;" />

**步骤②**：打开文件类型设置界面，【Editor】→【File Types】→【Ignored Files and Folders】，忽略文件或文件夹显示

<img src="images/image-20220102210706755.png" alt="image-20220102210706755" style="zoom:80%;" />

**步骤③**：添加你要隐藏的文件名称或文件夹名称，可以使用*号通配符，表示任意，设置完毕即可。



### 补充：REST风格

- <font color="#ff0000"><b>REST</b></font>：（Representational State Transfer），表现形式状态转换。一种资源表现风格

<img src="images/image-20220102212202122.png" alt="image-20220102212202122" style="zoom:80%;" />

优点：

- 隐藏资源的访问行为，无法通过地址得知对资源是何种操作。
- 书写简化

**这样就有一个问题**：我怎么知道它具体是做的什么操作呢，我查询指定用户用的是上面那个user/1的路径，那我删除呢，修改呢，于是这个公司建立了下面这种，加了一个行为动作。

<img src="images/image-20220102212358137.png" alt="image-20220102212358137" style="zoom:80%;" />

<font color="#ff0000"><b>RESTful</b></font>：后来我们统一把上面几个根据REST风格对资源进行访问的方式叫做RESTful

**Restful**风格的请求是使用**“url+请求方式”**表示一次请求目的的，HTTP 协议里面四个请求表示操作方式的动词如下：

-  GET：用于获取资源
-  POST：用于新建资源  (邮寄)
-  PUT：用于更新资源 （放置，摆放）
-  DELETE：用于删除资源

<font color="#ff0000"><b>注意事项：</b></font>

​		上述的行为是约定方式，约定不是规范，是可以被打破的，不一定要按照这样写，所以说叫做REST风格，不是REST规范。但随着人数越来越多，逐渐成为了一种专业的约定。

==注意到，RestFul风格的特点，这里不再使用？问号来传递我们的请求参数，而是把参数写在了访问路径url里面==

那我们具体如何取获取里请求参数呢？在Restful风格里面，获取请求参数有三种方式如下：，一般而言我们只用到两个。

- 获取url路径地址参数 如**localhost:80/msgs/13973034756**————@PathVariable；

  ```java
      @GetMapping("/{tele}")
      public String get(@PathVariable("tele") String tele){
          String code = msgService.get(tele);
          return code;
      }
  ```

- 获取请求体参数，只能使用实体类进行接收,如**localhost:80/msgs**————@RequestBody；（只有当使用Ajax提交，接收为json格式时用）

  ```java
  public class TeleCode {
  		private String tele;
  		private String code;
  		getter/setter…
  }
  @PostMapping
  public void check(@RequestBody TeleCode teleCode) throws IOException {
        
  }
  ```

  <img src="images/image-20220505185222864.png" alt="image-20220505185222864" style="zoom: 80%;" />

- 一般的MVC普通参数，如**localhost:80/msgs?phone=139&mycode=123456**————@RequestParam；

  ```java
      @PostMapping
      public boolean check(@RequestParam("phone") String tele, @RequestParam("mycode") String code){
  				......
      }
  ```

<img src="images/image-20220327102112041.png" alt="image-20220327102112041"  />

上述url地址	`/user/1`	中的1就是要获得的请求参数，在SpringMVC中可以使用**占位符**进行参数绑定。地址	`/user/1`	可以写成	`/user/{id}`	，占位符{id}对应的就是1的值。在业务方法中我们可以使用**@PathVariable注解**进行占位符的匹配获取工作。

```java
// 对应下面五个的请求行
http://localhost:8080/SpringMVC_war/users（post）
http://localhost:8080/SpringMVC_war/users（put）
http://localhost:8080/SpringMVC_war/users/5（delete）
http://localhost:8080/SpringMVC_war/users/3（get）
http://localhost:8080/SpringMVC_war/users（get）
```

```java
// Rest风格
@Controller
public class BooksController {

    @RequestMapping(value = "/books", method = RequestMethod.POST)
    @ResponseBody
    public String save(@RequestBody Book book){
        System.out.println("Book save ....."+book);
        return "{'module':'Books save'}";
    }

    @RequestMapping(value = "/books",method = RequestMethod.PUT)
    @ResponseBody
    public String update(@RequestBody Book book){
        System.out.println(book);
        return "{'module':'Books update'}";
    }

    @RequestMapping(value = "/books/{id}",method = RequestMethod.DELETE)
    @ResponseBody
    public String delete(@PathVariable(value="id") Integer id){
        System.out.println("Book delete..." + id);
        return "{'module':'Books delete'}";
    }

    @RequestMapping(value = "/books/{id}",method = RequestMethod.GET)
    @ResponseBody
    public String getById(@PathVariable(value="id") Integer id){
        System.out.println("Book getById..." + id);
        return "{'module':'Books getById'}";
    }

    @RequestMapping(value = "/books",method = RequestMethod.GET)
    @ResponseBody
    public String getAll(){
        System.out.println("Book getAll...");
        return "{'module':'Books getAll'}";
    }
}
```

从这里，可以看出：我前端访问同一个url，但请求方式不同，就会访问到不同的资源！！！就是以后url不用再写的那么复杂，不然每一个方法都要有一个单独命名的url才行。然后我们采用ApiPost进行测试，需要请求体的记得写，json格式串；



#### **Restful风格开发简化**

看到上面，其实是有很多重复的代码的，例如每个方法都写了@ResponseBody注解，每个方法url都有重复的books，

<img src="images/image-20220327105603136.png" alt="image-20220327105603136" style="zoom:80%;" />



**第一步简化**：我们把这些重复的代码，可以直接放到类上进行注解撒。但是注意哦，有路径参数的，还是要写单独写在方法上面。

<img src="images/image-20220327110212550.png" alt="image-20220327110212550" style="zoom:80%;" />



**第二步简化：**SpringBoot提供了一个单独给Rest风格的注解，==`@RestController`==，他等于`@Controller+@ResponseBody`。

<img src="images/image-20220327110343535.png" alt="image-20220327110343535" style="zoom:80%;" />



**第三步简化：**每个方法上都要加重复的代码`method = RequestMethod.。。`这个也还行哦，但也可以简化呢，springboot提供了每个请求方法的一个注解，例如 ==`@PostMapping`==,，如果有路径参数的话，直接在这个注解后面加上就好了，真棒。

```java
@PostMapping = @RequestMapping(method = RequestMethod.POST)

@DeleteMapping("/{id}") =
         @RequestMapping(value = "/{id}",method = RequestMethod.DELETE)
```

<img src="images/image-20220327110920861.png" alt="image-20220327110920861" style="zoom:80%;" />

**最终效果：**

<img src="images/image-20220505180752307.png" alt="image-20220505180752307" style="zoom:80%;" />

<img src="images/image-20220327111539700.png" alt="image-20220327111539700" style="zoom:80%;" />

## SpringBoot基础配置

入门案例做完了，下面就要研究SpringBoot的用法了。通过入门案例，各位小伙伴能够感知到一个信息，SpringBoot没有具体的功能，它在辅助加快Spring程序的开发效率。我们发现现在几乎不用做任何的配置，功能就有了，确实很好用。但是仔细想想，没有做配置意味着什么？意味着配置已经做好了，不用你自己写了。但是新的问题又来了，如果不想用已经写好的默认配置，该如何干预呢？这就是这一章咱们要研究的问题。

SpringBoot呈现给开发者的只有三个文件，一个pom.xml，一个引导类，一个application.properties。那不用说了，我们自定义的配置就是在这里弄了。

### 1.属性配置

application.properties就是springboot的配置文件，所有相关的配置都是在这里修改！

properties格式的文件书写规范是**key=value，**

- **修改服务器端口**

  默认tomcat的是8080，我们改成80。直接打开配置文件，写入：

  ```properties
  server.port=80
  ```

- **关闭,修改运行日志图表（banner)**

  ```properties
  spring.main.banner-mode=off
  spring.banner.image.location=text.png
  ```

- **设置运行日志的显示级别**

  ```properties
  logging.level.root=debug
  ```


你会发现，现在这么搞配置太爽了，以前你做配置怎么做？不同的技术有自己专用的配置文件，文件不同格式也不统一，现在呢？不用东奔西走的找配置文件写配置了，统一格式了，这就是大秦帝国啊，统一六国。SpringBoot比大秦狠，因为未来出现的技术还没出现呢，但是现在已经确认了，配置都写这个文件里面。

**第一个问题**：这个配置的规则是什么，我开发者可以配置哪些东西，在那里可以看所有能配置的项呢？

- 打开SpringBoot的官网，找到SpringBoot官方文档，打开查看附录中的Application Properties就可以获取到对应的配置项了，

**第二个问题，**这个配置项和什么有关。在pom中注释掉导入的**spring-boot-starter-web**，然后刷新工程，你会发现配置的提示消失了。其实是设定使用了什么技术才能做什么配置。也合理，不然配置的东西都没有使用对应技术，配了也是白配。

<font color="#f0f"><b>温馨提示</b></font>

​	所有的starter中都会依赖下面这个starter，叫做**spring-boot-starter**。这个starter是所有的SpringBoot的starter的基础依赖，里面定义了SpringBoot相关的基础配置，关于这个starter我们到开发应用篇和原理篇中再深入讲解。

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter</artifactId>
    <version>2.5.4</version>
    <scope>compile</scope>
</dependency>
```

例如，点击进去我们的spring-boot-starter-web里面就依赖这个资源！！！

<img src="images/image-20220326203546232.png" alt="image-20220326203546232" style="zoom:80%;" />

### 2.配置文件分类

SpringBoot除了支持properties格式的配置文件，还支持另外两种格式的配置文件.其实这三种都可以啊，但yml的格式比较符合我们的审美，哈哈哈。而且他分那种前后级，我们可以更好的找到所需要的。

- properties格式

  ```properties
  server.port=80
  spring.banner.image.location=text.png
  logging.level.root=debug
  ```

- **yml格式（常用）**

  ```yml
  server:
    port: 81
  spring:
    banner:
      image:
        location: "3-4.jpg"
  logging: 
    level: 
      root: debug
  ```

- yaml格式

  ```yaml
  server:
    port: 82
  ```

yml和yaml文件格式就是一模一样的，只是文件后缀不同，所以可以合并成一种格式来看.

**三种配置文件的优先级：**

1. 配置文件间的加载优先级	properties（最高）>  yml  >  yaml（最低）
2. 不同配置文件中相同配置按照加载优先级相互覆盖，不同配置文件中不同配置全部保留

### 教你一招：**自动提示功能消失解决方案**

有的时候，我们新建或者删除了配置文件之后，发现配置的代码自动提示消失了。

大体原因有如下2种：

1. Idea认为你现在写配置的文件不是个配置文件，所以拒绝给你提供提示功能
2. Idea认定你是合理的配置文件，但是Idea加载不到对应的提示信息

这里我们主要解决第一个现象，第二种现象到原理篇再讲解。第一种现象的解决方式如下：

**步骤①**：打开设置，【Files】→【Project Structure...】

<img src="images/image-20220102215017215.png" alt="image-20220102215017215" style="zoom:80%;" />

**步骤②**：在弹出窗口中左侧选择【Facets】，右侧选中Spring路径下对应的模块名称，也就是你自动提示功能消失的那个模块

<img src="images/image-20220102215031651.png" alt="image-20220102215031651" style="zoom:80%;" />

<img src="images/image-20220102215056457.png" alt="image-20220102215056457" style="zoom:80%;" />

**步骤③**：点击Customize Spring Boot按钮，此时可以看到当前模块对应的配置文件是哪些了。如果没有你想要称为配置文件的文件格式，就有可能无法弹出提示

<img src="images/image-20220102215120617.png" alt="image-20220102215120617" style="zoom:80%;" />

<img src="images/image-20220102215131052.png" alt="image-20220102215131052" style="zoom:80%;" />

**步骤④**：选择添加配置文件，然后选中要作为配置文件的具体文件就OK了

<img src="images/image-20220102215143545.png" alt="image-20220102215143545" style="zoom:80%;" />

<img src="images/image-20220102215150406.png" alt="image-20220102215150406" style="zoom:80%;" />

### 3.yml文件格式

yml具有严格的语法格式要求，具体如下：

1. 大小写敏感
2. 属性层级关系使用多行描述，每行结尾使用冒号结束
3. 使用缩进表示层级关系，同层级左侧对齐，只允许使用空格（不允许使用Tab键）
4. 数据属性值前面添加空格（很重要哦）

5. "  #  "号表示注释
6. 使用**引号包裹的字符串**，里面的转义字符可以生效，其他情况不生效

下面列出常见的数据书写格式，熟悉一下.

```yml
boolean: TRUE  						#TRUE,true,True,FALSE,false，False均可
float: 3.14    						#6.8523015e+5  #支持科学计数法
int: 123       						#0b1010_0111_0100_1010_1110    #支持二进制、八进制、十六进制
null: ~        						#使用~表示null
string: HelloWorld      			#字符串可以直接书写
string2: "Hello World"  			#可以使用双引号包裹特殊字符
date: 2018-02-17        			#日期必须使用yyyy-MM-dd格式
datetime: 2018-02-17T15:02:31+08:00  #时间和日期之间使用T连接，最后使用+代表时区
```

​	此外，yaml格式中也可以表示**数组**，在属性名书写位置的下方**使用减号**作为数据开始符号.

```yml
# 键值对，字符串可以加引号也可以不加
country: china
age: 8080

# 对象
user:
  name:heima
  age:12
user2:
  name:heima
  age:12

# 数组1
likes:
  - game
  - music
  - sleep
  - pp
# 数组2
likes2: [game,music,sleeo,pp]

# 对象数组1
users:
  -
    name: zhangsan
    age: 18
  -
    name: lisi
    age: 20
# 对象数组2（json格式）
users2: [{name: zhangsan,age: 18},{name: lisi, age: 10}]
```

### 4.yml数据读取

对于yaml文件中的数据，其实你就可以想象成这就是一个**小型的数据库**，里面保存有若干数据，每个数据都有一个独立的名字.

#### 读取单一数据

yaml中保存的单个数据，可以使用Spring中的注解直接读取，使用**@Value**（Spring的注解）可以读取单个数据，属性名引用方式：<font color="#ff0000"><b>${一级属性名.二级属性名……}</b></font>。从这里也可以知道，SpringBoot会自动读取我们yml配置文件里面的数据，并且放到Spring容器里面。

![image-20220327164311185](images/image-20220327164311185.png)

<img src="images/image-20220327163744960.png" alt="image-20220327163744960"  />

记得使用**@Value注解**时，要将该注入写在某一个指定的Spring管控的bean的属性名上方。

#### yml文件中的变量引用

同样可以直接在yml里面使用${ }引用。

再强调一点，yml里面字符串有两种写法，一是不加双引号，而是加双引号，区别在于使用引号包裹的字符串，里面的转义字符可以生效

```yml
baseDir: c:\win10
temDir: ${baseDir}\temp		#==>c:\win10\temp
temDir2: "${baseDir}\temp"	#==>c:\win10  emp
lesson: "Spring\tboot\nlession"		#==>Spring	boot
																	#==>lesson
```



#### 读取全部数据

SpringBoot提供了一个对象，能够把所有的配置数据都封装到这一个对象中，这个对象叫做**Environment**，使用**自动装配注解@Autowired**可以将所有的yaml数据封装到这个对象中

<img src="images/image-20220102221830626.png" alt="image-20220102221830626"  />

数据封装到了Environment对象中，获取属性时，通过Environment的接口操作进行，具体方法是**getProperties（String）**，参数填写属性名即可

#### 读取对象数据

单一数据读取书写比较繁琐，全数据封装又封装的太厉害了，每次拿数据还要一个一个的getProperties（）,总之用起来都不是很舒服。由于Java是一个面向对象的语言，很多情况下，我们会将一组数据封装成一个对象。SpringBoot也提供了可以将一组yaml对象数据封装一个Java对象的操作。

1，定义一个类，这个类是用来装yml的数据的，并将该对象放到Spring容器里面**@Component**。自定义对象属性名要与yml对象属性名一一对应

2，使用注解**@ConfigurationProperties**指定该对象加载yaml中哪一组对象配置的信息。

3，使用的时候从Spring容器里面直接拿取。**@Autowired**

<img src="images/image-20220102223529481.png" alt="image-20220102223529481"  />

这个@ConfigurationProperties必须告诉他加载的**数据前缀**是什么，这样当前前缀下的所有属性就封装到这个对象中。记得**自定义对象属性名要与yml对象属性名一一对应**啊，不然没法封装。

以后如果你要定义一组数据自己使用，就可以先写一个对象，然后定义好属性，然后到配置中根据这个格式书写即可。

<img src="images/image-20220102224730794.png" alt="image-20220102224730794" style="zoom:80%;" />

## SpringBoot实现SSMP整合

- 整合JUnit
- 整合MyBatis
- 整合MyBatis-Plus
- 整合Druid

### 1. 整合Junit

1，导入测试需要的starter；

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
```

2，测试类使用@SpringBootTest注解；

3，内部的测试方法就和我们之前的Spring一模一样；

```java
@SpringBootTest
class Demo1ApplicationTests {
    //注入你要测试的对象
    @Autowired
    private BookDao bookDao;
    @Test
    void contextLoads() {
        //执行要测试的对象对应的方法
        bookDao.save();
        System.out.println("two...");
    }
}
```

4，测试类如果存在于引导类所在包或子包中无需指定引导类

5，测试类如果不存在于引导类所在的包或子包中需要通过classes属性指定引导类

**原理解释：**

我们先来看一下不使用SpringBoot技术时，Spring整合JUnit的制作方式

```JAVA
//加载spring整合junit专用的类运行器
@RunWith(SpringJUnit4ClassRunner.class)
//指定对应的配置信息
@ContextConfiguration(classes = SpringConfig.class)
public class AccountServiceTestCase {
    //注入你要测试的对象
    @Autowired
    private AccountService accountService;
    @Test
    public void testGetById(){
        //执行要测试的对象对应的方法
        System.out.println(accountService.findById(2));
    }
}

```

​	其中核心代码是前两个注解，第一个注解@RunWith是设置Spring专用于测试的类运行器，简单说就是Spring程序执行程序有自己的一套独立的运行程序的方式，不能使用JUnit提供的类运行方式了，必须指定一下，但是格式是固定的，琢磨一下，<font color="#ff0000"><b>每次都指定一样的东西，这个东西写起来没有技术含量啊</b></font>，第二个注解@ContextConfiguration是用来设置Spring核心配置文件或配置类的，简单说就是加载Spring的环境你要告诉Spring具体的环境配置是在哪里写的，虽然每次加载的文件都有可能不同，但是仔细想想，如果文件名是固定的，这个貌似也是一个固定格式。似然<font color="#ff0000"><b>有可能是固定格式，那就有可能每次都写一样的东西，也是一个没有技术含量的内容书写</b></font>

然后他使用一个注解@SpringBootTest替换了前面两个注解。至于内部是怎么回事？和之前一样，只不过都走默认值。默认我们加载的配置类或配置文件就是之前启动程序的引导类，如果想手工指定引导类，可以直接在注解`@SpringBootTest`中添加classes属性指定配置类。

```java
@SpringBootTest(classes = Springboot04JunitApplication.class)
class Springboot04JunitApplicationTests {
    //注入你要测试的对象
    @Autowired
    private BookDao bookDao;
    @Test
    void contextLoads() {
        //执行要测试的对象对应的方法
        bookDao.save();
        System.out.println("two...");
    }
}
```



### 2. 整合MyBatis

下面看看SpringBoot整合MyBaits格式

**步骤①**：创建模块时勾选要使用的技术，MyBatis，由于要操作数据库，还要勾选对应数据库

![image-20211129092156020](images/image-20211129092156020.png)

![image-20211129092210993](images/image-20211129092210993.png)

​	或者手工导入对应技术的starter，和对应数据库的坐标

```XML
<dependencies>
    <!--1.导入对应的starter-->
    <dependency>
        <groupId>org.mybatis.spring.boot</groupId>
        <artifactId>mybatis-spring-boot-starter</artifactId>
        <version>2.2.0</version>
    </dependency>

    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <scope>runtime</scope>
    </dependency>
</dependencies>
```

**步骤②**：配置数据源相关信息，没有这个信息你连接哪个数据库都不知道

```yaml
#2.配置相关信息
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/mp?useSSL=false
    username: root
    password: 5240zhouquan
```

​	完了，就这么多，没了。有人就很纳闷，这就结束了？对，这就结束了，SpringBoot把配置中所有可能出现的通用配置都简化了。

下面写一下MyBatis程序运行需要的Mapper就可以运行。要注意的一点是，把我们的映射文件放到容器内的**注解@Mapper**

**第一步：**实体类

<img src="images/image-20220327210453926.png" alt="image-20220327210453926" style="zoom:80%;" />

**第二步：**Mapper接口和xml文件。（还是和原来一样的）注意 **@Mapper**注解，作用是：让数据库SQL映射被容器识别到。

<img src="images/image-20220327210553124.png" alt="image-20220327210553124" style="zoom:80%;" />

<img src="images/image-20220327210609008.png" alt="image-20220327210609008" style="zoom:80%;" />

**第三步：**测试

```java
@SpringBootTest
class DemoMybatisApplicationTests {

    @Autowired
    private UserMapper userMapper;

    @Test
    void testGetById() {
        User user = userMapper.getById(3);
        System.out.println(user);
    }
}
```

<font color="#ff0000"><b>注意</b></font>：当前使用的SpringBoot版本是2.5.4，对应的坐标设置中Mysql驱动使用的是8x版本。当SpringBoot2.4.3（不含）版本之前会出现一个小BUG，就是MySQL驱动升级到8以后要求强制配置时区，如果不设置会出问题。解决方案很简单，驱动url上面添加上对应设置就行了

```YAML
#2.配置相关信息
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC
    username: root
    password: root
```

​	这里设置的UTC是全球标准时间，你也可以理解为是英国时间，中国处在东八区，需要在这个基础上加上8小时，这样才能和中国地区的时间对应的，也可以修改配置不写UTC，写Asia/Shanghai也可以解决这个问题。

可以最后去打开SSM部分的笔记，对比一下Spring整合Mybatis是有多么麻烦！！！

### 3. 整合MyBatis-Plus

与Mybatis不同的点：

- 导入坐标不同；
- 数据层简化；

**步骤①**：导入对应的starter

```XML
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.4.3</version>
</dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
```

​	在springBoot创建的技术选择里面没有这个要我们自己加。

**步骤②**：配置数据源相关信息

```yaml
#2.配置相关信息     
server:
  port: 80
  
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/ssm_db
    username: root
    password: root

mybatis-plus:
  global-config:
    db-config:
      id-type: auto		#设置主键id字段的生成策略为数据库自增
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl  #开启运行日志，选择控制台输出
```

​	没了，就这么多，剩下的就是写MyBaitsPlus的程序了。

第一步：实体类

<img src="images/image-20220327213249862.png" alt="image-20220327213249862" style="zoom:80%;" />

第二步：Mapper接口和映射文件xml。注意加@Mapper，不然SpringBoot怎么知道呢、

<img src="images/image-20220327213353778.png" alt="image-20220327213353778" style="zoom:80%;" />

<img src="images/image-20220327213410986.png" alt="image-20220327213410986" style="zoom:80%;" />

第三步：随便测试一个BaseMapper里面的功能。

```java
@SpringBootTest
class DemoMybatisplusApplicationTests {

    @Autowired
    private UserMapper userMapper;

    @Test
    void testSelectList() {
        List<User> list = userMapper.selectList(null);
        System.out.println(list);
    }
}
```

### 4. 整合Druid

很简单了洛，两步就搞定了。先是导入对应的starter，然后在yml配置文件里面进行该技术的配置。

在没有指定数据源时，我们的配置如下：

```YAML
#2.配置相关信息
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=Asia/Shanghai
    username: root
    password: root
```

​	此时虽然没有指定数据源，但是根据SpringBoot的德行，肯定帮我们选了一个它认为最好的数据源对象，这就是HiKari。通过启动日志可以查看到对应的身影。

```tex
2021-11-29 09:39:15.202  INFO 12260 --- [           main] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Starting...
2021-11-29 09:39:15.208  WARN 12260 --- [           main] com.zaxxer.hikari.util.DriverDataSource  : Registered driver with driverClassName=com.mysql.jdbc.Driver was not found, trying direct instantiation.
2021-11-29 09:39:15.551  INFO 12260 --- [           main] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Start completed.
```

​	上述信息中每一行都有HiKari的身影。

**步骤①**：导入对应的starter

```XML
<dependencies>
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid-spring-boot-starter</artifactId>
        <version>1.2.6</version>
    </dependency>
</dependencies>
```

**步骤②**：修改配置

```YAML
spring:
  datasource:
    druid:
      driver-class-name: com.mysql.cj.jdbc.Driver
      url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC
      username: root
      password: root
```

除了这4个常规配置外，还有druid专用的其他配置。通过提示功能可以打开druid相关的配置查阅：

<img src="images/image-20220328100616343.png" alt="image-20220328100616343" style="zoom:80%;" />



### 5. SSMP综合案例

SpringBoot能够整合的技术太多太多了，对于初学者来说慢慢来，一点点掌握。下面我们做一个案例。

首先确定我们的技术选型：

1. 实体类开发————Lombok
2. Dao（Mapper）层开发————MybatisPlus
3. Service层开发————MybatisPlus
4. Controller层开发————Restful风格，接口测试使用ApiPost
5. Controller开发————前后端开发协议制作
6. 页面开发————基于VUE+ElementUI制作，前后端联调，页面数据处理，页面消息处理
   - 列表
   - 新增
   - 修改
   - 删除
   - 分页
   - 查询
7. 项目异常处理

#### 0.模块创建

对于这个案例如果按照企业开发的形式进行应该制作后台微服务，前后端分离的开发。

<img src="images/image-20211129114306062.png" alt="image-20211129114306062" style="zoom:80%;" />

我知道这个对初学的小伙伴要求太高了，咱们简化一下。后台做单体服务器，前端不使用前后端分离的制作了。

<img src="images/image-20211129114328967.png" alt="image-20211129114328967" style="zoom:80%;" />

一个服务器即充当后台服务调用，又负责前端页面展示，降低学习的门槛。

1. 勾选SpringMVC与MySQL坐标，其他的需要用的可以后面再加
2. 修改配置文件为yml
3. 端口修改为80，方便访问

#### 1.实体类开发

数据库表结构很简单，就是单一表如下：

```sql
-- ----------------------------
-- Table structure for tbl_book
-- ----------------------------
DROP TABLE IF EXISTS `tbl_book`;
CREATE TABLE `tbl_book`  (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `type` varchar(20) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  `name` varchar(50) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  `description` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = InnoDB AUTO_INCREMENT = 51 CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;

-- ----------------------------
-- Records of tbl_book
-- ----------------------------
INSERT INTO `tbl_book` VALUES (1, '计算机理论', 'Spring实战 第5版', 'Spring入门经典教程，深入理解Spring原理技术内幕');
INSERT INTO `tbl_book` VALUES (2, '计算机理论', 'Spring 5核心原理与30个类手写实战', '十年沉淀之作，手写Spring精华思想');
INSERT INTO `tbl_book` VALUES (3, '计算机理论', 'Spring 5 设计模式', '深入Spring源码剖析Spring源码中蕴含的10大设计模式');
INSERT INTO `tbl_book` VALUES (4, '计算机理论', 'Spring MVC+MyBatis开发从入门到项目实战', '全方位解析面向Web应用的轻量级框架，带你成为Spring MVC开发高手');
INSERT INTO `tbl_book` VALUES (5, '计算机理论', '轻量级Java Web企业应用实战', '源码级剖析Spring框架，适合已掌握Java基础的读者');
INSERT INTO `tbl_book` VALUES (6, '计算机理论', 'Java核心技术 卷I 基础知识（原书第11版）', 'Core Java 第11版，Jolt大奖获奖作品，针对Java SE9、10、11全面更新');
INSERT INTO `tbl_book` VALUES (7, '计算机理论', '深入理解Java虚拟机', '5个维度全面剖析JVM，大厂面试知识点全覆盖');
INSERT INTO `tbl_book` VALUES (8, '计算机理论', 'Java编程思想（第4版）', 'Java学习必读经典,殿堂级著作！赢得了全球程序员的广泛赞誉');
INSERT INTO `tbl_book` VALUES (9, '计算机理论', '零基础学Java（全彩版）', '零基础自学编程的入门图书，由浅入深，详解Java语言的编程思想和核心技术');
INSERT INTO `tbl_book` VALUES (10, '市场营销', '直播就该这么做：主播高效沟通实战指南', '李子柒、李佳琦、薇娅成长为网红的秘密都在书中');
INSERT INTO `tbl_book` VALUES (11, '市场营销', '直播销讲实战一本通', '和秋叶一起学系列网络营销书籍');
INSERT INTO `tbl_book` VALUES (12, '市场营销', '直播带货：淘宝、天猫直播从新手到高手', '一本教你如何玩转直播的书，10堂课轻松实现带货月入3W+');
```

<img src="images/image-20220328161218448.png" alt="image-20220328161218448" style="zoom:80%;" />



#### 2.Dao层开发

##### --基础CRUD

技术选型：MybatisPlus+Druid

**步骤①**：导入MyBatisPlus与Druid对应的starter

```xml
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.4.3</version>
</dependency>
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid-spring-boot-starter</artifactId>
    <version>1.2.6</version>
</dependency>
```

**步骤②**：配置数据库连接相关的数据源配置。

<font color="#f0f"><b>温馨提示</b></font>

​	MP技术默认的主键生成策略为雪花算法，生成的主键ID长度较大，和目前的数据库设定规则不相符，需要配置一下使MP使用数据库的主键生成策略，方式嘛还是老一套，做配置。

```yml
server:
  port: 80

spring:
  datasource:
    druid:
      driver-class-name: com.mysql.cj.jdbc.Driver
      url: jdbc:mysql://localhost:3306/studysql?useSSL=false&serverTimezone=UTC
      username: root
      password: 5240zhouquan
      
mybatis-plus:
  global-config:
    db-config:
      id-type: auto		#设置主键id字段的生成策略为数据库自增
```

**步骤③**：使用MP的标准通用接口BaseMapper加速开发，别忘了==@Mapper和泛型的指定==

```java
@Mapper
public interface BookMapper extends BaseMapper<Book> {
}
```

MP默认进行了ResultMap的转换哦，"小驼峰——user_name"

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.CCooky.mapper.BookMapper">


</mapper>
```

**步骤④**：制作测试类测试结果，这个测试类制作是个好习惯，不过在企业开发中往往都为加速开发跳过此步，且行且珍惜吧。

```java
@SpringBootTest
class SsmpApplicationTests {

    @Autowired
    private BookMapper bookMapper;

    @Test
    public void testSave(){
        Book book = new Book();
        book.setType("测试数据123");
        book.setName("测试数据123");
        book.setDescription("测试数据123");
        int result = bookMapper.insert(book);
    }

    @Test
    public void testGetById(){
        Book book = bookMapper.selectById(1);
        System.out.println(book);
    }

    @Test
    public void testGetAll(){
        List<Book> books = bookMapper.selectList(null);
        System.out.println(books);
    }

    @Test
    public void testUpdateById(){
        Book book = new Book();
        book.setId(14);
        book.setType("测试数据abcdefg");
        book.setName("测试数据123");
        book.setDescription("测试数据123");
        int result = bookMapper.updateById(book);
    }

    @Test
    public void testDeleteById(){
        bookMapper.deleteById(1);
    }
}
```

**查看MP运行日志**

每次运行我们还要打印一下查看结果，多捞 哦，日志技术给你干嘛呢！

yml文件开启一下MP的运行日志：

```yml
server:
  port: 80

spring:
  datasource:
    druid:
      driver-class-name: com.mysql.cj.jdbc.Driver
      url: jdbc:mysql://localhost:3306/studysql?useSSL=false&serverTimezone=UTC
      username: root
      password: 5240zhouquan

mybatis-plus:
  global-config:
    db-config:
      id-type: auto
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
```

此处设置的是日志的显示形式，当前配置的是控制台输出，当然还可以由更多的选择，根据需求切换即可

<img src="images/image-20220328164732364.png" alt="image-20220328164732364" style="zoom:80%;" />

##### --分页查询

这里慢慢讲一下实现过程：

首先，我们直接使用他的分页查询方法，其中第一个参数是Ipage对象，用来装我们的分页查询条件的（current&size），第二个是条件查询，用来生成where语句，他的名称叫做queryWrapper – 实体对象封装操作类。

<img src="images/image-20220328172138337.png" alt="image-20220328172138337" style="zoom:80%;" />

```java
@Test
public void testGetByPage(){
    IPage page = new Page(1,5);
    bookMapper.selectPage(page,null);
}
```

运行结果：

<img src="images/image-20220328170257017.png" alt="image-20220328170257017" style="zoom:80%;" />

哟，怎么回事呢？其实MybatisPlus做分页查询，也就是在我们正常的Select语句后面加上`Limit ?, ?`，对吧，看似很简单但Mybatis没有默认给我们开启这个功能。因为第一点，你不一定会用到这个往SQL语句后面添加的操作，第二点还有很多其他的可以SQL语句添加的功能，所以让开发者自己来使用。

实现过程，是通过MybatisPlus的拦截器实现的（可以理解成先是生成正常的语句，然后他给拦截下来强制加上一些东西再给数据库）。这个就不能通过yml配置文件了，需要我们新建一个配置类，然后把拦截器类给到Spring容器，后面MP他就会自己调用。（需要熟悉一些Spring注解写配置文件）

<img src="images/image-20220328170942215.png" alt="image-20220328170942215" style="zoom:80%;" />

```java
@Configuration
public class MPConfig {

    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor(){
        MybatisPlusInterceptor mybatisPlusInterceptor = new MybatisPlusInterceptor();
      // 这里可以添加多个拦截器，需要什么加什么
        mybatisPlusInterceptor.addInnerInterceptor(new PaginationInnerInterceptor());
        return mybatisPlusInterceptor;
    }
}
```

**那我们加了@Configuration，SpringBoot的主配置类会识别到吗？会的，之前讲过这个引导类会自动识别当前包及其子包下所有的注解。**

然后我们再直接测试前面的代码：GOOD

<img src="images/image-20220328171209997.png" alt="image-20220328171209997" style="zoom:80%;" />



**第二步：拿到我们的分页查询到的数据。**

<img src="images/image-20220328171628652.png" alt="image-20220328171628652" style="zoom:80%;" />

方法执行结果返回的还是一个Ipage对象，其实最后的结果全部封装给了我们传入的这个Ipage对象！！！真的不错，太方便了。我们通过Getter方法拿到里面的值。

```java
@Test
public void testGetByPage(){
    IPage page = new Page(1,5);
    bookMapper.selectPage(page,null);
    System.out.println(page.getCurrent()); //当前页码值
    System.out.println(page.getSize());    //每页显示数
    System.out.println(page.getTotal());   //数据总量
    System.out.println(page.getPages());   //总页数
    System.out.println(page.getRecords()); //详细数据
}
```

##### --条件查询

这里的核心对象就是之前我们遇见的**Wrapper**，在很多方法里面都有例如SelectList、selectPage、update、delete（可以在BaseMapper里面看到），我们点进去看一下这个**Wrapper类**，Crtl+H查看一下他的父类和实现类：

<img src="images/image-20220328174338007.png" alt="image-20220328174338007" style="zoom:80%;" />

标红的两个实现类就是我们用的最多最多的，**一个QueryWrapper、一个LambdaQueryWrapper**。他的专业名称叫做 实体对象封装操作类，通过调用里面的方法组后在SQL语句中生成Where条件！

我们来看一下这两个实现类的使用方式：	高级程序员都使用第二种嘿嘿

==**QueryWrapper**==

Wrapper里面有很多方法，like就是用来模糊查询的等等......

```java
@Test
public void testGetBy(){
    String bookName = "Spring";
    QueryWrapper<Book> qw = new QueryWrapper();
    if (Strings.isNotEmpty(bookName)){
        qw.like("name",bookName);// 第一个参数是column——表的列名
 
    }
    bookMapper.selectList(qw);
}
```

运行结果：

<img src="images/image-20220328175309781.png" alt="image-20220328175309781" style="zoom:80%;" />

<font color="#f0f"><b>温馨提示</b></font>

​		假如不加 非null验证，最后加载where的条件会变成  `%null%` ，那就GG洛

==**LambdaQueryWrapper**==

大司马见了，直呼内行内行！！！！Lambda表达式写法，就不可能出现写错`column`的情况

```java
@Test
public void testGetBy2(){
    String bookName = "String";
    LambdaQueryWrapper<Book> lqw = new LambdaQueryWrapper<>();
  // 第一个是codition——判断语句，是true的话，这行代码就会真正执行，否则失效
  // 第二个还是column——表的列名，只不过是通过我们的实体类来确定的！！细细细
    lqw.like(Strings.isNotEmpty(bookName), Book::getName, bookName);
  
    bookMapper.selectList(lqw);
}
```

#### 3.Service层开发

数据层开发告一段落，下面进行业务层开发，其实标准业务层开发很多初学者认为就是调用数据层，怎么说呢？这个理解是没有大问题的，更精准的说法应该是<font color="#ff0000"><b>组织业务逻辑功能，并根据业务需求，对数据持久层发起调用</b></font>。有什么差别呢？目标是为了组织出符合需求的业务逻辑功能，至于调不调用数据层还真不好说，有需求就调用，没有需求就不调用。

个常识性的知识普及一下，业务层的方法名定义一定要与业务有关，例如登录操作

```JAVA
login(String username,String password);
```

​	而数据层的方法名定义一定与业务无关，是一定，不是可能，也不是有可能，例如根据用户名密码查询

```JAVA
selectByUserNameAndPassword(String username,String password);
```

​	我们在开发的时候是可以根据完成的工作不同划分成不同职能的开发团队的。比如一个哥们制作数据层，他就可以不知道业务是什么样子，拿到的需求文档要求可能是这样的

```tex
接口：传入用户名与密码字段，查询出对应结果，结果是单条数据
接口：传入ID字段，查询出对应结果，结果是单条数据
接口：传入离职字段，查询出对应结果，结果是多条数据
```

​	但是进行业务功能开发的哥们，拿到的需求文档要求差别就很大

```tex
接口：传入用户名与密码字段，对用户名字段做长度校验，4-15位，对密码字段做长度校验，8到24位，对喵喵喵字段做特殊字符校验，不允许存在空格，查询结果为对象。如果为null，返回BusinessException，封装消息码INFO_LOGON_USERNAME_PASSWORD_ERROR
```

​	你比较一下，能是一回事吗？差别太大了，所以说业务层方法定义与数据层方法定义差异化很大，只不过有些入门级的开发者手懒或者没有使用过公司相关的ISO标准化文档而已。

**下面是Service的开发，很简单哦。**

注意几点：

1. 业务层都是逻辑规则，所以我们用Boolean类型；
2. 参数类型Integer的默认值为null，而int默认值为0。注意使用区分，因为0在我们Dao层给到数据库是有具有意义的！！！

<img src="images/image-20220328202822505.png" alt="image-20220328202822505" style="zoom:80%;" />

<img src="images/image-20220328202854457.png" alt="image-20220328202854457" style="zoom:80%;" />

##### --快速开发

MP设计了与我们前面Dao层一样类似父类，用于让我们继承，直接获取很多超级好用的业务方法。但是个人观点不推荐使用，凑合能用吧，其实就是一个封装+继承的思想，代码给出，实际开发慎用！！！

==他的底层也是在调用我们之前的数据层接口，所以说数据层的BaseMapper是必须的！！！==

接口快速开发

```JAVA
public interface IBookService extends IService<Book> {

    // 如果已经提供的不够再在下面写自己特殊的业务需求
    
}
```

接口实现类快速开发；继承的类需要传入两个泛型，一个是数据层接口，另一个是实体类

```JAVA
@Service
public class IBookServiceImpl extends ServiceImpl<BookMapper, Book> implements IBookService{
		
}
```

​	如果感觉MP提供的功能不足以支撑你的使用需要，其实是一定不能支撑的，因为需求不可能是通用的，在原始接口基础上接着定义新的API接口就行了，此处不再说太多了，就是自定义自己的操作了，但是不要和已有的API接口名冲突即可。

#### 4.controller层

##### --读取请求数据

这个就和之前的SpringMVC的获取请求数据的方式一模一样，只是说我们采用了RestFul风格的写法。

##### --回写数据

回写数据的话，当我们返回的是 对象/集合 时，springboot内部使用了json转换工具，会自动帮我们把对象和集合转换成json串，假如是String、Boolean等类型，就依然是原数据类型。

表现层终于到了，哇。这里技术选型：基于Restful风格，ApiPost工具测试接口.

相对来说，表现层的东西比较简单了，主要是几个注解`@RestController = @ReposeBody + @Controller`、`@RequestMapping("/books")`、 `@GetMapping....`、`@RequestBody`、`@PathVariable`

其中注意：分页查询的返回结果直接给Ipage对象就行了。反正最后都会转成json。

<img src="images/image-20220330165326863.png" alt="image-20220330165326863" style="zoom:80%;" />

全部通过测试。其中单独看一下分页查询返回的这个Page对象，转换成JSON后有什么数据，

<img src="images/image-20220330165439870.png" alt="image-20220330165439870" style="zoom: 67%;" />

##### --消息一致性处理

目前我们通过Postman测试后业务层接口功能时通的，但是这样的结果给到前端开发者会出现一个小问题。不同的操作结果所展示的数据格式差异化严重!

<img src="images/image-20220330165648554.png" alt="image-20220330165648554" style="zoom:80%;" />

​	所以说必须将所有操作的操作结果数据格式统一起来，需要设计表现层返回结果的模型类，用于后端与前端进行数据格式统一，也称为**前后端数据协议**。

<img src="images/image-20220330170044941.png" alt="image-20220330170044941" style="zoom:80%;" />

其中flag表示该操作是否成功；data用来装查询到的数据。对于增删改这种无数据的，就给null就行。

**具体实现：**

第一步：新建类，这是给表现层用的吧，我们就写在表现层里面。

<img src="images/image-20220330170420152.png" alt="image-20220330170420152" style="zoom:80%;" />

<img src="images/image-20220330171822017.png" alt="image-20220330171822017" style="zoom:80%;" />

第二步：修改表现层的返回结果，全部设为R类型，通过构造方法注入值。（用Setter方法，显得代码量多，很冗余）。值得注意的是：为什么查询的操作都是设为true，因为既然已经走到业务处理层了，那就证明服务器在处理请求的过程中没有出错，所以设置为true。

<img src="images/image-20220330172205151.png" alt="image-20220330172205151" style="zoom:80%;" />

后面前端拿到的分页查询数据json格式如下：这一看，多爽哦，舒舒服服。

<img src="images/image-20220330172658826.png" alt="image-20220330172658826" style="zoom:67%;" />

到这里，后端的工作已经全部搞完了！

#### 5.前后端连通性测试

这里我们就没有进行前后端分离开发，后面真正开发应该是前端的东西放在Node.js上面，后端在我们的Tomcat。这里是单体服务器。

- 前后端分离结构设计中，页面归属前端服务器；
- 单体工程中页面放到resources目录下的static目录中。（若出现问题建议先执行clean）

<img src="images/image-20220330200152181.png" alt="image-20220330200152181" style="zoom:80%;" />

我们先打开books.html简单看一下他的页面结构，启动服务器，访问url：http://localhost/pages/books.html，看一下他的页面效果。

前后端连通性测试，就是简单测试一下我们的getAll( )结构，看能否获取到所有数据。

<img src="images/image-20220330200609120.png" alt="image-20220330200609120" style="zoom:80%;" />

<img src="images/image-20220330200747857.png" alt="image-20220330200747857" style="zoom:67%;" />

#### 6.页面基础功能开发

##### --列表功能

这里是非分页版本。

其实很简单，VUE的数据模型双向绑定，通过查看，我们发现这个Table里面绑定的数据模型是`dataList`，然后我们直接在getAll方法里面赋值就完成了。用组件就是爽！

<img src="images/image-20220330202347753.png" alt="image-20220330202347753" style="zoom:80%;" />

<img src="images/image-20220330202458461.png" alt="image-20220330202458461" style="zoom: 67%;" />

##### --新增功能

先找到新增按钮和对应的绑定方法，然后找到新增弹框标签，再到方法里面进行属性设置。

<img src="images/image-20220330203151770.png" alt="image-20220330203151770" style="zoom:80%;" />

然后在方法里面设置，为true就可以看见新增的弹框了。哎呀后面所有的前端操作都差不多，找到方法，找到数据模型，然后点击后触发什么事件......后面就不这样一个个截图说明了，很麻烦。

​	**默认状态**

```vue
data:{
	dialogFormVisible: false,//添加表单是否可见
	...
},
```

​		**切换为显示状态**

```JS
//弹出添加窗口
handleCreate() {
	this.dialogFormVisible = true;
},
```

​		由于每次添加数据都是使用同一个弹窗录入数据，所以每次操作的痕迹将在下一次操作时展示出来，需要在每次操作之前清理掉上次操作的痕迹

​		**定义清理数据操作**

```js
//重置表单
resetForm() {
    this.formData = {};
},
```

​		**切换弹窗状态时清理数据**

```js
//弹出添加窗口
handleCreate() {
    this.dialogFormVisible = true;
    this.resetForm();
},
```

​		至此准备工作完成，下面就要调用后台完成添加操作了。

​		**添加操作**

```js
//添加
handleAdd () {
    //发送异步请求
    axios.post("/books",this.formData).then((res)=>{
        //如果操作成功，关闭弹层，显示数据
        if(res.data.flag){
            this.dialogFormVisible = false;
            this.$message.success("添加成功");
        }else {
            this.$message.error("添加失败");
        }
    }).finally(()=>{
        this.getAll();
    });
},
```

​	**取消添加操作**

```JS
//取消
cancel(){
    this.dialogFormVisible = false;
    this.$message.info("操作取消");
},
```

##### --删除功能

**删除操作**

```JS
// 删除
handleDelete(row) {
    axios.delete("/books/"+row.id).then((res)=>{
        if(res.data.flag){
            this.$message.success("删除成功");
        }else{
            this.$message.error("删除失败");
        }
    }).finally(()=>{
        this.getAll();
    });
},
```

​		**删除操作提示信息**

```JS
// 删除
handleDelete(row) {
    //1.弹出提示框
    this.$confirm("此操作永久删除当前数据，是否继续？","提示",{
        type:'info'
    }).then(()=>{
        //2.做删除业务
        axios.delete("/books/"+row.id).then((res)=>{
       		if(res.data.flag){
            	this.$message.success("删除成功");
        	}else{
            	this.$message.error("删除失败");
        	}
        }).finally(()=>{
            this.getAll();
        });
    }).catch(()=>{
        //3.取消删除
        this.$message.info("取消删除操作");
    });
}，	
```

##### --修改功能

1. **查询并展示数据**

```JS
//弹出编辑窗口
handleUpdate(row) {
    axios.get("/books/"+row.id).then((res)=>{
        if(res.data.flag){
            //展示弹层，加载数据
            this.formData = res.data.data;
            this.dialogFormVisible4Edit = true;
        }else{
            this.$message.error("数据同步失败，自动刷新");
        }
    });
},
```

​		**修改操作**

```JS
//修改
handleEdit() {
    axios.put("/books",this.formData).then((res)=>{
        //如果操作成功，关闭弹层并刷新页面
        if(res.data.flag){
            this.dialogFormVisible4Edit = false;
            this.$message.success("修改成功");
        }else {
            this.$message.error("修改失败，请重试");
        }
    }).finally(()=>{
        this.getAll();
    });
},
```

#### 7.异常消息处理

就拿我们之前的保存业务方法为例，我们手动在里面添加一个异常如下：

<img src="images/image-20220331171722893.png" alt="image-20220331171722893" style="zoom:67%;" />

，然后用ApiPost测试一下，返回的结果是什么。哦呼这是什么玩意呢，

<img src="images/image-20220331171915043.png" alt="image-20220331171915043" style="zoom:80%;" />

我们可不能吧这种消息发送给前端，因为前端根本看不懂啊。<img src="images/image-20220331172020953.png" alt="image-20220331172020953" style="zoom:80%;" />

所以说我们还要对错误的操作数据格式做同样的格式处理！！！！必须将这些消息处理成之前发送的格式。

**处理方法：**采用SpringMVC的异常处理器，拦截所有的异常，统一进行处理。并且之前讲过，我们所有的异常统一抛出到controller层，然后再抛出，这样异常处理器就会拿到这个异常进行处理。

**第一步：在当前的数据结果中添加消息字段，用来包装后台出现的异常信息；**

```java
@Data
public class R {
    private Boolean flag;
    private Object data;
    private String msg;		//用于封装异常消息

//    这个两个构造方法很好哦，很巧妙哦，免得写很多重复的东西。
//    因为flag是必须传递值的，而data不一定
    public R (Boolean flag){
        this.flag = flag;
    }

    public R (Boolean flag, Object data){
        this.flag = flag;
        this.data = data;
    }
    
//    异常情况下的返回值
    public R (String msg){
        this.flag = false;
        this.msg = msg;
    }
}
```

**第二步：编写SpringMVC异常处理器。就写在controller层的utils里面。**

<img src="images/image-20220331173551909.png" alt="image-20220331173551909" style="zoom:80%;" />

```java
// 作为springMVC的异常处理器
// @ControllerAdvice
@RestControllerAdvice
public class ProjectExceptionAdvice {

    //  拦截所有的异常信息进行下面处理. e就是拦截到的异常对象
//  @ExceptionHandler(IOException.class)  //可以单独争对不同异常进行个性化处理
    @ExceptionHandler
    public R doException(Exception e) {
        // 记录日志
        // 通知运维
        // 通知开发
        e.printStackTrace();//控制台打印信息
        return new R("服务器故障,请稍后再试");
    }
}
```

异常代码如下：（同前面截图）

```java
@PostMapping
public R save(@RequestBody Book book) throws IOException {
    if (true){
        throw new IOException();
    }
    Boolean result = bookService.save(book);
    return new R(result);
}
```

这样，我们返回给前端的消息数据就变成了如下格式：

<img src="images/image-20220331174157240.png" alt="image-20220331174157240" style="zoom:80%;" />

**第三步：前端处理**

即，如果后台发送的flag为false的话，就说明服务器故障，我们的操作没有成功撒，所以我们吧所有判断语句那里改一下，

```js
//添加
handleAdd () {
	//发送ajax请求
    axios.post("/books",this.formData).then((res)=>{
        //如果操作成功，关闭弹层，显示数据
        if(res.data.flag){
            this.dialogFormVisible = false;
            this.$message.success("添加成功");
        }else {
            this.$message.error(res.data.msg);//消息来自于后台传递过来，而非固定内容
        }
    }).finally(()=>{
        this.getAll();
    });
},
```

**第四步：**这个操作成功时，`this.$message.success("添加成功");`，这个添加成功是前端自己写的，但它也是属于消息处理的范畴，所以说应该后台人员写好，前端直接拿到数据，就和异常消息那里一样。

很简单的拉

```java
@PostMapping
public R save(@RequestBody Book book) throws IOException {
    Boolean result = bookService.save(book);
    return new R(result, result?"操作成功":"服务器故障,请稍后再试");
}
```

```js
//添加
handleAdd () {
	//发送ajax请求
    axios.post("/books",this.formData).then((res)=>{
        //如果操作成功，关闭弹层，显示数据
        if(res.data.flag){
            this.dialogFormVisible = false;
            this.$message.success(res.data.msg);
        }else {
            this.$message.error(res.data.msg);//消息来自于后台传递过来，而非固定内容
        }
    }).finally(()=>{
        this.getAll();
    });
},
```

#### 8.页面高级功能开发

##### --分页功能

分页功能是用来替代之前的查询全部功能的，所以说我们直接把前面的 “ 查询全部 ”注释掉，直接覆盖掉：

<img src="images/image-20220331183446611.png" alt="image-20220331183446611" style="zoom: 80%;" />

然后，再看看elementUI提供的分页组件模型：

<img src="images/image-20220331183615677.png" alt="image-20220331183615677" style="zoom:80%;" />

对应了三个分页的数据模型：

```js
data:{
	pagination: {	
		//分页相关模型数据
		currentPage: 1,	//当前页码
		pageSize:10,	//每页显示的记录数
		total:0,		//总记录数
	}
.....
```

好了，然后我们写函数：

```js
//分页查询
getAll() {
    axios.get("/books/"+this.pagination.currentPage+"/"+this.pagination.pageSize).then((res) => {
        this.pagination.currentPage = res.data.data.current;
        this.pagination.pagesize = res.data.data.size;
        this.pagination.total = res.data.data.total;
        this.dataList = res.data.data.records;
    })
},
//切换页码
handleCurrentChange(currentPage) {
    this.pagination.currentPage = currentPage;
    this.getAll();
},
```

##### --删除功能维护

由于使用了分页功能，当最后一页只有一条数据时，删除操作就会出现BUG，最后一页无数据但是独立展示，对分页查询功能进行后台功能维护，如果当前页码值大于最大页码值，重新执行查询。其实这个问题解决方案很多，这里给出比较简单的一种处理方案。

```java
    @GetMapping("/{current}/{size}")
    public R getPage(@PathVariable int current, @PathVariable int size){
        LambdaQueryWrapper<Book> lqw = new LambdaQueryWrapper();
        IPage<Book> page = bookService.getByPage(current, size,lqw);
//        如果当前前端传递的页码值大于总页码值
        if (current> page.getPages()){
            page = bookService.getByPage((int) page.getPages(), size,lqw);
        }
        return new R(true,page);
    }
```

这是一个很常见的问题；但这个办法不能解决所有的问题哦，假如数据量特别大的话，那有可能在我们进行查询的过程中，数据又被删除了呢，对不对。所以说这个根本来说，是根据我们的业务需求来维护的！！

##### --分页条件查询功能

这个要在之前的分页功能上面进行增强，就是前端要多传递几个参数，然后后端分页那里要加上Wrapper对象的条件。

**页面封装查询条件字段**

```js
pagination: {		
//分页相关模型数据
	currentPage: 1,		//当前页码
	pageSize:10,		//每页显示的记录数
	total:0,			//总记录数
	name: "",
	type: "",
	description: ""
},
```

页面添加查询条件字段对应的数据模型绑定名称

```HTML
<div class="filter-container">
    <el-input placeholder="图书类别" v-model="pagination.type" class="filter-item"/>
    <el-input placeholder="图书名称" v-model="pagination.name" class="filter-item"/>
    <el-input placeholder="图书描述" v-model="pagination.description" class="filter-item"/>
    <el-button @click="getAll()" class="dalfBut">查询</el-button>
    <el-button type="primary" class="butT" @click="handleCreate()">新建</el-button>
</div>
```

将查询条件组织成url参数，添加到请求url地址中，这里可以借助其他类库快速开发，当前使用手工形式拼接，降低学习要求。为什么不能用post，因为我们是Restful风格撒，查询只能用get请求。

![image-20220331205844928](images/image-20220331205844928.png)

然后修改后台controller层代码，也很简单。

<img src="images/image-20220331205600695.png" alt="image-20220331205600695"  />

# SpringBoot实用篇

实用篇是在基础篇的根基之上，补全SpringBoot的知识图谱。比如在基础篇中只给大家讲了yaml的语法格式，但是具体写yaml文件的时候还有很多实用开发过程中的坑，这些在实用篇中都要进行学习。

实用篇共分为两块内容，分别是运维实用篇和开发实用篇。在运维实用篇中，我给学习者的定位是玩转配置，为开发实用篇中做各种技术的整合做好准备工作。与开发实用篇相比，运维实用篇的内容显得略微单薄，并且有部分知识模块在运维实用篇和开发实用篇中都要讲一部分，这些内容都后置到开发实用篇中了。

# SpringBoot运维实用篇

包含以下内容：

- SpringBoot程序的打包与运行
- 配置高级
- 多环境开发
- 日志

## SpringBoot程序的打包与运行

首先考虑的一个问题：我们目前的所有程序都是在Idea下写的，运行也是在Idea下运行的，对吧，浏览器直接访问IDEA上运行的服务器。

<img src="images/image-20220401101401764.png" alt="image-20220401101401764" style="zoom:80%;" />

但是实际开发完成后，我们的项目是不可能运行在自己的电脑上的。因为如果你电脑关机了那不是就没有人可以访问资源了嘛。我们以后制作的程序是运行在专用的服务器上的，简单说就是将你做的程序放在一台独立运行的电脑上，这台电脑要比你开发使用的计算机更专业，并且安全等级各个方面要远超过你现在的电脑。

![image-20220401102144672](images/image-20220401102144672.png)

那问题来了，如何将我们的程序放到这个高级电脑上面运行呢？首先要将我们的程序先组织成一个文件，然后将这个文件传输到这台服务器上。这里面就存在两个过程，一个是**打包的过程**，另一个**是运行的过程**。

<font color="#f0f"><b>温馨提示</b></font>

​		企业项目上线为了保障环境适配性会采用下面流程发布项目，这里不讨论此过程。

1. 开发部门使用Git、SVN等版本控制工具上传工程到版本服务器
2. 服务器使用版本控制工具下载工程
3. 服务器上使用Maven工具在当前真机环境下重新构建项目
4. 启动服务

最后，我们Springboot的打包与运行分为两种，一个是windows下，一个是Linux下：

- 程序打包与运行（Windows版）
-  程序运行（Linux版）

### 1.程序打包与运行(Windows版)

首先打开我们前面基础篇的ssmp整合工程，以这个为例。

==①：对SpringBoot项目打包（执行Maven构建指令package）==

```java
mvn package
```

<img src="images/image-20220401102507534.png" alt="image-20220401102507534" style="zoom:80%;" />

然后它就会开始执行：观察他的执行过程，

<img src="images/image-20220401103004925.png" alt="image-20220401103004925" style="zoom:80%;" />

欸，它怎么执行了这些sql语句呢？？？其实是因为maven的生命周期，你看上面package撒，打包前有test，也就是执行了maven的test流程，他会把我们写的所有测试方法都执行一遍，woc，我一想那可不行啊，解决办法就是我们跳过测试流程，在maven上面这个键：

<img src="images/image-20220401103309941.png" alt="image-20220401103309941" style="zoom:80%;" />

<img src="images/image-20220401103325786.png" alt="image-20220401103325786" style="zoom:80%;" />

然后我们再次执行就不会出现Test环节了。打包完成后的jar包在我们的target目录下：

<img src="images/image-20220401103446024.png" alt="image-20220401103446024" style="zoom:80%;" />

在文件夹中打开这个目录：<img src="images/image-20220401103532061.png" alt="image-20220401103532061" style="zoom:80%;" />

==②：运行项目（执行启动指令）==

在该目录下，打开cmd命令窗口

<img src="images/image-20220401103732669.png" alt="image-20220401103732669" style="zoom:80%;" />

通过java来执行这个jar包，执行命令：

```java
java -jar demo_ssmp-0.0.1-SNAPSHOT.jar
```

![image-20220401103934426](images/image-20220401103934426.png)

哦豁，这个不就和我们在IDEA上运行程序的结果一样的嘛，那是这个服务器已经正常启动了嘛？我们打开网页访问一下我们的资源，

![image-20220401104140754](images/image-20220401104140754.png)

哇塞，那就🆗了。此时我们的程序以jar包的形式，单独通过java的jdk就可以运行，不用再依赖我们的Idea工具了。OHOHOHOHOHOHOHOHOH

#### --实现细节

为什么SpringBoot的程序这么简单就可以打包直接在jdk下运行呢？其实关键在于一个插件的使用，每个初始的springBoot都会带有这个：

<img src="images/image-20220401151656600.png" alt="image-20220401151656600" style="zoom:80%;" />

这个是实现的关键！！！

我们打开jar包看一下，可执行行jar包目录结构：

![image-20220401152225587](images/image-20220401152225587.png)

从里面可以看出来：

1. BOOT-INF：里面的第一个classes文件夹存放的内容和我们一般的maven打包后的东西一模一样，而第二个lib文件夹则存放了当前工程运行需要使用的jar包，这是一般maven打包没有的东西，就是为了不依赖程序包外部的任何资源可以独立运行当前程序。这也是为什么大的程序包容量是小的程序包容量的30倍的主要原因。

2. org：进入此目录，目录名是`org\springframework\boot\loader`，在里面可以找到一个<font color="#ff0000"><b>JarLauncher.class</b></font>的文件，先记得这个文件。再看这套目录名，明显是一个Spring的目录名，为什么要把Spring框架的东西打包到这个程序包中呢？不清楚。

3. META_IMF：jar包描述文件；

   在这个包里面有一个MANIFEST.MF的文件，我们同时打开两种情况下的该文件，比较内容：<img src="images/image-20220401153331929.png" alt="image-20220401153331929" style="zoom: 67%;" />

大文件中明显比小文件中多了几行信息，其中最后一行信息是Main-Class: org.springframework.boot.loader.<font color="#ff0000"><b>JarLauncher</b></font>。他的作用是：如果使用java -jar执行此程序包，将执行Main-Class属性配置的类，这个类恰巧就是前面看到的那个spring文件。而这个org.springframework.boot.loader.<font color="#ff0000"><b>JarLauncher</b></font>类内部要查找Start-Class属性中配置的类，并执行对应的类。这个属性在当前配置中也存在，对应的就是我们的引导类类名。

**现在这组设定的作用就搞清楚了**

1. SpringBoot程序添加配置后会打出一个特殊的包，包含Spring框架部分功能，原始工程内容，原始工程依赖的jar包
2. 首先读取MANIFEST.MF文件中的Main-Class属性，用来标记执行java -jar命令后运行的类
3. JarLauncher类执行时会找到Start-Class属性，也就是启动类类名
4. 运行启动类时会运行当前工程的内容
5. 运行当前工程时会使用依赖的jar包，从lib目录中查找

再来看有可能出现的的报错信息：

<img src="images/image-20220401153733210.png" alt="image-20220401153733210" style="zoom: 67%;" />

由于打包时没有使用那段配置，结果打包后形成了一个普通的jar包，在MANIFEST.MF文件中也就没有了Main-Class对应的属性了，所以运行时提示找不到主清单属性，这就是报错的原因。

#### --端口冲突

```cmd
# 查询端口
netstat -ano
# 查询指定端口
netstat -ano |findstr "端口号"
# 根据进程PID查询进程名称
tasklist |findstr "进程PID号"
# 根据PID杀死任务
taskkill /F /PID "进程PID号"
# 根据进程名称杀死任务
taskkill -f -t -im "进程名称"
```

##  高级配置

关于配置在基础篇讲过一部分，基础篇的配置总体上来说就是让各位小伙伴掌握配置的格式。比如配置文件如何写啊，写好的数据如何读取啊，都是基础的语法级知识。在实用篇中就要集中在配置的应用这个方面了，下面就开始配置高级相关内容的第一部分学习，为什么说第一部分，因为在开发实用篇中还有对应的配置高级知识要进行学习。

###1.  临时属性

什么意思呢？其实这是给运维的同学准备的。因为我们项目上线是用jdk直接运行springboot的jar包嘛，那我们此时就不能去更改之前开发在IDEA里面写的配置文件，这就会出现一个大问题，例如我们项目要换一个端口运行什么的，一大堆。

以需要在运行时更改端口为例，jar包里面端口配置为8080，现在运行是需要改成80。

<img src="images/image-20220405104613858.png" alt="image-20220405104613858" style="zoom:80%;" />

<img src="images/image-20220405104349421.png" alt="image-20220405104349421" style="zoom:80%;" />

办法就是在命令行启动程序时，在后面添加上对应参数就可以了。

```cmd
java –jar springboot.jar –-server.port=80
```

书写格式为：在命令输入完毕后，空一格，然后输入两个 - 号，再按照属性名=属性值的形式添加对应参数，是properties的格式。

如果你发现要修改的属性不止一个，可以按照上述格式继续写，属性与属性之间使用空格分隔。

```cmd
java –jar springboot.jar –-server.port=80 --logging.level.root=debug
```

然后我们就会发现，现在的配置生效了，说明我们的临时配置覆盖了之前的固定配置。这个配置就叫做临时配置。

### 2. 属性加载优先级

现在我们的程序配置受两个地方控制了，第一配置文件，第二临时属性。并且我们发现临时属性的加载优先级要高于配置文件的。

那问题来了，那是否还有其他的配置方式呢？其实是有的，而且还不少，打开官方文档中对应的内容，就可以查看配置读取的优先顺序。地址奉上：https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-external-config

![image-20220405105038964](images/image-20220405105038964.png)

我们可以看到，居然有14种配置的位置，而我们现在使用的是这里面的2个。第3条Config data说的就是使用配置文件，第11条Command line arguments说的就是使用命令行临时参数。而这14种配置的顺序就是SpringBoot加载配置的顺序，言外之意，命令行临时属性比配置文件的加载优先级高，所以这个列表上面的优先级低，下面的优先级高。其实这个东西不用背的，你就记得一点，你最终要什么效果，你自己是知道的，不管这个顺序是怎么个高低排序，开发时一定要配置成你要的顺序为准。这个顺序只是在你想不明白问题的时候帮助你分析罢了。

​		比如你现在加载了一个user.name属性。结果你发现出来的结果和你想的不一样，那肯定是别的优先级比你高的属性覆盖你的配置属性了，那你就可以看着这个顺序挨个排查。哪个位置有可能覆盖了你的属性。

​		我在课程评论区看到小伙伴学习基础篇的时候问这个问题了，就是这个原因造成的。在yaml中配置了user.name属性值，然后读取出来的时候居然不是自己的配置值，因为在系统属性中有一个属性叫做user.name，两个相互冲突了。而系统属性的加载优先顺序在上面这个列表中是5号，高于3号，所以SpringBoot最终会加载系统配置属性user.name。

### 3. 开发中使用临时属性

有这一部分的原因在于：有的时候我们运维的人员发现，我在命令行输入的语句没有起效果啊，你这个项目怎么回事？其实就是一个配置加载覆盖的问题，这个锅要我们开发人员背，所以我们在开发的最后环节应该把运维人员这些临时配置的东西全部测试跑通才行。

下面说一下开发环境中如何使用临时属性，其实就是Idea界面下如何操作了。

![image-20220405110408121](images/image-20220405110408121.png)

然后在程序参数那里输入临时属性：就可以成功！！！

<img src="images/image-20220405110535520.png" alt="image-20220405110535520" style="zoom:80%;" />

然后我们再次看向一下我们的引导类，

<img src="images/image-20220405110651735.png" alt="image-20220405110651735" style="zoom:80%;" />

哦豁，哇塞，不会吧，难道这个args参数就是我们的临时属性？？？答案是没错，就是这样，我们的命令行参数就是通过这个args字符串数组，给到我们的程序的，不相信的同学可以在run方法前打印一下args。

那又想到了一点，如果不用这个args是不是就断开了外部手段传递临时属性的入口呢？是这样的，如果我们在run方法时不接收这个args参数，那么外部临时属性就无法进入到SpringBoot程序中了。

<img src="images/image-20220405111112743.png" alt="image-20220405111112743" style="zoom:80%;" />

这样做的好处是：保护我们程序的不变性，稳定性！！！

或者还可以使用如下格式来玩这个操作，就是将配置不写在配置文件中，直接写成一个字符串数组，传递给程序入口。当然，这种做法并没有什么实际开发意义。

```JAVA
public static void main(String[] args) {
    String[] arg = new String[1];
    arg[0] = "--server.port=8082";
    SpringApplication.run(SSMPApplication.class, arg);
}
```

### 4. 配置文件分类

springboot给配置分等级的原因：满足不同人的需求！！！一是后台开发人员，二是项目经理，三是别人公司（他们有高级机密配置）等；因为之前命令行的方式太捞了呗，显得多不专业，每次写来写去，万一改的地方特别多怎么搞！！！所以有了下面这种方式，通过配置文件覆盖的方法满足不同人的需求。

<img src="images/image-20220405112745338.png" alt="image-20220405112745338" style="zoom:80%;" />

SpringBoot提供了配置文件和临时属性的方式来对程序进行配置。前面一直说的是临时属性，这一节要说说配置文件了。其实这个配置文件我们一直在使用，只不过我们用的是SpringBoot提供的4级配置文件中的其中一个。4个级别分别是：

- 程序包jar所在目录中config目录下配置文件；
- 程序包jar所在目录中配置文件；
- 类路径下配置文件（就是resources目录中的application.yml文件）；
- 类路径下config目录下配置文件；

​		好复杂，一个一个说。其实上述4种文件是提供给你了4种配置文件书写的位置，功能都是一样的，都是做配置的。他们的区别就是存放的位置不同导致了他们四个的配置优先级不同；上面4个文件的加载优先顺序为：

1. **file ：config/application.yml** 【最高】	（jar包）
2. **file ：application.yml**    （jar包）
3. **classpath：config/application.yml**    （resources下）
4. **classpath：application.yml**   【最低】    （resources下）

1级覆盖二级，二级覆盖三级,.....采用叠加并覆盖的形式，有则改之，无则加上。

一般而言：四种配置文件的使用者为：

| 配置文件等级 | 使用者                   |
| ------------ | ------------------------ |
| 4级          | 开发者本机开发与测试     |
| 3级          | 项目经理整体调控         |
| 2级          | 运维人员配置涉密线上环境 |
| 1级          | 运维经理整体调控         |

举例说明一下哈：

**开发人员：**

<img src="images/image-20220405114849188.png" alt="image-20220405114849188" style="zoom:80%;" />

**项目经理：**

<img src="images/image-20220405114520618.png" alt="image-20220405114520618" style="zoom:80%;" />

**运维人员：**

<img src="images/image-20220405114638638.png" alt="image-20220405114638638" style="zoom:80%;" />

**运维经理：**

<img src="images/image-20220405114809968.png" alt="image-20220405114809968" style="zoom: 67%;" />

### 5. 自定义配置文件名称

我们会发现，每次初始化过程时，自带了一个配置文件`application.properties`，并且我们一般会把他后缀改成yml格式的；但也没有想过这个文件的名称可以随便改吗？答案是不可以的，如果一定要改，就需要进行一些配置，让springboot知道哪个是配置文件。

​	自定义配置文件名方式有如下两种：

**方式一：使用临时属性设置配置文件名，注意仅仅是名称，不要带扩展名**

<img src="images/image-20211206105548238.png" alt="image-20211206105548238" style="zoom:67%;" />

**方式二：使用临时属性设置配置文件路径，这个是全路径名**

<img src="images/image-20211206105716450.png" alt="image-20211206105716450" style="zoom:67%;" />

​		也可以设置加载多个配置文件，顺序在后面的会覆盖前面的已有配置

<img src="images/image-20211206105750285.png" alt="image-20211206105750285" style="zoom:67%;" />

​		使用的属性一个是spring.config.name，另一个是spring.config.location，这个一定要区别清楚。

<font color="#f0f"><b>温馨提示</b></font>

​		我们现在研究的都是SpringBoot单体项目，就是单服务器版本。其实企业开发现在更多的是使用基于SpringCloud技术的多服务器项目。这种配置方式和我们现在学习的完全不一样，所有的服务器将不再设置自己的配置文件，而是通过配置中心获取配置，动态加载配置信息。为什么这样做？集中管理。这里不再说这些了，后面再讲这些东西。

## 多环境开发

讲的内容距离线上开发越来越近了，下面说一说多环境开发问题。

​		什么是多环境？其实就是说你的电脑上写的程序最终要放到别人的服务器上去运行。每个计算机环境不一样，这就是多环境。常见的多环境开发主要兼顾3种环境设置，开发环境——自己用的，测试环境——自己公司用的，生产环境——甲方爸爸用的。因为这是绝对不同的三台电脑，所以环境肯定有所不同，比如连接的数据库不一样，设置的访问端口不一样等等。

<img src="images/image-20220405164902986.png" alt="image-20220405164902986" style="zoom: 67%;" />

### 1. YML单一文件版

什么意思呢，就是在一个yml文件里面配置多个环境，然后可选择程序加载哪一个环境。

方法：通过三个减号 “ - ”，进行配置文件的内部划分

```yml
# 应用环境（去额外加载下面哪一个环境配置）
# 公共配置（这里放下面各个环境重复的一些配置，也可以不放）
spring:
  profiles:
    active: dev

# 生产环境
---
spring:
  profiles: pro  # 指定环境名称
server:
  port: 80

# 开发环境
---
spring:
  profiles: dev
server:
  port: 81

# 测试环境
---
spring:
  profiles: test
server:
  port: 82
```

并且打开配置文件后可以在文件左下角，看到整个配置文件被分成了几个部分：

<img src="images/image-20220405170318466.png" alt="image-20220405170318466" style="zoom:67%;" />

其中，这里给我们的profiles划了一个横线，意思是这种写法过时了，推荐使用新的写法，不过这个无所谓，都可以。

<img src="images/image-20220405170524714.png" alt="image-20220405170524714" style="zoom:67%;" />

### 2. YML多文件版

因为上面那种简直了，怎么可能进行合作配呢，对不对。所以需要把上面三个环境拆成三个单独的配置文件。

实现方式是：主配置文件通过特定的文件名称来识别需要加载的配置文件；

<img src="images/image-20220405174226619.png" alt="image-20220405174226619" style="zoom: 80%;" />

<img src="images/image-20220405174112713.png" alt="image-20220405174112713" style="zoom:80%;" />

**注意：**

-  主配置文件中设置公共配置（全局）
-  环境分类配置文件中常用于设置冲突属性（局部）

<img src="images/image-20220405174331854.png" alt="image-20220405174331854" style="zoom:80%;" />

### 3. properties多文件版

同前面的yml，一模一样几乎。

![image-20220405174533983](images/image-20220405174533983.png)

### 4. 多环境开发独立配置文件书写技巧

 **根据功能对配置文件中的信息进行拆分，并制作成独立的配置文件，命名规则如下**

-  application-devDB.yml（数据库配置）
-  application-devRedis.yml（Redis）
-  application-devMVC.yml（SpringMVC）

**使用include属性在激活指定环境的情况下，同时对多个环境进行加载使其生效，多个环境间使用逗号分隔**

<img src="images/image-20220405175614960.png" alt="image-20220405175614960" style="zoom:80%;" />

<img src="images/image-20220405175637901.png" alt="image-20220405175637901" style="zoom:80%;" />

 从Spring2.4版开始使用group属性替代include属性，降低了配置书写量。

 使用**group属性**定义多种主环境与子环境的包含关系

<img src="images/image-20220405175800318.png" alt="image-20220405175800318" style="zoom:80%;" />

### 5. 多环境开发控制

多环境开发到这里基本上说完了，最后说一个冲突问题。就是maven和SpringBoot同时设置多环境的话怎么搞。

解决这个问题的第一步，搞清楚这两个之间的关系。springboot是不是依赖maven进行开发的呢，是，maven是做什么的？项目构建管理的，最终生成代码包的，SpringBoot是干什么的？简化开发的。所以说最终还是要靠maven来管理整个工程，所以SpringBoot应该听maven的，Maven在多环境开发中占了主导地位。

maven去控制springboot的多环境大体思想如下：

- 先在maven环境中设置用什么具体的环境
- 在SpringBoot中读取maven设置的环境即可

**①：Maven中设置多环境属性**

![image-20220405210132945](images/image-20220405210132945.png)

**②：SpringBoot中引用Maven属性**

哪一个profile里面设置了active为true就读取哪一个的配置信息。

![image-20220405210150037](images/image-20220405210150037.png)

下面是原来的写法，其实就是把这个active的取值，让maven去控制，不让我们在yml文件写而已，没有什么特殊的。上面的`@属性名@`就是读取maven中配置的属性值的语法格式。

<img src="images/image-20220405210331031.png" alt="image-20220405210331031" style="zoom:67%;" />

**③：执行Maven打包指令，并在生成的boot打包文件.jar文件中查看对应信息**

## 日志

### 1. 日志基础

日志（log）作用

-  **编程期调试代码**
-  **运营期记录信息**

​			记录日常运营重要信息（峰值流量、平均响应时长……） 

​			记录应用报错信息（错误堆栈）

​			记录运维过程数据（扩容、宕机、报警……）

==**代码中使用日志工具记录日志**==

**步骤①**：添加日志记录操作。这个超级简单

代码中log对象就是用来记录日志的对象，下面的log.debug，log.info这些操作就是写日志的API了。

![image-20220407161806316](images/image-20220407161806316.png)

**步骤②**：设置日志输出级别；

日志的级别分为6种，分别是：

- TRACE：运行堆栈信息，使用率低
- **DEBUG：程序员调试代码使用**（开发）
- **INFO：记录运维过程数据**（上线）
- **WARN：记录运维过程报警数据**（运维）
- **ERROR：记录错误堆栈信息**
- FATAL：灾难信息，合并计入ERROR

我们在yml文件内控制日志得输出级别：

```yml
# 设置日志级别，root表示根节点，即整体应用日志级别
logging:
	level:
    	root: debug
```

**步骤③**：设置日志组，控制指定包对应的日志输出级别。更细粒度的控制

```yml
logging:
  #  设置分组，每一个组下可放多个包
  group:
    #  一个组名为ebank，一个为iservice
    ebank: com.CCooky.controller,com.CCooky.service
    iservice: com.alibaba
  level:
    #  整体输出均为info级别
    root: info
    #  单独设置日志组的日志输出级别
    ebank: warn
```

==**教你一招：优化日志对象创建代码**==

之前那样写日志存在的问题：每一个类都需要获得一个日志对象，重复代码。

<img src="images/image-20220407164927806.png" alt="image-20220407164927806" style="zoom:80%;" />

使用lombok提供的注解**@Slf4j**简化开发，减少日志对象的声明操作

![image-20220407165103269](images/image-20220407165103269.png)

### 2. 日志输出格式

日志已经能够记录了，但是目前记录的格式是SpringBoot给我们提供的，如果想自定义控制就需要自己设置了。先分析一下当前日志的记录格式。

![image-20211206123431222](images/image-20211206123431222.png)

- PID：进程ID，用于表明当前操作所处的进程，当多服务同时记录日志时，该值可用于协助程序员调试程序
- 所属类/接口名：当前显示信息为SpringBoot重写后的信息，名称过长时，简化包名书写为首字母，甚至直接删除		

对于单条日志信息来说，日期，触发位置，记录信息是最核心的信息。级别用于做筛选过滤，PID与线程名用于做精准分析。了解这些信息后就可以DIY日志格式了。本课程不做详细的研究，有兴趣的小伙伴可以学习相关的知识。下面给出课程中模拟的官方日志模板的书写格式，便于大家学习。

```yml
# %d---事件，%clr---彩色标注，%p---级别，PID我们自己无法直接获取，%t---进程
# %c---类名，%m---日志信息，%n---换行
logging:
	pattern:
    	console: "%d %clr(%p) --- [%16t] %clr(%-40.40c){cyan} : %m %n"
```

### 3. 日志文件

日志不能仅显示在控制台上，要把日志记录到文件中，方便后期维护查阅。对于日志文件的使用存在各种各样的策略，例如每日记录，分类记录，报警后记录等。这里主要研究日志文件如何记录。

记录日志到文件中格式非常简单，直接设置日志文件名即可。

```yml
logging:
	file:
    	name: server.log
```

然后启动我们的项目，直接在IDEA是找不到这个文件的，他在这个模块的同级目录下：

<img src="images/image-20220407170823284.png" alt="image-20220407170823284" style="zoom:80%;" />

新的问题来了，难道我们项目就一直用这个文件吗？那这个文件不就越来越大呢！！！肯定是不行的，所以我们要对日志进行分开记录。通常会每天记录日志文件，同时为了便于维护，还要限制每个日志文件的大小。

下面给出日志文件的常用配置方式：

```YAML
logging:
	logback:
    	rollingpolicy:
        	max-file-size: 3KB
            file-name-pattern: server.%d{yyyy-MM-dd}.%i.log
           # 日志文件大小最大为3kb
           # 日志文件的命名模板---server.2022-04-07.0.log
```

​		以上格式是基于logback日志技术设置每日日志文件的设置格式，要求容量到达3KB以后就转存信息到第二个文件中。文件命名规则中的%d标识日期，%i是一个递增变量，用于区分日志文件。

# SpringBoot开发实用篇

前言：开发实用篇中因为牵扯到SpringBoot整合各种各样的技术，由于不是每个小伙伴对各种技术都有所掌握，所以在整合每一个技术之前，都会做一个快速的普及，这样的话内容整个开发实用篇所包含的内容就会比较多。各位小伙伴在学习的时候，如果对某一个技术不是很清楚，可以先跳过对应章节，或者先补充一下技术知识，然后再来看对应的课程。

开发实用篇具体包含的内容如下：

- 热部署
- 配置高级
- 测试
- 数据层解决方案
- 整合第三方技术
- 监控

看目录感觉内容量并不是很大，但是在数据层解决方案和整合第三方技术中包含了大量的知识，一点一点慢慢学吧。

## 1. 热部署

简单说：就是我们程序代码更改了之后，不用再去重启服务器就可以获得该有的效果。

关于热部署的两个概念 ：

- **重启（Restart）**：自定义开发代码，包含类、页面、配置文件等，加载位置restart类加载器；
- **重载（ReLoad）**：jar包，加载位置base类加载器；

项目第一次启动服务时，重启＋重载；热部署时仅仅重启，所以说热部署可以很快的实现效果。

**springboot项目热部署实现原理**

​		基于springboot开发的web工程其实有一个显著的特征，就是tomcat服务器内置了，还记得内嵌服务器吗？服务器是以一个对象的形式在spring容器中运行的。本来我们期望于tomcat服务器加载程序后由tomcat服务器盯着程序，你变化后我就重新启动重新加载，但是现在tomcat和我们的程序是平级的了，都是spring容器中的组件，这下就麻烦了，缺乏了一个直接的管理权，那该怎么做呢？简单，再搞一个程序X在spring容器中盯着你原始开发的程序A不就行了吗？确实，搞一个盯着程序A的程序X就行了，如果你自己开发的程序A变化了，那么程序X就命令tomcat容器重新加载程序A就OK了。并且这样做有一个好处，spring容器中东西不用全部重新加载一遍，只需要重新加载你开发的程序那一部分就可以了，这下效率又高了，挺好。

​	下面就说说，怎么搞出来这么一个程序X，肯定不是我们自己手写了，springboot早就做好了，搞一个坐标导入进去就行了。

**步骤①**：导入开发者工具对应的坐标

```XML
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <optional>true</optional>
</dependency>
```

**步骤②**：构建项目，可以使用快捷键激活此功能

![image-20220222121257218](images/image-20220222121257218.png)

​		以上过程就实现了springboot工程的热部署，每次更改代码完，重启构建一下项目就好了。

### --自动热部署

有的人嫌麻烦，其实也还好，就是说每次改完代码后，还要自己去Build一下，其实也可以让IDEA自动去Build。IDEA是有这样的设置的如下：

**步骤①**：设置自动构建项目

​		打开【File】，选择【settings...】,在面板左侧的菜单中找到【Compile】选项，然后勾选【Build project automatically】，意思是自动构建项目

![image-20220222123543551](images/image-20220222123543551.png)

​		自动构建项目选项勾选后

**步骤②**：允许在程序运行时进行自动构建

​		使用快捷键【Ctrl】+【Alt】+【Shit】+【/】打开维护面板，选择第1项【Registry...】

![image-20220222124006910](images/image-20220222124006910.png)

​		在选项中搜索comple，然后勾选对应项即可

<img src="images/image-20220222124240069.png" alt="image-20220222124240069" style="zoom:80%;" />

​		这样程序在运行的时候就可以进行自动构建了，实现了热部署的效果。

<font color="#ff0000"><b>IDEA自动Build特点</b></font>：当我们的鼠标点击到IDEA以外时，即idea工具失去焦点，后5s，他会自动进行Build。

### --参与热部署监控的文件范围配置

通过实际操作，你会发现，欸呀，怎么好像有的代码文件参与了热部署，有的改了没有生效了。其实并不是所有的文件都会参与热部署的，这里有一个范围控制。

配置中默认不参与热部署的目录信息如下：

- /META-INF/maven
- /META-INF/resources
- /resources
- /static
- /public
- /templates

当然我们可以自己去更改这个配置：

```yml
spring:
  devtools:
    restart:
      # 设置不参与热部署的文件或文件夹
      exclude: static/**,public/**,config/application.yml
```

热部署功能是一个典型的开发阶段使用的功能，到了线上环境运行程序时，这个功能就没有意义了。能否关闭热部署功能呢？

### --关闭热部署

线上环境运行时是不可能使用热部署功能的，所以需要强制关闭此功能，通过配置可以关闭此功能。

```yaml
spring:
  devtools:
    restart:
      enabled: false
```

​		如果当心配置文件层级过多导致相符覆盖最终引起配置失效，可以提高配置的层级，在更高层级中配置关闭热部署。

![image-20220408103437502](images/image-20220408103437502.png)例如在启动容器前通过系统属性设置关闭热部署功能。

```JAVA
@SpringBootApplication
public class SSMPApplication {
    public static void main(String[] args) {
        System.setProperty("spring.devtools.restart.enabled","false");
        SpringApplication.run(SSMPApplication.class);
    }
}
```

## 	2.@ConfigurationProperties

这个之前在学习yml问价的时候讲过一遍了，他是用来读取yml文件内的对象数据的，并且绑定到我们自定义的一个类的属性中。

这里主要是讲：这个注解不仅可以为我们自定义Bean绑定属性，也可以为第三方的Bean绑定属性。第三方Bean就是指例如Druid里面的DruidDataSource对象，我们通过在yml文件内写值，然后把该值赋给对象里面的属性。

==注意前缀是全小写的datasource, 而yml中为驼峰==

<img src="images/image-20220408151534868.png" alt="image-20220408151534868" style="zoom:80%;" />

<img src="images/image-20220408152631594.png" alt="image-20220408152631594" style="zoom:80%;" />

运行结果：

<img src="images/image-20220408151603021.png" alt="image-20220408151603021" style="zoom:80%;" />

==**@EnableConfigurationProperties**==

除此之外，还有一个和@ConfigurationProperties注解很像的注解，我们也经常在springboot见到，@EnableConfigurationProperties。他的使用如下：

- @EnableConfigurationProperties注解可以将使用@ConfigurationProperties注解对应的类直接加入Spring容器

<img src="images/image-20220408152013137.png" alt="image-20220408152013137" style="zoom:80%;" />

<img src="images/image-20220408152025973.png" alt="image-20220408152025973" style="zoom:80%;" />

总结一句话就是，他换了一种方式，把我们需要绑定yml对象数据的类放到spring容器里面，让这个类成为一个Bean，这样做的好处是：我们可以在引导类上直接看到，有哪些Bean的属性绑定了yml配置数据。同时使用，就会在容器里面产生两个相同的Bean。

<img src="images/image-20220408152241469.png" alt="image-20220408152241469" style="zoom:80%;" />

**解除使用@ConfigurationProperties注释警告**

![image-20220408152326821](images/image-20220408152326821.png)

直接导入一个依赖就行了，

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-configuration-processor</artifactId>
</dependency>
```

### 	--松散绑定

这里就是Springboot在使用@ConfigurationProperties进行Bean属性绑定时，yml对象属性名与Bean属性名之间的命名规则。是springboot进行编程时人性化设计的一种体现，即配置文件中的命名格式与变量名的命名格式可以进行格式上的最大化兼容。

**第一点：绑定前缀名**

springboot推荐使用烤肉串的形式，仅能使用纯小写字母、数字、中划线作为合法的字符（data-source），中划线在真正绑定时会自动去掉。但我们一般就用纯小写字母。

<img src="images/image-20220408154452267.png" alt="image-20220408154452267" style="zoom:80%;" />

**第二点：属性名绑定**

在ServerConfig中的ipAddress属性名

```JAVA
@Data
@ConfigurationProperties(prefix = "servers")
public class ServerConfig {
    private String ipAddress;
}
```

​		可以与下面的配置属性名规则全兼容

```YML
servers:
  ipAddress: 192.168.0.2       # 驼峰模式
  ip_address: 192.168.0.2      # 下划线模式
  ip-address: 192.168.0.2      # 烤肉串模式
  IP_ADDRESS: 192.168.0.2      # 常量模式
```

也可以说，以上4种模式最终都可以匹配到ipAddress这个属性名。

原因就是在进行匹配时，配置中的名称要**去掉中划线和下划线后，忽略大小写**的情况下去与java代码中的属性名进行**忽略大小写的**等值匹配！！

### --常用计量单位绑定

这里就介绍了两个常用的单位，一个是时间单位，一个是存储容量单位。

**举个例子说明：**

在前面的配置中，我们书写了如下配置值，其中第三项超时时间timeout描述了服务器操作超时时间，当前值是-1表示永不超时。

```YML
servers:
  ip-address: 192.168.0.1 
  port: 2345
  timeout: -1
```

​		但是每个人都这个值的理解会产生不同，比如线上服务器完成一次主从备份，配置超时时间240，这个240如果单位是秒就是超时时间4分钟，如果单位是分钟就是超时时间4小时。面对一次线上服务器的主从备份，设置4分钟，简直是开玩笑，别说拷贝过程，备份之前的压缩过程4分钟也搞不定，这个时候问题就来了。

**解决办法：**

springboot充分利用了JDK8中提供的全新的用来表示计量单位的新数据类型，从根本上解决这个问题。分别是Duration和DataSize两个类。

```java
@Data
@ConfigurationProperties(prefix = "servers")
public class ServerConfig {
    @DurationUnit(ChronoUnit.HOURS)
    private Duration serverTimeOut;
  
    @DataSizeUnit(DataUnit.MEGABYTES)
    private DataSize dataSize;
}
```

**Duration**：表示时间间隔，可以通过@DurationUnit注解描述时间单位，例如上例中描述的单位为小时（ChronoUnit.HOURS）

**DataSize**：表示存储空间，可以通过@DataSizeUnit注解描述存储空间单位，例如上例中描述的单位为MB（DataUnit.MEGABYTES）

两个类支持的所有单位如下：

![image-20220408160220145](images/image-20220408160220145.png)

### --数据校验

好了，问题又来了。

<img src="images/image-20220408160412570.png" alt="image-20220408160412570" style="zoom:80%;" />

我的timeout绑定的Bean属性为时间单位的分钟，但就是有的人总会写错，把这里写成非数字，因为yml目前对对象的值没有限制，例如写成abcd，那就会报错了，无法绑定。

对此，springboot提供了数据校验，即对yml文件对象属性值的书写进行格式限制。J2EE规范中JSR303规范定义了一组有关数据校验相关的API，springboot把他整合好了，直接拿来用就行了。

**①：添加JSR303规范坐标与Hibernate校验框架对应坐标**

```xml
<!--1.导入JSR303规范（接口）-->
<dependency>
    <groupId>javax.validation</groupId>
    <artifactId>validation-api</artifactId>
</dependency>
<!--使用hibernate框架提供的校验器做实现类-->
<dependency>
    <groupId>org.hibernate.validator</groupId>
    <artifactId>hibernate-validator</artifactId>
</dependency>
```

**步骤②**：在需要开启校验功能的类上使用注解@Validated开启校验功能

```java
@Component
@Data
@ConfigurationProperties(prefix = "servers")
//开启对当前bean的属性注入校验
@Validated
public class ServerConfig {
}
```

**步骤③**：对具体的字段设置校验规则

```JAVA
@Component
@Data
@ConfigurationProperties(prefix = "servers")
//开启对当前bean的属性注入校验
@Validated
public class ServerConfig {
    //设置具体的规则
    @Max(value = 8888,message = "最大值不能超过8888")
    @Min(value = 202,message = "最小值不能低于202")
    private int port;
}
```

​		通过设置数据格式校验，就可以有效避免非法数据加载，其实使用起来还是挺轻松的，基本上就是一个格式。当出现格式不正确时，运行就会报错，并且告诉你错误地方。

<img src="images/image-20220408163702499.png" alt="image-20220408163702499" style="zoom:80%;" />

最后，javase提供给我们了多少种约束规则呢，在哪里查看呢，如下：

![image-20220408164123476](images/image-20220408164123476.png)

然后在左边就可以看到全部了。这里都是JSR303规范的API

<img src="images/image-20220408164154848.png" alt="image-20220408164154848" style="zoom:80%;" />

还有一部分规范的API，是hibernate框架提供给我们的，

<img src="images/image-20220408165959434.png" alt="image-20220408165959434" style="zoom: 67%;" />

<img src="images/image-20220408170029797.png" alt="image-20220408170029797" style="zoom:80%;" />

### --数据类型转换

有关spring属性注入的问题到这里基本上就讲完了，但是最近一名开发者向我咨询了一个问题，我觉得需要给各位学习者分享一下。在学习阶段其实我们遇到的问题往往复杂度比较低，单一性比较强，但是到了线上开发时，都是综合性的问题，而这个开发者遇到的问题就是由于bean的属性注入引发的灾难。

​		先把问题描述一下，这位开发者连接数据库正常操作，但是运行程序后显示的信息是密码错误。

```CMD
java.sql.SQLException: Access denied for user 'root'@'localhost' (using password: YES)
```

​		其实看到这个报错，几乎所有的学习者都能分辨出来，这是用户名和密码不匹配，就就是密码输入错了，但是问题就在于密码并没有输入错误，这就比较讨厌了。给的报错信息无法帮助你有效的分析问题，甚至会给你带到沟里。如果是初学者，估计这会心态就崩了，我密码没错啊，你怎么能说我有错误呢？来看看用户名密码的配置是如何写的：

```YAML
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC
    username: root
    password: 0127
```

​		这名开发者的生日是1月27日，所以密码就使用了0127，其实问题就出在这里了。

​		之前在基础篇讲属性注入时，提到过类型相关的知识，在整数相关知识中有这么一句话，**支持二进制，八进制，十六进制**

<img src="images/image-20220222225748370.png" alt="image-20220222225748370" style="zoom:80%;" />

而八进制的要求就是： ” 数字0开头，后面跟0—7“，十六进制的要求：”数字0x开头，后面跟0-9，a-f “	。

这个问题就处在这里了，因为0127在开发者眼中是一个字符串“0127”，但是在springboot看来，这就是一个数字，而且是一个八进制的数字。当后台使用String类型接收数据时，如果配置文件中配置了一个整数值，他是先安装整数进行处理，读取后再转换成字符串。巧了，0127撞上了八进制的格式，所以最终以十进制数字87的结果存在了。

​		**这里提两个注意点，第一，字符串标准书写加上引号包裹，养成习惯，第二，遇到0开头的数据多注意吧。**

## 3. 测试

### --加载临时配置属性

**什么意思呢？**

就是说在我们@springbootest这个测试用例里面，单独加一些yml文件那种的配置信息，仅仅在该测试用例有效。

**为什么这样做呢？**

测试过程本身并不是一个复杂的过程，但是很多情况下测试时需要模拟一些线上情况，或者模拟一些特殊情况。如果当前环境按照线上环境已经设定好了，例如是下面的配置

```YAML
env:
  maxMemory: 32GB
  minMemory: 16GB
```

​	但是你现在想测试对应的兼容性，需要测试如下配置

```YAML
env:
  maxMemory: 16GB
  minMemory: 8GB
```

​	这个时候我们能不能每次测试的时候都去修改源码application.yml中的配置进行测试呢？显然是不行的。每次测试前改过来，每次测试后改回去，这太麻烦了。于是我们就想，需要在测试环境中创建一组临时属性，去覆盖我们源码中设定的属性，这样测试用例就相当于是一个独立的环境，能够独立测试，这样就方便多了。

**添加临时配置属性方式：**

点击进去这个@springbootest注解，内部有几个属性，其中properties和args就是给我们添加临时配置属性滴

<img src="images/image-20220408193121668.png" alt="image-20220408193121668" style="zoom:80%;" />

**properties属性：**

等同于在yml文件里面加的那种意思，属于springboot配置文件优先级里面的那个等级。

==开发人员一般写这个==

```java
//properties属性可以为当前测试用例添加临时的属性配置
@SpringBootTest(properties = {"test.prop=testValue1","..."})
public class PropertiesAndArgsTest {

    @Value("${test.prop}")
    private String msg;
    
    @Test
    void testProperties(){
        System.out.println(msg);
    }
}
```

<img src="images/image-20220408193515640.png" alt="image-20220408193515640" style="zoom:80%;" />

**args属性：**

这个就牛逼一点，还记得我们引导类的那个args字符串数组吗，没错，这个和那个是一样的，属于添加临时的命令行参数；配置的优先级高于上面那个。

==需要给运维测试命令行测试时用==

```java
//args属性可以为当前测试用例添加临时的命令行参数
@SpringBootTest(args={"--test.prop=testValue2"})
public class PropertiesAndArgsTest {
    
    @Value("${test.prop}")
    private String msg;
    
    @Test
    void testProperties(){
        System.out.println(msg);
    }
}
```

### --加载临时的Bean

解释：我想在我的这个测试案例临时加载一些Bean，来辅助我进行测试。

为什么会有这个需求呢，往前面的那个简单案例看看，其中有一个MP的分页功能的实现，里面是不是我们手动加载了一个第三方的分页拦截器的Bean到容器里面而实现的这个功能呢！！，所以说大概就是这个意思，实际开发是有这个需求的。

做法：学习过Spring的知识，我们都知道，其实一个spring环境中可以设置若干个配置文件或配置类，若干个配置信息可以同时生效。现在我们的需求就是在测试环境中再添加一个配置类，然后启动测试环境时，生效此配置就行了（规定第三方Bean均在配置文件里面进行添加）

注意事项：引导类会自动扫描并且加载所在包及其子包下所有的注解，既然我们要测试案例专用，就一定不能写在源码下面！！！

**具体操作步骤如下：**

**步骤①**：在测试包test中创建专用的测试环境配置类

<img src="images/image-20220413171545308.png" alt="image-20220413171545308" style="zoom:80%;" />

下面这种方式很聪明哦，给容器里面加一个String类的Bean，用于测试一下

```java
@Configuration
public class MsgConfig {

    @Bean
    public String msg(){
        return "bean msg";
    }
}
```

**步骤②**：在启动测试环境时，导入测试环境专用的配置类，使用**@Import**注解即可实现。

```java
@SpringBootTest
@Import(MsgConfig.class) //需要导入配置类哦，实现测试用例的单独添加Bean
public class ConfigurationTest {

    @Autowired
    private String msg;

    @Test
    public void test1(){
        System.out.println(msg);
    }

}
```

### --Web环境模拟测试

 Why：前面我们做案例的时候，不是都会对Mapper层和Service层进行接口测试嘛，不过一般开发只测试Service就好了，对于Web（Controller层）我们是没有管的，是通过ApiPost进行测试的，但实际开发测试中不可能这样做的！！！因为我们的SpringBoot程序在打包的时候，会把测试用例全部跑一遍以保证程序的正确性，如果在ApiPost测试表现层，那就无法保证表现层的稳定性，而且手动 测试很捞哦，真的很捞哦。

分析：在测试中对表现层功能进行测试需要一个基础和一个功能。所谓的一个基础是运行测试程序时，必须启动web环境，不然没法测试web功能。一个功能是必须在测试程序中具备发送web请求的能力，不然无法实现web功能的测试。所以在测试用例中测试表现层接口这项工作就转换成了两件事，一，如何在测试类中启动web测试，二，如何在测试类中发送web请求。

==**第一步：测试类中启动Web环境**==

之前我们写的一般测试类是这样的，然后他的运行结果是这样的：

![image-20220413194314894](images/image-20220413194314894.png)

很明显，这没有启动Web服务器啊，因为他连端口号什么的都没有对不对，所以说要另外加一些配置信息，才能让测试类也可以启动Web服务器。

点进去@SpringBootTest注解，查看一下他里面的属性，我们会发现有一个Web的环境属性，那就是他了。

<img src="images/image-20220413194540165.png" alt="image-20220413194540165" style="zoom:80%;" />

直接在注解后面加上该属性的配置，如下：一共有四个配置信息，

<img src="images/image-20220413194733514.png" alt="image-20220413194733514" style="zoom:80%;" />

- MOCK：根据当前设置确认是否启动web环境，例如使用了Servlet的API就启动web环境，属于适配性的配置
- **DEFINED_PORT**：使用自定义的端口作为web服务器端口
- **RANDOM_PORT**：使用随机端口作为web服务器端口
- NONE：不启动web环境

接下来，再次启动测试类，就有了：

<img src="images/image-20220413194920469.png" alt="image-20220413194920469" style="zoom:80%;" />

==**第二步：测试类中发送请求**==

这一套写法比较规定，是Springboot的东西。

```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
// 开启虚拟MVC调用
@AutoConfigureMockMvc
public class WebTest {
    
    // 注入虚拟mvc调用对象
    @Autowired
    MockMvc mvc;

    @Test
    void test1() throws Exception {
        // 我们要去调用 http://localhost:8080/books
        // 创建虚拟Get请求，当前访问  /books
        MockHttpServletRequestBuilder builder = MockMvcRequestBuilders.get("/books");
        // 执行对应请求
        mvc.perform(builder);
    }
}
```

运行该test1后结果如下：

执行测试程序，现在就可以正常的发送/books对应的请求了，注意访问路径不要写http://localhost:8080/books，因为前面的服务器IP地址和端口使用的是当前虚拟的web环境，无需指定，仅指定请求的具体路径即可。

![image-20220413201404161](images/image-20220413201404161.png)

==**第三步：验证表现层接口结果**==

我们已经可以成功发起请求了，接下来就是测试表现层接口结果的准确性，即将我们的预期值与响应对象里面的值进行对比。

响应对象内含有多个内容，常见的比对内容如下：

1. **响应状态匹配**

   ```java
       @Test
       void test1() throws Exception {
           // 我们要去调用 http://localhost:8080/books
           // 创建虚拟Get请求，当前访问  /books
           MockHttpServletRequestBuilder builder = MockMvcRequestBuilders.get("/books");
           // 执行对应请求
           ResultActions actions = mvc.perform(builder);
   
   //设定预期值，与真实值进行比较
           // 响应状态
           StatusResultMatchers status = MockMvcResultMatchers.status();
           ResultMatcher matcher = status.isOk();// 设置预期值：状态200
           actions.andExpect(matcher); // 与真实值进行比对
   
       }
   }
   ```

   我们模拟一下，出现错误的情况，例如把路径改成books111，然后我们在控制台可以看到很多信息：

   <img src="images/image-20220413202713858.png" alt="image-20220413202713858" style="zoom:80%;" />

​	除此之外，前面我们还可以看到请求信息和响应信息，

<img src="images/image-20220413202808385.png" alt="image-20220413202808385" style="zoom:80%;" />

2. **响应体匹配（非json数据格式）**

   表现层我们直接访问接口，return的结果是字符串——“spring boot is running”

```java
    @Test
    void testResposeBody() throws Exception {
        // 我们要去调用 http://localhost:8080/books
        // 创建虚拟Get请求，当前访问  /books
        MockHttpServletRequestBuilder builder = MockMvcRequestBuilders.get("/books");
        // 执行对应请求
        ResultActions actions = mvc.perform(builder);

//设定预期值，与真实值进行比较
        // 响应状态
        ContentResultMatchers content = MockMvcResultMatchers.content();
        ResultMatcher matcher = content.string("springboot");// 设置预期值：字符串“springboot”
        actions.andExpect(matcher); // 与真实值进行比对
    }
```

3. **响应体匹配（json数据格式，开发中的主流使用方式）**

即返回值为对象或者集合。会自动转换成json串。我们先把表现层的返回值改成如下：

<img src="images/image-20220416112043245.png" alt="image-20220416112043245" style="zoom:80%;" />

然后启动服务器，在浏览器中可以看到响应结果：

<img src="images/image-20220416112329318.png" alt="image-20220416112329318" style="zoom:80%;" />

然后把这个json串复制一下哦。因为后面要用的，不然得自己写很麻烦。只用改一个地方就好了，用到得函数是.json，然后在参数双引号里面，直接复制之前得json串即可。

```java
    @Test
    void testResposeBody2() throws Exception {
        // 我们要去调用 http://localhost:8080/books
        // 创建虚拟Get请求，当前访问  /books
        MockHttpServletRequestBuilder builder = MockMvcRequestBuilders.get("/books");
        // 执行对应请求
        ResultActions actions = mvc.perform(builder);

//设定预期值，与真实值进行比较
        // 响应状态
        ContentResultMatchers content = MockMvcResultMatchers.content();
        ResultMatcher matcher = content.json("{\"id\":1,\"name\":\"spring boot\",\"type\":\"Java学习\"}");
        actions.andExpect(matcher); // 与真实值进行比对
    }
```

做到这里，相信大家已经发现，其实匹配结果得写法都差不多，就我们响应数据里面有什么东西，我们就可以去匹配什么东西，很简单得，那我们再写一个简单的匹配，先看看响应数据还有什么内容：

<img src="images/image-20220416113233769.png" alt="image-20220416113233769" style="zoom:80%;" />

那我们就匹配一下这个Header吧，响应头。

4. **响应头信息匹配**

```java
@Test
void testContentType(@Autowired MockMvc mvc) throws Exception {
    MockHttpServletRequestBuilder builder = MockMvcRequestBuilders.get("/books");
    ResultActions action = mvc.perform(builder);
    //设定预期值 与真实值进行比较，成功测试通过，失败测试失败
    //定义本次调用的预期值
    HeaderResultMatchers header = MockMvcResultMatchers.header();
    ResultMatcher contentType = header.string("Content-Type", "application/json");
    //添加预计值到本次调用过程中进行匹配
    action.andExpect(contentType);
}
```

**总结**

基本上齐了，头信息，正文信息，状态信息都有了，就可以组合出一个完美的响应结果比对结果了。以下范例就是三种信息同时进行匹配校验，也是一个完整的信息匹配过程。

```JAVA
@Test
void testGetById(@Autowired MockMvc mvc) throws Exception {
    MockHttpServletRequestBuilder builder = MockMvcRequestBuilders.get("/books");
    ResultActions action = mvc.perform(builder);

    StatusResultMatchers status = MockMvcResultMatchers.status();
    ResultMatcher ok = status.isOk();
    action.andExpect(ok);

    HeaderResultMatchers header = MockMvcResultMatchers.header();
    ResultMatcher contentType = header.string("Content-Type", "application/json");
    action.andExpect(contentType);

    ContentResultMatchers content = MockMvcResultMatchers.content();
    ResultMatcher result = content.json("{\"id\":1,\"name\":\"springboot\",\"type\":\"springboot\"}");
    action.andExpect(result);
}
```

### --数据层测试事务回滚

问题在于：我们需要对表现层、业务层、数据层接口进行测试撒，但在测试的过程中，会对数据库里面的数据进行更改，例如插入，删除操作等等，这就有问题了，因为我测试用的都是垃圾数据，不希望他在数据库留下痕迹啊，解决办法就是将当前测试用例的数据库事务进行回滚即可。

springboot给出了最简单的一个方案：注解@Transactional。

当程序运行后，只要注解@Transactional和注解@SpringBootTest同时存在，springboot就会认为这是一个测试程序，无需提交事务，所以也就可以避免事务的提交。

```java
@SpringBootTest
@Transactional
public class DaoTest {
    @Autowired
    private BookService bookService;

    @Test
    void testSave(){
        Book book = new Book();
        book.setName("springboot3");
        book.setType("springboot3");
        book.setDescription("springboot3");

        bookService.save(book);
    }
}
```

如果想要在测试用例里面提交事务，再加上一个注解@RollBack(false) 即可。

<img src="images/image-20220419183449796.png" alt="image-20220419183449796" style="zoom:80%;" />

### --产生随机的测试数据

之前我们测试的数据都是人工写好的，其实是不合理的，因为你是测试程序是否有bug，一直用一个值就失去了测试的意义了。所以springboot提供了在测试配置中使用随机值的机制，确保每次运行程序加载的数据都是随机的。具体如下：

```yaml
testcase:
  book:
    id: ${random.int}
    id2: ${random.int(10)}
    type: ${random.int!5,10!}
    name: ${random.value}
    uuid: ${random.uuid}
    publishTime: ${random.long}
```

​		当前配置就可以在每次运行程序时创建一组随机数据，避免每次运行时数据都是固定值的尴尬现象发生，有助于测试功能的进行。数据的加载按照之前加载数据的形式，使用@ConfigurationProperties注解即可

```JAVA
@Component
@Data
@ConfigurationProperties(prefix = "testcase.book")
public class BookCase {
    private int id;
    private int id2;
    private int type;
    private String name;
    private String uuid;
    private long publishTime;
}
```

```java
@SpringBootTest
@Transactional
public class DaoTest {
    @Autowired
    private BookCase bookCase;

    @Test
    void testSave(){
        bookService.save(bookCase);
    }
}
```

对于随机值的产生，还有一些小的限定规则，比如产生的数值性数据可以设置范围等，具体如下：

<img src="images/image-20220223135454862.png" alt="image-20220223135454862" style="zoom:80%;" />

- ${random.int}表示随机整数
- ${random.int(10)}表示10以内的随机数
- ${random.int(10,20)}表示10到20的随机数
- 其中()可以是任意字符，例如[]，!!均可

**总结**

1. 使用随机数据可以替换测试用例中书写的固定数据，提高测试用例中的测试数据有效性

## 4. 数据层解决方案

开发实用篇前三章基本上是开胃菜，从第四章开始，开发实用篇进入到了噩梦难度了，从这里开始，不再是单纯的在springboot内部搞事情了，要涉及到很多相关知识。本章节主要内容都是和数据存储与读取相关，前期学习的知识与数据层有关的技术基本上都围绕在数据库这个层面上，所以本章要讲的第一个大的分支就是**SQL解决方案**相关的内容，除此之外，数据的来源还可以是非SQL技术相关的数据操作，因此第二部分围绕着**NOSQL解决方案**讲解。至于什么是NOSQL解决方案，讲到了再说吧。下面就从SQL解决方案说起。

### 4.1 SQL解决方案

这就是我们之前一直在学习的东西，目前数据层解决方案技术选型就是Mysql+Druid+MyBatisPlus三个技术，

```xml
<!---->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <scope>runtime</scope>
</dependency>
<!---->
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.4.2</version>
</dependency>
<!---->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid-spring-boot-starter</artifactId>
    <version>1.2.8</version>
</dependency>
```

而三个技术分别对应了数据层操作的三个层面：

- 数据源技术：Druid
- 持久化技术：MyBatisPlus
- 数据库技术：MySQL

而每一个技术部分其实都有多种选择，下面我们一个个介绍。

#### --数据源技术

目前我们使用的数据源技术是Druid，starter坐标如上，对应的配置信息如下：

```yml
spring:
  datasource:
    druid:
      driver-class-name: com.mysql.cj.jdbc.Driver
      url: jdbc:mysql://localhost:3306/studysql?useSSL=false&serverTimezone=UTC
      username: root
      password: 5240zhouquan
```

启动服务器运行时可以在日志中看到对应的数据源初始化信息，具体如下：

<img src="images/image-20220419191550020.png" alt="image-20220419191550020" style="zoom:80%;" />

那当我们不使用这个数据源时，会是什么情况呢，其实之前讲过一点点，springboot是内嵌了默认的数据源技术的，就和内嵌tomcat一样。

首先移除我们的Druid数据源，需要两步：一是修改配置文件，二是去掉他的starter。我们平常这样直接写的数据源连接信息就是针对内置数据源的，

```java
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/studysql?useSSL=false&serverTimezone=UTC
    username: root
    password: 5240zhouquan
```

**SpringBoot提供了3种内嵌的数据源对象供开发者选择：**

- HikariCP：默认内置数据源对象
- Tomcat提供DataSource：HikariCP不可用的情况下，且在web环境中，将使用tomcat服务器配置的数据源对象

- Commons DBCP：Hikari不可用，tomcat数据源也不可用，将使用dbcp数据源

因为通用配置无法设置具体的数据源配置信息，仅提供基本的连接相关配置，如需个性化配置，在下一级配置中设置具体设定：

```java
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/studysql?useSSL=false&serverTimezone=UTC
    username: root
    password: 5240zhouquan
    hikari:
				maximum-pool-size: 50
```

**springboot在数据源配置上的简化**

因为不同的数据库，他们的url的写法完全不一样，所以sp可以监测到，避免了我们驱动类写错的情况。

<img src="images/image-20220420164825722.png" alt="image-20220420164825722" style="zoom:80%;" />

#### --持久化技术

说完数据源解决方案，再来说一下持久化解决方案。springboot充分发挥其最强辅助的特征，给开发者提供了一套现成的数据层技术，叫做JdbcTemplate。其实这个技术不能说是springboot提供的，因为不使用springboot技术，一样能使用它，谁提供的呢？spring技术提供的，所以在springboot技术范畴中，这个技术也是存在的，毕竟springboot技术是加速spring程序开发而创建的。

​		这个技术其实就是回归到jdbc最原始的编程形式来进行数据层的开发，下面直接上操作步骤：

**步骤①**：导入jdbc对应的坐标，记得是starter

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency
```

**步骤②**：自动装配JdbcTemplate对象

```java
@SpringBootTest
class Springboot15SqlApplicationTests {
    @Test
    void testJdbcTemplate(@Autowired JdbcTemplate jdbcTemplate){
    }
}
```

**步骤③**：使用JdbcTemplate实现查询操作（非实体类封装数据的查询操作）

```java
@Test
void testJdbcTemplate(@Autowired JdbcTemplate jdbcTemplate){
    String sql = "select * from tbl_book";
    List<Map<String, Object>> maps = jdbcTemplate.queryForList(sql);
    System.out.println(maps);
}
```

**步骤④**：使用JdbcTemplate实现查询操作（实体类封装数据的查询操作）

```java
@Test
void testJdbcTemplate(@Autowired JdbcTemplate jdbcTemplate){

    String sql = "select * from tbl_book";
    RowMapper<Book> rm = new RowMapper<Book>() {
        @Override
        public Book mapRow(ResultSet rs, int rowNum) throws SQLException {
            Book temp = new Book();
            temp.setId(rs.getInt("id"));
            temp.setName(rs.getString("name"));
            temp.setType(rs.getString("type"));
            temp.setDescription(rs.getString("description"));
            return temp;
        }
    };
    List<Book> list = jdbcTemplate.query(sql, rm);
    System.out.println(list);
}
```

**步骤⑤**：使用JdbcTemplate实现增删改操作

```java
@Test
void testJdbcTemplateSave(@Autowired JdbcTemplate jdbcTemplate){
    String sql = "insert into tbl_book values(3,'springboot1','springboot2','springboot3')";
    jdbcTemplate.update(sql);
}
```

​		如果想对JdbcTemplate对象进行相关配置，可以在yml文件中进行设定，具体如下：

```yaml
spring:
  jdbc:
    template:
      query-timeout: -1   # 查询超时时间
      max-rows: 500       # 最大行数
      fetch-size: -1      # 缓存行数
```

**总结**

1. SpringBoot内置JdbcTemplate持久化解决方案
2. 使用JdbcTemplate需要导入spring-boot-starter-jdbc的坐标

#### --数据库技术

截止到目前，springboot给开发者提供了**内置的数据源解决方案和持久化解决方案**，在数据层解决方案三件套中还剩下一个数据库，莫非springboot也提供有内置的解决方案？还真有，还不是一个，三个，这一节就来说说内置的数据库解决方案。

springboot提供了3款**内置的数据库**，分别是

- H2
- HSQL
- Derby

可以像是tomcat服务器一样，采用内嵌的形式运行在spirngboot容器中。内嵌在容器中运行，那必须是java对象啊，对，这三款数据库底层都是使用java语言开发的。

**使用内嵌数据库的优点：**我们一直使用MySQL数据库就挺好的，为什么有需求用这个呢？原因就在于这三个数据库都可以采用内嵌容器的形式运行，在应用程序运行后，如果我们进行测试工作，此时测试的数据无需存储在磁盘上，但是又要测试使用，内嵌数据库就方便了，运行在内存中，该测试测试，该运行运行，等服务器关闭后，一切烟消云散，多好，省得你维护外部数据库了。这也是内嵌数据库的最大优点，方便进行功能测试。

下面以H2数据库为例讲解如何使用这些内嵌数据库，操作步骤也非常简单，简单才好用嘛

**步骤①**：导入H2数据库对应的坐标，一共2个

```xml
<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```

**步骤②**：将工程设置为web工程，启动工程时启动H2数据库

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

**步骤③**：配置开启H2数据库控制台访问程序，开启类似Navicat的可视化数据库工具，只是在网页上

```yaml
# 先不要配置数据源的相关信息
server:
  port: 80
  
spring:
  h2:
    console:
      enabled: true
      path: /h2
```

启动Web服务，在控制台我们可以看到Hibernate数据库启动了，打开网页访问localhost/h2即可看到可视化信息如下：

​		<img src="images/image-20220420163316434.png" alt="image-20220420163316434" style="zoom:80%;" />

密码可以自己设置，我们设置123456。现在点击连接会显示，因为第一次他配置信息未初始化，并没有创建一个名字为test的数据库，该数据库会放在c盘user目录下的。![image-20220420163329968](images/image-20220420163329968.png)

我们需要先配置下列数据源信息，就把网页上面的复制下来，一模一样，启动程序运行后再次访问/h2路径就可以正常访问了，==下次启动就不需要在配置这些datasource的信息==

```yaml
server:
  port: 80

spring:
  h2:
    console:
      enabled: true
      path: /h2
  datasource:
    driver-class-name: org.h2.Driver
    url: jdbc:h2:~/test
    username: sa
    password: 123456
```

<img src="images/image-20220420163726315.png" alt="image-20220420163726315" style="zoom: 50%;" />

该数据库的创建表的sql语句：

​		`create table tbl_book (id int,name varchar,type varchar,description varchar)`

**步骤④**：使用JdbcTemplate或MyBatisPlus技术操作数据库，后面操作就没有区别了，我们只是换了一个数据库而已，其他的东西都不受影响。

==一个重要提醒==，别忘了，上线时，把内存级数据库关闭，采用MySQL数据库作为数据持久化方案，关闭方式就是设置enabled属性为false即可。到后面你会发现hiberate数据库的速度比我们MySQL快多了，毕竟是内存级的撒。

#### --总结

<img src="images/image-20220420165016622.png" alt="image-20220420165016622" style="zoom:80%;" />

### 4.2 NoSQL解决方案

NoSQL意思就是非关系型数据库解决方案，简单说就是我们数据不再放在关系型数据库里面了，但数据的存取依然不变该怎么样怎么样，但这些数据放哪里呢？那肯定是一些能够存储数据的其他相关技术中了，比如Redis等。在springboot官方文档中提供了10种相关技术的整合方案，我们将讲解国内市场上最流行的几款NoSQL数据库整合方案，分别是**Redis、MongoDB、ES**。

==加速数据读写：== Redis可以使用内存加载数据并实现数据快速访问，MongoDB可以在内存中存储类似对象的数据并实现数据的快速访问

==加速数据查询：==ES

#### 4.2.1Redis

Redis是一款**key-value**存储结构的**内存级NoSQL**数据库。支持多种数据存储格式

- 支持持久化
- 支持集群

Redis重点关注数据存储格式，value可以存多种类型的数据格式，虽然说Redis有它的数据持久化方案，分别是RDB和AOF，但是Redis自身并不是为了数据持久化而生的，主要是**在内存中保存数据，加速数据访问的**，所以说是一款内存级数据库。

##### 安装

​		windows版安装包下载地址：https://github.com/tporadowski/redis/releases

​		下载的安装包有两种形式，一种是一键安装的msi文件，还有一种是解压缩就能使用的zip文件，哪种形式都行，我们用zip。

打开Redis的安装目录，我们只要关心两个主要的文件，一个是.conf的，一个是.exe的。

<img src="images/image-20220502152146450.png" alt="image-20220502152146450" style="zoom:80%;" />

启动服务的命令：

- 管理员运行cmd命令，进入到安装的文件夹目录，执行 redis-server.exe redis.windows.conf 启动服务
- 如果出现1688错误则依次执行 redis-cli.exe	shutdown	exit然后再执行启动命令即可。

将redis添加到windows服务列表中：

- 管理员运行cmd，进入到安装文件夹目录，执行   redis-server.exe --service-install redis.windows.conf --loglevel verbose 就会发现已经添加成功了。

##### 基本操作

服务器启动后，使用客户端就可以连接服务器，类似于启动完MySQL数据库，然后启动SQL命令行操作数据库。		

存储一个字符串数据到redis中

```CMD
set "name" "itheima"
set "age" "12"
```

从redis中取出已经放入的数据，根据键取，就可以得到对应数据。

```CMD
get "name"
get "age"
```

以上使用的数据存储是一个名称对应一个值，如果要维护的数据过多，可以使用别的数据存储结构。例如hash，它是一种一个名称下可以存储多个数据的存储模型，并且每个数据也可以有自己的二级存储名称。向hash结构中存储数据格式如下：

```
hset a a1 aa1		#对外key名称是a，在名称为a的存储模型中，a1这个key中保存了数据aa1
hset a a2 aa2
```

获取hash结构中的数据命令如下

```CMD
hget a a1			#得到aa1
hget a a2			#得到aa2
```

有关redis的基础操作就普及到这里，需要全面掌握redis技术，请参看相关教程学习。

##### 整合

​		在进行整合之前先梳理一下整合的思想，springboot整合任何技术其实就是在springboot中使用对应技术的API。如果两个技术没有交集，就不存在整合的概念了。所谓整合其实就是使用springboot技术去管理其他技术，几个问题是躲不掉的。

- 第一，需要先导入对应技术的坐标，而整合之后，这些坐标都有了一些变化

- 第二，任何技术通常都会有一些相关的设置信息，整合之后，这些信息如何写，写在哪

- 第三，没有整合之前操作如果是模式A的话，整合之后如果没有给开发者带来一些便捷操作，那整合将毫无意义，所以整合后操作肯定要简化一些，那对应的操作方式自然也有所不同


这springboot整合所有技术是一种通用思想。

下面就开始springboot整合redis，操作步骤如下：

**步骤①**：导入springboot整合redis的starter坐标

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

​		上述坐标可以在创建模块的时候通过勾选的形式进行选择，归属NoSQL分类中

<img src="images/image-20220224101142220.png" alt="image-20220224101142220" style="zoom:50%;" />

**步骤②**：进行服务器基础配置

```yaml
spring:
  redis:
    host: localhost
    port: 6379
```

​	操作redis，最基本的信息就是操作哪一台redis服务器，所以服务器地址属于基础配置信息，不可缺少。但是即便你不配置，目前也是可以用的。因为以上两组信息都有默认配置，刚好就是上述配置值。

**步骤③**：**Redis的客户端**

使用springboot整合redis的专用客户端接口操作，此处使用的是**RedisTemplate。**

我们点进去看一下这个类，发现它支持泛型，就是说我们的键值对两个的类型，一般我们不指定，就是object类。

<img src="images/image-20220502162232335.png" alt="image-20220502162232335" style="zoom:80%;" />

```java
@SpringBootTest
class Springboot16RedisApplicationTests {
    @Autowired
    private RedisTemplate redisTemplate;
    @Test
    void set() {
        ValueOperations ops = redisTemplate.opsForValue();
        ops.set("age",41);
    }
    @Test
    void get() {
        ValueOperations ops = redisTemplate.opsForValue();
        Object age = ops.get("name");
        System.out.println(age);
    }
    @Test
    void hset() {
        HashOperations ops = redisTemplate.opsForHash();
        ops.put("info","b","bb");
    }
    @Test
    void hget() {
        HashOperations ops = redisTemplate.opsForHash();
        Object val = ops.get("info", "b");
        System.out.println(val);
    }
}

```

​		在操作redis时，需要先确认操作何种数据，根据数据种类得到操作接口。例如使用opsForValue()获取string类型的数据操作接口，使用opsForHash()获取hash类型的数据操作接口，剩下的就是调用对应api操作了。各种类型的数据操作接口如下：

<img src="images/image-20220224103104908.png" alt="image-20220224103104908" style="zoom:80%;" />

**StringRedisTemplate**

使用前面的客户端会存在一个问题，就是我们Redis内部只支持键值对两个类型均为字符串，==因此当操作的数据以对象的形式存在时，会进行转码，转换成字符串格式后进行操作。所以说在java里面存的东西，在Redis里面一般找不到。

为了方便开发者使用基于字符串为数据的操作，springboot整合redis时提供了专用的API接口StringRedisTemplate，你可以理解为这是**RedisTemplate的一种指定数据泛型为String的操作API**。

```java
@SpringBootTest
public class StringRedisTemplateTest {
    @Autowired
    private StringRedisTemplate stringRedisTemplate;
  
    @Test
    void get(){
        ValueOperations<String, String> ops = stringRedisTemplate.opsForValue();
        String name = ops.get("name");
        System.out.println(name);
    }
}
```

##### jedis客户端

jedis是传统的一个操作Redis的客户端，我们springboot也整合了，可以使用，只是说默认提供的是lettucs客户端技术。

<img src="images/image-20220502163258360.png" alt="image-20220502163258360" style="zoom:80%;" />

**步骤①**：导入jedis坐标

```xml
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
</dependency>
```

​		jedis坐标受springboot管理，无需提供版本号

**步骤②**：配置客户端技术类型，设置为jedis

```yaml
spring:
  redis:
    host: localhost
    port: 6379
    client-type: jedis
```

**步骤③**：根据需要设置对应的配置

```yaml
spring:
  redis:
    host: localhost
    port: 6379
    client-type: jedis
    lettuce:
      pool:
        max-active: 16
    jedis:
      pool:
        max-active: 16
```

**lettcus与jedis区别**

- jedis连接Redis服务器是直连模式，当多线程模式下使用jedis会存在线程安全问题，解决方案可以通过配置连接池使每个连接专用，这样整体性能就大受影响
- lettcus基于Netty框架进行与Redis服务器连接，底层设计中采用StatefulRedisConnection。 StatefulRedisConnection自身是线程安全的，可以保障并发访问安全问题，所以一个连接可以被多线程复用。当然lettcus也支持多连接实例一起工作

#### 4.2.2 MongoDB

==Redis可以使用内存加载数据并实现数据快速访问，MongoDB可以在内存中存储类似对象的数据并实现数据的快速访问，ES技术加速数据的查询==

Redis的缺点在于：响应速度虽然很快，但不能使用结构化的数据。现在需要一个两者都具备的技术——MongoDB。

MongoDB是一个开源、高性能、**无模式**的**文档型**数据库，它是NoSQL数据库产品中的一种，是最像关系型数据库的非关系型数据库。

什么叫无模式呢？简单说就是作为一款数据库，没有固定的数据存储结构，第一条数据可能有A、B、C一共3个字段，第二条数据可能有D、E、F也是3个字段，第三条数据可能是A、C、E3个字段，也就是说数据的结构不固定，这就是无模式。

已经了解了这几种数据库，我们先看一下各类型数据库的使用场景：

<img src="images/image-20220502170058836.png" alt="image-20220502170058836" style="zoom:80%;" />

##### 安装

​		windows版安装包下载地址：https://www.mongodb.com/try/download

​		下载的安装包也有两种形式，一种是一键安装的msi文件，还有一种是解压缩就能使用的zip文件，哪种形式都行，本课程采用解压缩zip文件进行安装。

​		解压缩完毕后会得到如下文件，其中bin目录包含了所有mongodb的所有可执行命令

<img src="images/image-20220503111058845.png" alt="image-20220503111058845" style="zoom:80%;" />

​		mongodb在运行时需要指定一个数据存储的目录，所以创建一个数据存储目录，此处创建data的目录用来存储数据，然后data下创建一个db用来放数据库信息，具体如下：

<img src="images/image-20220503111215555.png" alt="image-20220503111215555" style="zoom:80%;" />

如果在安装的过程中出现了如下警告信息，就是告诉你，你当前的操作系统缺少了一些系统文件，这个不用担心。

<img src="images/image-20220503111245584.png" alt="image-20220503111245584" style="zoom:67%;" />

根据下列方案即可解决，在浏览器中搜索提示缺少的名称对应的文件，并下载，将下载的文件拷贝到windows安装目录的system32目录下，然后在命令行中执行regsvr32命令注册此文件。根据下载的文件名不同，执行命令前更改对应名称。               

```
regsvr32 vcruntime140_1.dll
```

**启动服务器**

使用bin文件夹下的那几个可执行命令，先cmd进入。

```CMD
mongod --dbpath=..\data\db
```

启动服务器时需要指定数据存储位置，通过参数--dbpath进行设置。默认服务端口27017。

**启动客户端**

再重新cmd一下，然后启动客户端。连接成功

```CMD
mongo --host=127.0.0.1 --port=27017
```

##### 基本操作

MongoDB虽然是一款数据库，但是它的操作并不是使用SQL语句进行的，因此操作方式各位小伙伴可能比较陌生，好在有一些类似于Navicat的数据库客户端软件，能够便捷的操作MongoDB，先安装一个客户端，再来操作MongoDB。

同类型的软件较多，本次安装的软件时Robo3t，Robot3t是一款绿色软件，无需安装，解压缩即可。解压缩完毕后进入安装目录双击robot3t.exe即可使用。

<img src="images/image-20220224114911573.png" alt="image-20220224114911573" style="zoom: 33%;" />

​		打开软件首先要连接MongoDB服务器，选择【File】菜单，选择【Connect...】

![image-20220224115202422](images/image-20220224115202422.png)

​		进入连接管理界面后，选择左上角的【Create】链接，创建新的连接设置

<img src="images/image-20220224115254200.png" alt="image-20220224115254200" style="zoom:80%;" />

​		如果输入设置值即可连接（默认不修改即可连接本机27017端口），进行Test。

![image-20220224115300266](images/image-20220224115300266.png)

**创建数据库**

<img src="images/image-20220503114021185.png" alt="image-20220503114021185" style="zoom:80%;" />

**创建集合**

在Collections上使用右键创建，输入集合名称即可，集合等同于数据库中的表的作用

<img src="images/image-20220503114054757.png" alt="image-20220503114054757" style="zoom:67%;" />

**增删改文档：**

文档是一种类似json格式的数据，初学者可以先把数据理解为就是json数据）	

<img src="images/image-20220503114203510.png" alt="image-20220503114203510" style="zoom:80%;" />

**查询文档：**

<img src="images/image-20220503114237886.png" alt="image-20220503114237886" style="zoom:80%;" />

​		新增文档：	

```CMD
db.集合名称.insert/save/insertOne(文档)
```

​		删除文档：

```CMD
db.集合名称.remove(条件)
```

​		修改文档：

```cmd
db.集合名称.update(条件，{操作种类:{文档}})
```

​		查询文档：

```CMD
基础查询
查询全部：		   db.集合.find();
查第一条：		   db.集合.findOne()
查询指定数量文档：	db.集合.find().limit(10)					//查10条文档
跳过指定数量文档：	db.集合.find().skip(20)					//跳过20条文档
统计：			  	db.集合.count()
排序：				db.集合.sort({age:1})						//按age升序排序
投影：				db.集合名称.find(条件,{name:1,age:1})		 //仅保留name与age域

条件查询
基本格式：			db.集合.find({条件})
模糊查询：			db.集合.find({域名:/正则表达式/})		  //等同SQL中的like，比like强大，可以执行正则所有规则
条件比较运算：		   db.集合.find({域名:{$gt:值}})				//等同SQL中的数值比较操作，例如：name>18
包含查询：			db.集合.find({域名:{$in:[值1，值2]}})		//等同于SQL中的in
条件连接查询：		   db.集合.find({$and:[{条件1},{条件2}]})	   //等同于SQL中的and、or
```

有关MongoDB的基础操作就普及到这里，需要全面掌握MongoDB技术，请参看相关教程学习。



##### 整合

和整合Redis一样的，三步走。

**步骤①**：导入springboot整合MongoDB的starter坐标

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-mongodb</artifactId>
</dependency>
```

上述坐标也可以在创建模块的时候通过勾选的形式进行选择，同样归属NoSQL分类中。

<img src="images/image-20220224120721626.png" alt="image-20220224120721626" style="zoom: 67%;" />

**步骤②**：进行基础配置。

```yaml
spring:
  data:
    mongodb:
      uri: mongodb://localhost/itheima
```

操作MongoDB需要的配置与操作redis一样，最基本的信息都是操作哪一台服务器，区别就是连接的服务器IP地址和端口不同，书写格式不同而已。

**步骤③**：使用springboot整合MongoDB的专用**客户端接口MongoTemplate**来进行操作

```java
@SpringBootTest
class Springboot17MongodbApplicationTests {
    @Autowired
    private MongoTemplate mongoTemplate;
    @Test
    void contextLoads() {
        Book book = new Book();
        book.setId(2);
        book.setName("springboot2");
        book.setType("springboot2");
        book.setDescription("springboot2");
        mongoTemplate.save(book);
    }
    @Test
    void find(){
        List<Book> all = mongoTemplate.findAll(Book.class);
        System.out.println(all);
    }
}
```

​		整合工作到这里就做完了，感觉既熟悉也陌生。熟悉的是这个套路，三板斧，就这三招，导坐标做配置用API操作，陌生的是这个技术，里面具体的操作API可能会不熟悉，有关springboot整合MongoDB我们就讲到这里。有兴趣可以继续学习MongoDB的操作，然后再来这里通过编程的形式操作MongoDB。

#### 4.2.3 ES

==Redis可以使用内存加载数据并实现数据快速访问，MongoDB可以在内存中存储类似对象的数据并实现数据的快速访问，ES技术加速数据的查询==

ES（Elasticsearch）是一个分布式全文搜索引擎，重点是全文搜索。

那什么是全文搜索呢？比如用户要买一本书，以Java为关键字进行搜索，不管是书名中还是书的介绍中，甚至是书的作者名字，只要包含java就作为查询结果返回给用户查看，上述过程就使用了全文搜索技术。搜索的条件不再是仅用于对某一个字段进行比对，而是在一条数据中使用搜索条件去比对更多的字段，只要能匹配上就列入查询结果，这就是全文搜索的目的。

要实现全文搜索的效果，不可能使用数据库中like操作去进行比对，这种效率太低了。ES设计了一种全新的思想——**倒排索引**，来实现全文搜索。具体操作过程如下：

1. 将数据库数据全部文本信息进行分词

   - 例如“中华人民共和国”就会被拆分成三个词，分别是“中华”、“人民”、“共和国”，此过程有专业术语叫做分词。分词的策略不同，分出的效果不一样，不同的分词策略称为分词器。

2. 将分词得到的结果存储起来，对应每条数据的id

   - 例如id为1的数据中名称这一项的值是“中华人民共和国”，那么分词结束后，就会出现“中华”对应id为1，“人民”对应id为1，“共和国”对应id为1

   - 例如id为2的数据中名称这一项的值是“人民代表大会“，那么分词结束后，就会出现“人民”对应id为2，“代表”对应id为2，“大会”对应id为2

   - 此时就会出现如下对应结果，按照上述形式可以对所有文档进行分词。需要注意分词的过程不是仅对一个字段进行，而是对每一个参与查询的字段都执行，最终结果汇总到一个表格中

     | 分词结果关键字 | 对应id |
     | -------------- | ------ |
     | 中华           | 1      |
     | 人民           | 1,2    |
     | 共和国         | 1      |
     | 代表           | 2      |
     | 大会           | 2      |

3. 当进行查询时，如果输入“人民”作为查询条件，可以通过上述表格数据进行比对，得到id值1,2，然后根据id值就可以得到查询的结果数据了。

上述过程中分词结果关键字内容每一个都不相同，作用有点类似于数据库中的索引，是用来加速数据查询的。但是数据库中的索引是对某一个字段而言，而这里的分词结果关键字不是一个完整的字段值，只是一个字段中的其中的一部分内容。之前是根据索引查找整条数据，但这里全文搜索中的分词结果关键字查询后得到的并不是整条的数据，而是数据的id，要想获得具体数据还要再次查询，因此这里为这种分词结果关键字起了一个全新的名称，叫做**倒排索引**。

通过上述内容的学习，发现使用ES其实准备工作还是挺多的，必须先建立文档的倒排索引，然后才能继续使用。快速了解一下ES的工作原理，下面直接开始我们的学习，老规矩，先安装，再操作，最后说整合。

##### 安装

​		windows版安装包下载地址：[https://](https://www.elastic.co/cn/downloads/elasticsearch)[www.elastic.co/cn/downloads/elasticsearch](https://www.elastic.co/cn/downloads/elasticsearch)

​		下载的安装包是解压缩就能使用的zip文件，解压缩完毕后会得到如下文件

![image-20220225132756400](images/image-20220225132756400.png)

- bin目录：包含所有的可执行命令
- config目录：包含ES服务器使用的配置文件
- jdk目录：此目录中包含了一个完整的jdk工具包，版本17，当ES升级时，使用最新版本的jdk确保不会出现版本支持性不足的问题
- lib目录：包含ES运行的依赖jar文件
- logs目录：包含ES运行后产生的所有日志文件
- modules目录：包含ES软件中所有的功能模块，也是一个一个的jar包。和jar目录不同，jar目录是ES运行期间依赖的jar包，modules是ES软件自己的功能jar包
- plugins目录：包含ES软件安装的插件，默认为空

##### **启动服务器**

双击即可启动ES服务器，默认服务端口9200。通过浏览器访问http://localhost:9200看到如下信息视为ES服务器正常启动。

<img src="images/image-20220503152001828.png" alt="image-20220503152001828" style="zoom:67%;" />

<img src="images/image-20220503153023953.png" alt="image-20220503153023953" style="zoom:67%;" />

可能出现的问题：

1. 开启了ssl认证。

   ![image-20220503152909902](images/image-20220503152909902.png)

解决：在  ES/config/[elasticsearch](https://so.csdn.net/so/search?q=elasticsearch&spm=1001.2101.3001.7020).yml  文件中把`xpack.security.http.ssl:enabled`设置成`false`即可

2. 开启了密码验证模式

解决：在  ES/config/[elasticsearch](https://so.csdn.net/so/search?q=elasticsearch&spm=1001.2101.3001.7020).yml  文件中把`xpack.security.enabled`属性设置为`false`

##### 基本操作

ES中保存有我们要查询的数据，只不过格式和数据库存储数据格式不同而已。在ES中我们要先创建倒排索引，这个索引的功能又点类似于数据库的表，然后将数据添加到倒排索引中，添加的数据称为文档。所以要进行ES的操作要先创建索引，再添加文档，这样才能进行后续的查询操作。

==操作ES是通过Rest风格的请求来进行，也就是说发送一个请求就可以执行一个操作==

我们采用APIPost来进行，首先启动开ES的服务器先，然后打开ApiPost。

**第一步：创建索引。**

```cmd
PUT请求		http://localhost:9200/books
```

发送请求后，看到如下信息即索引创建成功.

<img src="images/image-20220504142911399.png" alt="image-20220504142911399" style="zoom:80%;" />

注意索引只能创建一个哦，是不能重复的。假如再次发送这个PUT就会报错。

- 查询索引

  ```CMD
  GET请求		http://localhost:9200/books
  ```

  查询索引得到索引相关信息，如下

  <img src="images/image-20220504143132728.png" alt="image-20220504143132728" style="zoom:80%;" />

  如果查询了不存在的索引，会返回错误信息，status：404

- 删除索引

  ```CMD
  DELETE请求	http://localhost:9200/books
  ```

  删除所有后，给出删除结果，重复删除就会报错

  <img src="images/image-20220504143210882.png" alt="image-20220504143210882" style="zoom:80%;" />

**第一步二：创建索引，同时指定分词器**

前面我们创建的索引是没有指定分词器的，他就不会自动分词，所以我们需要在创建的时候添加请求参数来设置分词器。目前国内较为流行的分词器是IK分词器，使用前先在下对应的分词器，然后使用。IK分词器下载地址：https://github.com/medcl/elasticsearch-analysis-ik/releases

分词器下载后解压到ES安装目录的plugins目录中即可，安装分词器后需要重新启动ES服务器。

<img src="images/image-20220504143805915.png" alt="image-20220504143805915" style="zoom:80%;" />

可能遇到的问题：ES服务器启动失败了，如下：

<img src="images/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0ZIbGFuZw==,size_16,color_FFFFFF,t_70.png" style="zoom: 50%;" />

原因在于IK的配置文件里面，打开他的配置文件

<img src="images/image-20220504144424396.png" alt="image-20220504144424396" style="zoom:80%;" />

我们会发现，es的版本和我安装的版本不一样的，因为我安装的IK是8.1.2的，它并不知道我本地安装的ES会是什么版本的，所以它默认的ES版本就是8.1.2，可惜我的ES是8.1.3版本的，改一下就好了

<img src="images/image-20220504144503829.png" alt="image-20220504144503829" style="zoom:67%;" />

```json
PUT请求		http://localhost:9200/books

请求体参数如下（注意是json格式的参数）

{
    "mappings":{							#定义mappings属性，替换创建索引时对应的mappings属性	
        "properties":{						#定义索引中包含的属性设置
            "id":{							#设置索引中包含id属性
                "type":"keyword"			#当前属性可以被直接搜索
            },
            "name":{						#设置索引中包含name属性
                "type":"text",              #当前属性是文本信息，参与分词  
                "analyzer":"ik_max_word",   #使用IK分词器进行分词             
                "copy_to":"all"				#分词结果拷贝到all属性中
            },
            "type":{
                "type":"keyword"
            },
            "description":{
                "type":"text",	                
                "analyzer":"ik_max_word",                
                "copy_to":"all"
            },
            "all":{				#定义属性，用来描述多个字段的分词结果集合，当前属性可以参与查询
                "type":"text",	                
                "analyzer":"ik_max_word"
            }
        }
    }
}
```

创建完毕后返回结果和不使用分词器创建索引的结果是一样的，此时可以通过查看索引信息观察到添加的请求参数mappings已经进入到了索引属性中，

<img src="images/image-20220504153803055.png" alt="image-20220504153803055" style="zoom:80%;" />

目前我们已经有了索引了，但是索引中还没有数据，所以要先添加数据，==ES中称数据为文档==，下面进行文档操作。

**第二步：添加文档**

- 添加文档，有三种方式

  ```json
  POST请求	http://localhost:9200/books/_doc		#使用系统生成id
  POST请求	http://localhost:9200/books/_create/1	#使用指定id
  POST请求	http://localhost:9200/books/_doc/1		#使用指定id，不存在创建，存在更新（版本递增）
  
  文档通过请求参数传递，数据格式json
  {
      "name":"springboot",
      "type":"springboot",
      "description":"springboot"
  }  
  ```

- 查询文档

  ```json
  GET请求	http://localhost:9200/books/_doc/1		 #查询单个文档 		
  GET请求	http://localhost:9200/books/_search		 #查询全部文档
  ```

- 条件查询

  ```json
  GET请求	http://localhost:9200/books/_search?q=name:springboot	# q=查询属性名:查询属性值
  ```

- 删除文档

  ```json
  DELETE请求	http://localhost:9200/books/_doc/1
  ```

- 修改文档（全量更新）

  ```json
  PUT请求	http://localhost:9200/books/_doc/1
  
  文档通过请求参数传递，数据格式json
  {
      "name":"springboot",
      "type":"springboot",
      "description":"springboot"
  }
  ```

- 修改文档（部分更新）

  ```json
  POST请求	http://localhost:9200/books/_update/1
  
  文档通过请求参数传递，数据格式json
  {			
      "doc":{
          "name":"springboot"		#仅更新提供的属性值，未提供的属性值不参与更新操作
      }
  }
  ```

##### 整合

整合前，我们先建立了一个简单的工程环境，如下：

<img src="images/image-20220504164748427.png" alt="image-20220504164748427" style="zoom:67%;" />

用到mysql、druid、mybatisplus、lombok四个技术，做对MySQL数据的简单操作。

<img src="images/image-20220504164933594.png" alt="image-20220504164933594" style="zoom:80%;" />

下面就开始springboot整合ES，操作步骤如下：

**步骤①**：导入springboot整合ES的starter坐标

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-elasticsearch</artifactId>
</dependency>
```

**步骤②**：进行基础配置

```yaml
spring:
  elasticsearch:
    uris: http://localhost:9200
```

​				配置ES服务器地址，端口9200

**步骤③**：使用springboot整合ES的专用客户端接口**RestHighLevelClient**来进行操作

```java
@SpringBootTest
class DemoEsApplicationTests {

    @Autowired
    private RestHighLevelClient client;
    
    @Test
    void testCreateIndex() throws IOException {
//        创建索引
        CreateIndexRequest request = new CreateIndexRequest("book");
        client.indices().create(request, RequestOptions.DEFAULT);
        
    }
}
```

然后我们用Apipost查询一下，看有没有成功。

<img src="images/image-20220504203709168.png" alt="image-20220504203709168" style="zoom:67%;" />

**创建索引（IK分词器）**：先把前面那个没有设置分词器的books索引删除

```java
@Test
// 创建索引
void testCreateIndexByIK() throws IOException {
    CreateIndexRequest request = new CreateIndexRequest("books");
    String json = "{\n" +
            "    \"mappings\":{\n" +
            "        \"properties\":{\n" +
            "            \"id\":{\n" +
            "                \"type\":\"keyword\"\n" +
            "            },\n" +
            "            \"name\":{\n" +
            "                \"type\":\"text\",\n" +
            "                \"analyzer\":\"ik_max_word\",\n" +
            "                \"copy_to\":\"all\"\n" +
            "            },\n" +
            "            \"type\":{\n" +
            "                \"type\":\"keyword\"\n" +
            "            },\n" +
            "            \"description\":{\n" +
            "                \"type\":\"text\",\n" +
            "                \"analyzer\":\"ik_max_word\",\n" +
            "                \"copy_to\":\"all\"\n" +
            "            },\n" +
            "            \"all\":{\n" +
            "                \"type\":\"text\",\n" +
            "                \"analyzer\":\"ik_max_word\"\n" +
            "            }\n" +
            "        }\n" +
            "    }\n" +
            "}";
    //设置请求中的参数
    request.source(json, XContentType.JSON);
    client.indices().create(request, RequestOptions.DEFAULT);
}
```

​		IK分词器是通过请求参数的形式进行设置的，设置请求参数使用request对象中的source方法进行设置，至于参数是什么，取决于你的操作种类。

**添加文档**：

```java
@Test
//添加文档
void testCreateDoc() throws IOException {
    Book book = bookDao.selectById(1);
    IndexRequest request = new IndexRequest("books").id(book.getId().toString());
    String json = JSON.toJSONString(book);
    request.source(json,XContentType.JSON);
    client.index(request,RequestOptions.DEFAULT);
}
```

​		添加文档使用的请求对象是IndexRequest，与创建索引使用的请求对象不同。	

**批量添加文档**：

```java
@Test
//批量添加文档
void testCreateDocAll() throws IOException {
    List<Book> bookList = bookDao.selectList(null);
    BulkRequest bulk = new BulkRequest();
    for (Book book : bookList) {
        IndexRequest request = new IndexRequest("books").id(book.getId().toString());
        String json = JSON.toJSONString(book);
        request.source(json,XContentType.JSON);
        bulk.add(request);
    }
    client.bulk(bulk,RequestOptions.DEFAULT);
}
```

​		批量做时，先创建一个BulkRequest的对象，可以将该对象理解为是一个保存request对象的容器，将所有的请求都初始化好后，添加到BulkRequest对象中，再使用BulkRequest对象的bulk方法，一次性执行完毕。

**按id查询文档**：

```java
@Test
//按id查询
void testGet() throws IOException {
    GetRequest request = new GetRequest("books","1");
    GetResponse response = client.get(request, RequestOptions.DEFAULT);
    String json = response.getSourceAsString();
    System.out.println(json);
}
```

​		根据id查询文档使用的请求对象是GetRequest。

**按条件查询文档**：

```java
    @Test
    void testGetby() throws IOException {
        // 设置请求方式对象
        SearchRequest request = new SearchRequest("books");
        // 设置查询条件
        SearchSourceBuilder builder = new SearchSourceBuilder();
        builder.query(QueryBuilders.termQuery("name","java"));
        request.source(builder);
        // 开始查询
        SearchResponse response = client.search(request, RequestOptions.DEFAULT);
        SearchHits hits = response.getHits();
        for (SearchHit hit : hits) {
            System.out.println(hit.getSourceAsString());
        }
    }
```

按条件查询文档使用的请求对象是SearchRequest，查询时调用SearchRequest对象的termQuery方法，需要给出查询属性名，此处支持使用合并字段，也就是前面定义索引属性时添加的all属性。

springboot整合ES的操作到这里就说完了，与前期进行springboot整合redis和mongodb的差别还是蛮大的，主要原始就是我们没有使用springboot整合ES的客户端对象。至于操作，由于ES操作种类过多，所以显得操作略微有点复杂。有关springboot整合ES就先学习到这里吧。

## 5. 整合第三方技术

### 5.1 缓存

企业级应用主要作用是信息处理，当需要读取数据时，由于受限于数据库的访问效率，导致整体系统性能偏低。

<img src="images/image-20220226154148303.png" alt="image-20220226154148303" style="zoom:67%;" />

​									        应用程序直接与数据库打交道，访问效率低

​		为了改善上述现象，开发者通常会在应用程序与数据库之间建立一种临时的数据存储机制，该区域中的数据在内存中保存，读写速度较快，可以有效解决数据库访问效率低下的问题。这一块临时存储数据的区域就是缓存。

<img src="images/image-20220226154233010.png" alt="image-20220226154233010" style="zoom:67%;" />

​				使用缓存，应用程序与缓存打交道，缓存与数据库打交道，访问效率提高

- 缓存是一种介于数据永久存储介质与数据应用之间的数据临时存储介质
- 使用缓存可以有效的减少**低速数据读取过程的次数**（例如磁盘IO），提高系统性能
- 缓存不仅可以用于提高永久性存储介质的数据读取效率，还可以提供临时的数据存储空间，如我们自己自定义缓存，新建一个Map之类的。

#### SP内置缓存解决方案

**步骤①**：导入springboot提供的缓存技术对应的starter

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-cache</artifactId>
</dependency>
```

**步骤②**：启用缓存，在引导类上方标注注解@EnableCaching配置springboot程序中可以使用缓存

```java
@SpringBootApplication
//开启缓存功能
@EnableCaching
public class Springboot19CacheApplication {
    public static void main(String[] args) {
        SpringApplication.run(Springboot19CacheApplication.class, args);
    }
}
```

**步骤③**：设置操作的数据是否使用缓存

```java
@Service
public class BookServiceImpl implements BookService {
    @Autowired
    private BookDao bookDao;
		
  // value--缓存存储空间的名字；key——缓存的唯一标识（这里的id对应形参的Integer id）；缓存id的对应数据——方法的返回值
    @Cacheable(value="cacheSpace",key="#id")
    public Book getById(Integer id) {
        return bookDao.selectById(id);
    }
}
```

上例中value属性描述缓存的存储位置，可以理解为是一个存储空间名，key属性描述了缓存中保存数据的名称，使用#id读取形参中的id值作为缓存名称。

==@Cacheable注解执行流程：==当该方法被调用时，首先会调用这个注解，去对应的缓存空间找key=参数id的数据，假如找到了就会直接返回缓存里面key对应的数据，不再调用方法。没有找到这个key时，才会调用下面的方法。

##### --手机验证码案例

这个是后面缓存技术讲解的最初案例，实际开发中大家可能都接触过使用Redis作为缓存是较多的，上面SP的内置缓存性能一般，SP考虑到了这一点，他很聪明，就和java里面的jdbc一样，SP提供了接口，方便企业自由选择使用的缓存技术。

<img src="images/image-20220505210923459.png" alt="image-20220505210923459" style="zoom:80%;" />

**后面介绍整合几种常用的技术，手机验证码案例原始是内置解决方案（采用的是内存级缓存），我们会一一进行技术更换，使用专业的缓存技术商提供的，从中可以看到SP整合这些技术的方便之处。**

<img src="images/image-20220505211201066.png" alt="image-20220505211201066" style="zoom:80%;" />

**步骤①**：导入springboot提供的缓存技术对应的starter

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-cache</artifactId>
</dependency>
```

**步骤②**：启用缓存，在引导类上方标注注解@EnableCaching配置

```java
@SpringBootApplication
//开启缓存功能
@EnableCaching
public class Springboot19CacheApplication {
    public static void main(String[] args) {
        SpringApplication.run(Springboot19CacheApplication.class, args);
    }
}
```

**步骤③**：定义Controller与Service的基本逻辑。

需求一：后端接收前端发送的手机号，然后返回前端验证码。这里前端发送参数既可以采用url，也可以采用传统的？拼接。我们就采用传统的？，使用@RequestParam注解。

```java
localhost:80/sms?tele=13973034756
```

需求二：后端接收前端发送的手机号以及人工输入的验证码，返回前端验证结果。这里有两个以上参数，所以我们前端采用请求体json串的方式，后端采用@RequestBody+实体类属性接收。

<img src="images/image-20220506182030676.png" alt="image-20220506182030676" style="zoom:80%;" />

```java
// Controller
@RestController
@RequestMapping("/sms")
public class SMCodeController {

    @Autowired
    private SMCodeService service;

    @GetMapping()
    public String getCode(@RequestParam("tele") String tele){
        String code = service.sendCodeToSM(tele);
        return code;
    }

    @PostMapping
    public boolean checkCode(@RequestBody SMCode smCode){
        return service.checkCode(smCode);
    }
}

// Service && ServiceImpl
public interface SMCodeService {
    String sendCodeToSM(String tele);
    boolean checkCode(SMCode smCode);
}

@Service
public class SMCodeServiceImpl implements SMCodeService {

    @Autowired
    private Codeutils codeutils;

    @Override
    @CachePut(value = "smCode",key = "#tele")
    public String sendCodeToSM(String tele) {
        return codeutils.generateCode(tele);
    }

    @Override
    public boolean checkCode(SMCode smCode) {
        String tele = smCode.getTele();
        String code = smCode.getCode();
        // 取出内存中缓存的验证码
        String cacheCode = codeutils.getCacheCode(tele);
        return Objects.equals(cacheCode,code);
    }
}
```

 ==@CachePut(value = "smCode",key = "#tele")==：这个注解就是单纯的存入key=tele的缓存数据，然后就没了，他不会去取出缓存的数据，不会停止下面方法的调用，懂了吧。@Cacheable注解是缓存中没有值则放入值，缓存中有值则取值。

**步骤④**：对于校验验证码的功能建议放入工具类中进行。定义验证码的生成策略与根据手机号读取验证码的功能。

==这里一定注意的一个重点重点：我们所有的方法必须放到Spring容器里面才行，这样他们上面的注解才会生效==，也就是说假如我们在SMCodeServiceImpl，下一个一模一样获取缓存验证码的方法就会失效，因为这个方法不受容器管理！！！！

```java
@Component
public class Codeutils {
    private String[] patch = {"00000","0000","000","00","0",""};

    public String generateCode(String tele){
        int hash = tele.hashCode();
        int encryption = 2021210827;
        long result = hash^encryption;
        long time  = System.currentTimeMillis();
        result = result ^ time;
        long code = result % 1000000;
        // 保证结果均为正数
        code = code <0 ? -code:code;
        // 保证一定取出来为6位
        String codeStr = code + "";
        int length = codeStr.length();
        return patch[length-1] + codeStr;
    }

    // 取出缓存里面的验证码
    @Cacheable(value = "smCode", key = "#tele")
    public String getCacheCode(String tele){
        return null;
    }
}
```

#### SpringBoot整合Ehcache缓存

==手机验证码案例原始是内置解决方案（采用的是内存级缓存）我们会一一进行技术更换，使用专业的缓存技术商提供的，从中可以看到SP整合这些技术的方便之处。==第一个整合Ehcache技术。Ehcache是一种缓存技术，使用springboot整合Ehcache其实就是变更一下缓存技术的实现方式，话不多说，直接开整。

==需要安装独立的服务器，这种是内存级缓存==

**步骤①**：导入Ehcache的坐标

```xml
<dependency>
    <groupId>net.sf.ehcache</groupId>
    <artifactId>ehcache</artifactId>
</dependency>
```

此处为什么不是导入Ehcache的starter，而是导入技术坐标呢？其实springboot整合缓存技术做的是通用格式(接口），不管你整合哪种缓存技术，只是实现变化了，操作方式一样。这也体现出springboot技术的优点，统一同类技术的整合方式。

**步骤②**：配置缓存技术实现使用Ehcache

```yaml
spring:
  cache:
    type: ehcache
    ehcache:
      config: ehcache.xml
```

配置缓存的类型type为ehcache。

由于ehcache的配置有独立的配置文件格式，因此还需要指定ehcache的配置文件，以便于读取相应配置。

<img src="images/image-20220506202818842.png" alt="image-20220506202818842" style="zoom:80%;" />

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:noNamespaceSchemaLocation="http://ehcache.org/ehcache.xsd"
         updateCheck="false">
    <diskStore path="D:\ehcache" />

    <!--默认缓存策略 -->
    <!-- external：是否永久存在，设置为true则不会被清除，此时与timeout冲突，通常设置为false-->
    <!-- diskPersistent：是否启用磁盘持久化-->
    <!-- maxElementsInMemory：最大缓存数量-->
    <!-- overflowToDisk：超过最大缓存数量是否持久化到磁盘-->
    <!-- timeToIdleSeconds：最大不活动间隔，设置过长缓存容易溢出，设置过短无效果，可用于记录时效性数据，例如验证码-->
    <!-- timeToLiveSeconds：最大存活时间-->
    <!-- memoryStoreEvictionPolicy：缓存清除策略-->
    <defaultCache
        eternal="false"
        diskPersistent="false"
        maxElementsInMemory="1000"
        overflowToDisk="false"
        timeToIdleSeconds="60"
        timeToLiveSeconds="60"
        memoryStoreEvictionPolicy="LRU" />
  
    <!-- name：java里面指定的缓存命名空间（value = "smCode"）-->
    <cache
        name="smCode"
        eternal="false"
        diskPersistent="false"
        maxElementsInMemory="1000"
        overflowToDisk="false"
        timeToIdleSeconds="10"
        timeToLiveSeconds="10"
        memoryStoreEvictionPolicy="LRU" />
</ehcache>
```

注意前面的案例中，设置了数据保存缓存命名空间是smCode。在企业开发过程中，通过设置不同名称的cache来设定不同的缓存策略，应用于不同的缓存数据。

<img src="images/image-20220506202925233.png" alt="image-20220506202925233" style="zoom:80%;" />

真的是太简单了，导入对应的技术，配置一下使用该技术，没得了。就和jdbc的设计方式一模一样（统一接口，供应商只需要做实现），很强，好吧。

#### 数据淘汰策略

知识加油站：介绍一下上面ehcahce里面那个`<!-- memoryStoreEvictionPolicy：缓存清除策略-->`淘汰策略。

<img src="images/image-20220506203903924.png" alt="image-20220506203903924" style="zoom:80%;" />



#### SpringBoot整合Redis缓存

==需要安装独立的Redis服务器，连接时需要输入对应的服务器地址，这种是远程缓存==

**步骤①**：导入redis的坐标

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

**步骤②**：配置缓存技术实现使用redis

```yaml
spring:
  redis:
    host: localhost
    port: 6379
  cache:
    type: redis
```

如需要对redis作为缓存进行配置，注意不是对原始的redis这个软件进行配置，而是配置redis作为缓存使用相关的配置，隶属于spring.cache.redis节点下，注意不要写错位置了。

```yaml
spring:
	# redis的连接信息
  redis:
    host: localhost
    port: 6379
  # redis的缓存配置信息
  cache:
    type: redis
    redis:
      use-key-prefix: true  # 是否加上key的前缀——缓存命名空间，（默认true）
      key-prefix: sms_  # 是否在在key最前面多加上一个前缀（默认无）
      cache-null-values: false  # 是否缓存空值
      time-to-live: 10s  # 缓存存活时间
```

默认情况下，我们在redis里面存的key是下面这个，`@Cacheable(value = "smCode", key = "#tele")`

<img src="images/image-20220506222307231.png" alt="image-20220506222307231" style="zoom:80%;" />

假如设置`use-key-prefix: false`，缓存空间key——`13973034756`，且`key-prefix: sms_` 无效

假如设置`use-key-prefix: true，key-prefix: sms_`，             则缓存空间key——` sms_smCode：：13973034756`

#### SpringBoot整合Memcached缓存

==需要安装独立的Memcached服务器，连接时需要输入对应的服务器地址，这种是远程缓存==

本节研究一下国内比较流行的一款缓存memcached。

springboot并没有支持使用memcached作为其缓存解决方案，也就是说在type属性中没有memcached的配置选项，这里就需要更变一下处理方式了。在整合之前先安装memcached。

##### **安装**

windows版安装包下载地址：https://www.runoob.com/memcached/window-install-memcached.html

下载的安装包是解压缩就能使用的zip文件，解压缩完毕后会得到如下文件

![image-20220226174957040](images/image-20220226174957040.png)

可执行文件只有一个memcached.exe，使用该文件可以将memcached作为系统服务启动，执行此文件时会出现报错信息，如下：

<img src="images/image-20220226175141986.png" alt="image-20220226175141986" style="zoom:80%;" />

​		此处出现问题的原因是注册系统服务时需要使用管理员权限，当前账号权限不足导致安装服务失败，切换管理员账号权限启动命令行

<img src="images/image-20220226175302903.png" alt="image-20220226175302903" style="zoom:80%;" />

​		然后再次执行安装服务的命令即可，如下：

```CMD
memcached.exe -d install
```

​		服务安装完毕后可以使用命令启动和停止服务，如下：

```cmd
memcached.exe -d start		# 启动服务
memcached.exe -d stop		# 停止服务
```

​		也可以在任务管理器中进行服务状态的切换

<img src="images/image-20220226175441675.png" alt="image-20220226175441675" style="zoom:67%;" />

##### **实现**

由于memcached未被springboot收录为缓存解决方案(未被整合 ），因此使用memcached需要通过手工硬编码的方式来使用，于是前面的套路都不适用了，需要自己写了。

<img src="images/image-20220507103312367.png" alt="image-20220507103312367" style="zoom:80%;" />

**步骤①**：导入xmemcached的坐标

```xml
<dependency>
    <groupId>com.googlecode.xmemcached</groupId>
    <artifactId>xmemcached</artifactId>
    <version>2.4.7</version>
</dependency>
```

**步骤②**：配置memcached，这里SP都没有整合这个技术，所以哪里来的配置呢，我们需要自己单独写配置文件才行，不能直接在yml里面写。将我们的Xmemcached客户端放到容器里面管理，之后使用的时候，直接注入就欧克

```java
@Configuration
public class XMemcachedConfig {
    @Bean
    public MemcachedClient getMemcachedClient() throws IOException {
        MemcachedClientBuilder memcachedClientBuilder = new XMemcachedClientBuilder("localhost:11211");
        MemcachedClient memcachedClient = memcachedClientBuilder.build();
        return memcachedClient;
    }
}
```

memcached默认对外服务端口11211。

**步骤③**：使用xmemcached客户端操作缓存，注入MemcachedClient对象

```java
@Service
public class SMSCodeServiceImpl implements SMSCodeService {
		@Autowired
    private MemcachedClient memcachedClient;

    @Override
    public String sendCodeToSM(String tele) {
        String code = codeutils.generateCode(tele);
        // 将验证码数据放到缓存中
        try {
            memcachedClient.set(tele,0,code);
        } catch (Exception e) {
            e.printStackTrace();
        }
        return code;
    }

    @Override
    public boolean checkCode(SMCode smCode) {
        // 取出缓存里面的验证码
        String cacheCode = null;
        try {
            cacheCode = memcachedClient.get(smCode.getTele());
        } catch (Exception e) {
            e.printStackTrace();
        }
        // 得到前端输入的验证码
        String code = smCode.getCode();
        return Objects.equals(cacheCode, code);
    }
}
```

设置值到缓存中使用set操作，取值使用get操作，其实更符合我们开发者的习惯。

==但是上述代码中对于服务器的配置使用硬编码写死到了代码中`("localhost:11211")`，我们应该数据提取出来，做成独立的配置属性，如下。==

**自定义配置属性**

采用前期学习的属性配置方式进行，当前操作有助于理解原理篇中的很多知识。

- 定义配置类，加载必要的配置属性，读取配置文件中memcached节点信息

  ```java
  @Component
  @ConfigurationProperties(prefix = "memcached")
  @Data
  public class XMemcachedProperties {
      private String servers;
      private int poolSize;
      private long opTimeout;
  }
  ```

- 自定义memcached节点信息

  ```yaml
  memcached:
    servers: localhost:11211
    poolSize: 10
    opTimeout: 3000
  ```

- 在memcached配置类中加载信息

```java
@Configuration
public class XMemcachedConfig {
    @Autowired
    private XMemcachedProperties props;
    @Bean
    public MemcachedClient getMemcachedClient() throws IOException {
        MemcachedClientBuilder memcachedClientBuilder = new XMemcachedClientBuilder(props.getServers());
        memcachedClientBuilder.setConnectionPoolSize(props.getPoolSize());
        memcachedClientBuilder.setOpTimeout(props.getOpTimeout());
        MemcachedClient memcachedClient = memcachedClientBuilder.build();
        return memcachedClient;
    }
}
```

**总结**

1. memcached安装后需要启动对应服务才可以对外提供缓存功能，安装memcached服务需要基于windows系统管理员权限
2. 由于springboot没有提供对memcached的缓存整合方案，需要采用手工编码的形式创建xmemcached客户端操作缓存
3. 导入xmemcached坐标后，创建memcached配置类，注册MemcachedClient对应的bean，用于操作缓存
4. 初始化MemcachedClient对象所需要使用的属性可以通过自定义配置属性类的形式加载

#### **上面的总结**

到这里已经完成了三种缓存的整合，其中redis和mongodb需要安装独立的服务器，连接时需要输入对应的服务器地址，这种是远程缓存，Ehcache是一个典型的内存级缓存，因为它什么也不用安装，启动后导入jar包就有缓存功能了。这个时候就要问了，能不能这两种缓存一起用呢？咱们下节再说。

#### SpringBoot整合jetcache缓存

说白了，这不是一种和上面redis一样的缓存技术，他是一个底层的缓存框架，由阿里开源，jetCache对SpringCache进行了封装，在原有功能基础上实现了多级缓存、缓存统计、自动刷新、异步调用、数据报表等功能。解决了AB两种缓存一起用的问题！！整体技术使用方式和springboot的缓存解决方案思想非常类似。

做之前要先明确一下，jetcache并不是随便拿两个缓存都能拼到一起去的，是有支持范围的，如下：

<img src="images/image-20220507162634485.png" alt="image-20220507162634485" style="zoom:80%;" />

我们下面以LinkedHashMap+Redis的方案实现本地与远程缓存方案同时使用。

##### 纯远程方案

**步骤①**：导入springboot整合jetcache对应的坐标starter，当前坐标默认使用的远程方案是redis

```xml
<dependency>
    <groupId>com.alicp.jetcache</groupId>
    <artifactId>jetcache-starter-redis</artifactId>
    <version>2.6.2</version>
</dependency>
```

**步骤②**：远程方案基本配置

​				==注意到这个区域名哦，是一个重要信息，下面会涉及到。==

```yaml
jetcache:
  remote:
    default:  # 这个default是远程缓存配置的一个区域名，可以有多个
      type: redis
      host: localhost
      port: 6379
      poolConfig:
        maxTotal: 50
   sm:   # sm是远程缓存配置的一个区域名，可以有多个
    type: redis
    host: localhost
    port: 6379
    poolConfig:
      maxTotal: 10
```

​				其中poolConfig是必配项，否则会报错。

**步骤③**：启用缓存，

```java
@SpringBootApplication
//jetcache启用缓存的主开关
@EnableCreateCacheAnnotation  // 启用可以使用注解的方式创建缓存
public class Springboot20JetCacheApplication {
    public static void main(String[] args) {
        SpringApplication.run(Springboot20JetCacheApplication.class, args);
    }
}
```

**步骤④**：创建缓存对象Cache，并使用注解@CreateCache标记当前缓存的信息，然后使用Cache对象的API操作缓存，put写缓存，get读缓存。

​	==这个注解里面的缓存信息area就是配置的区域名，由area+name实现针对不同数据的灵活缓存==

```java
@Service
public class SMSCodeServiceImpl implements SMSCodeService {
    @Autowired
    private CodeUtils codeUtils;
    
    // 创建jetcache的缓存空间. name--缓存命名空间,area--指定使用哪一个缓存配置区域名，默认“default”
    @CreateCache(name = "myjetCache_",expire = 10,timeUnit = TimeUnit.SECONDS)
    private Cache<String,String> jetCache;

    // 创建jetcache的缓存空间. name--缓存命名空间,area--指定使用哪一个缓存配置区域名，默认“default”
    @CreateCache(area = "sm", name = "myjetCache2_",expire = 10,timeUnit = TimeUnit.SECONDS)
    private Cache<String,String> jetCache2;


    public String sendCodeToSMS(String tele) {
        String code = codeUtils.generator(tele);
      // jetcache存入缓存空间
        jetCache.put(tele,code);
        return code;
    }

    public boolean checkCode(SMSCode smsCode) {
       // jetcache从缓存空间取数据
        String code = jetCache.get(smsCode.getTele());
        return smsCode.getCode().equals(code);
    }
}
```

在redis里面存储的key名如下：由区域名+缓存空间名+存储的key名组成。

<img src="images/image-20220507181346797.png" alt="image-20220507181346797" style="zoom:80%;" />

##### 纯本地方案

很简单，首先我们要进行本地方案的配置撒，和远程的差不多：

```yml
jetcache:
  local:
    default: # 这个default是本地缓存配置的一个区域名，可以有多个
      type: linkedhashmap
      keyConvertor: fastjson
  remote:
    default: # 这个default是远程缓存配置的一个区域名，可以有多个
      type: redis
      host: localhost
      port: 6379
      poolConfig:
        maxTotal: 50
    sm: # sm是远程缓存配置的一个区域名，可以有多个
      type: redis
      host: localhost
      port: 6379
      poolConfig:
          maxTotal: 50
```

然后，在使用注解@CreateCache创建jetcache的时候，其实里面还有一个参数，就是选择本地缓存、远程缓存还是两者都用。

```java
// 创建jetcache的缓存空间. name--缓存命名空间,area--指定使用哪一个缓存配置区域名，默认“default”，cacheType——使用哪种缓存方案，默认是Remote。
@CreateCache(name = "myjetCache_",expire = 10,timeUnit = TimeUnit.SECONDS, cacheType = CacheType.LOCAL)
private Cache<String,String> jetCache;
```

默认是选择的Remote，所以前面我们使用远程缓存时，没有加这个参数。

<img src="images/image-20220508133608475.png" alt="image-20220508133608475" style="zoom:80%;" />

##### 本地+远程

这就更简单了，其实现在具体使用哪个配置，是一个一层层阶梯的关系，最前面的是            remote/local——区域名——缓存空间命名。

在创建缓存的时候，配置cacheType为BOTH即则本地缓存与远程缓存同时使用。这里是使用的remote和local下的两个default配置。

```java
@Service
public class SMSCodeServiceImpl implements SMSCodeService {
    @CreateCache(name="jetCache_",expire = 1000,timeUnit = TimeUnit.SECONDS,cacheType = CacheType.BOTH)
    private Cache<String ,String> jetCache;
}
```

前面我们对jetcache的本地、远程的缓存配置，都是写的必须要有的，否则就会出错，当然还有很多其他的配置，如下：

<img src="images/image-20220508134517125.png" alt="image-20220508134517125" style="zoom:80%;" />

<img src="images/image-20220508134533218.png" alt="image-20220508134533218" style="zoom:80%;" />

##### 方法缓存

前面我们把数据放到缓存空间，都是手动的进行存取，put和get方法，但这个人工手动控制多捞啊，对不对，所以阿里给我们弄了自动的缓存存取功能，以注解的方式！！！！——方法缓存

**步骤①**：添加remote—redis的配置信息。

因为我们redis只支持保存String字符串类型，不支持保存Object对象，因此需要对redis设置当Object类型数据进入到redis中时的类型转换，keyConvertor表示key的类型转换方式，同时对于value的转换：valueEncode为java，表明值进入redis时是java类型；标注valueDecode为java，表明值从redis中读取时转换成java。

```yml
jetcache:
  statIntervalMinutes: 1  # 每隔1min向控制台打印一次缓存空间统计信息
  local:
    default: # 这个default是本地缓存配置的一个区域名，可以有多个
      type: linkedhashmap
      keyConvertor: fastjson
  remote:
    default: # 这个default是远程缓存配置的一个区域名，可以有多个
      type: redis
      host: localhost
      port: 6379
      keyConvertor: fastjson  # redis不支持保存Object
      valueEncode: java
      valueDecode: java
      poolConfig:
        maxTotal: 50
    sm: # sm是远程缓存配置的一个区域名，可以有多个
      type: redis
      host: localhost
      port: 6379
      poolConfig:
          maxTotal: 50
```

**步骤②**：实体类实现序列化接口

注意，为了实现Object类型的值进出redis，需要保障进出redis的Object类型的数据必须实现序列化接口。

<img src="images/image-20220508150211139.png" alt="image-20220508150211139" style="zoom:80%;" />

**步骤③**：启用方法缓存.

```java
@SpringBootApplication
//jetcache启用缓存的主开关
@EnableCreateCacheAnnotation
//开启方法注解缓存
@EnableMethodCache(basePackages = {"com.itheima"})  // 数组形式
public class Springboot20JetCacheApplication {
    public static void main(String[] args) {
        SpringApplication.run(Springboot20JetCacheApplication.class, args);
    }
}
```

**步骤④**：使用注解@Cached标注当前方法使用缓存

```java
@Service
public class BookServiceImpl implements BookService {
    @Autowired
    private BookDao bookDao;
    
    @Override
    @Cached(name = "book_",key = "#id", expire = 3600,timeUnit = TimeUnit.SECONDS,cacheType = CacheType.REMOTE)
    public Book getById(Integer id) {
        return bookDao.selectById(id);
    }
}
```

##### 远程方案数据同步

​		由于远程方案中redis保存的数据可以被多个客户端共享，这就存在了数据同步问题。jetcache提供了3个注解解决此问题，分别在更新、删除操作时同步缓存数据，和读取缓存时定时刷新数据

**更新缓存**

```JAVA
@CacheUpdate(name="book_",key="#book.id",value="#book")  //这里会把缓存中原来book实体类的信息覆盖掉，而不是部分更新
public boolean update(Book book) {
    return bookDao.updateById(book) > 0;
}
```

**删除缓存**

```JAVA
@CacheInvalidate(name="book_",key = "#id")
public boolean delete(Integer id) {
    return bookDao.deleteById(id) > 0;
}
```

**定时刷新缓存**

```JAVA
 @Cached(name = "book_",key = "#id", expire = 3600,timeUnit = TimeUnit.SECONDS,cacheType = CacheType.REMOTE)
 @CacheRefresh(refresh = 5)  //5s刷新一次缓存内的信息，也就是执行一次下面的查询，查询内容就是前面已经放到缓存空间的key
public Book getById(Integer id) {
    return bookDao.selectById(id);
}
```

##### 数据报表

​		jetcache还提供有简单的数据报表功能，帮助开发者快速查看缓存命中信息，只需要添加一个配置即可

```yaml
jetcache:
  statIntervalMinutes: 1
```

​		设置后，每1分钟在控制台输出缓存数据命中信息

```CMD
[DefaultExecutor] c.alicp.jetcache.support.StatInfoLogger  : jetcache stat from 2022-02-28 09:32:15,892 to 2022-02-28 09:33:00,003
cache    |    qps|   rate|   get|    hit|   fail|   expire|   avgLoadTime|   maxLoadTime
---------+-------+-------+------+-------+-------+---------+--------------+--------------
book_    |   0.66| 75.86%|    29|     22|      0|        0|          28.0|           188
---------+-------+-------+------+-------+-------+---------+--------------+--------------
```

##### 总结

我们jetcache使用==方法缓存==进行操作

| 操作名称 | 使用缓存注解          |
| -------- | --------------------- |
| 查询数据 | @CacheRefresh @Cached |
| 更新数据 | @CacheUpdate          |
| 删除数据 | @CacheInvalidate      |

jetcache存在问题：只能使用固定的4个缓存方案，不能灵活选择。

#### SpringBoot整合j2cache缓存

jetcache可以在限定范围内构建多级缓存，但是灵活性不足，不能随意搭配缓存。

j2cache是一个缓存整合框架，可以提供缓存的整合方案，使各种缓存搭配使用，自身不提供缓存功能。下面我们基于 ehcache + redis 进行整合。

**步骤①**：导入j2cache、redis、ehcache坐标

```xml
<dependency>
    <groupId>net.oschina.j2cache</groupId>
    <artifactId>j2cache-core</artifactId>
    <version>2.8.4-release</version>
</dependency>
<dependency>
    <groupId>net.oschina.j2cache</groupId>
    <artifactId>j2cache-spring-boot2-starter</artifactId>
    <version>2.8.0-release</version>
</dependency>
<dependency>
    <groupId>net.sf.ehcache</groupId>
    <artifactId>ehcache</artifactId>
</dependency>
```

j2cache的starter中默认包含了redis坐标，官方推荐使用redis作为二级缓存，因此此处无需导入redis坐标。

SP的配置文件application.yml配置j2cahce的信息；就只有这一个，具体的信息写在他自己的文件

```yml
j2cache:
  config-location: j2cache.properties
```

**步骤②**：新建j2cache.properties和ehcache.xml 两个配置文件

<img src="images/image-20220508155741837.png" alt="image-20220508155741837" style="zoom:80%;" />

**j2cache.properties：**

配置一级与二级缓存，并配置一二级缓存间数据传递方式，配置书写在名称为j2cache.properties的文件中。

```yaml
# 1级缓存
j2cache.L1.provider_class = ehcache
ehcache.configXml = ehcache.xml

# 2级缓存
j2cache.L2.provider_class = net.oschina.j2cache.cache.support.redis.SpringRedisProvider
j2cache.L2.config_section = redis
redis.hosts = localhost:6379

# 1级缓存中的数据如何到达二级缓存
j2cache.broadcast = net.oschina.j2cache.cache.support.redis.SpringRedisPubSubPolicy
```

​		此处配置不能乱配置，需要参照官方给出的配置说明进行。例如1级供应商选择ehcache，供应商名称仅仅是一个ehcache，但是2级供应商选择redis时要写专用的Spring整合Redis的供应商类名SpringRedisProvider，而且这个名称并不是所有的redis包中能提供的，也不是spring包中提供的。因此配置j2cache必须参照官方文档配置，而且还要去找专用的整合包，导入对应坐标才可以使用。

​		一级与二级缓存最重要的一个配置就是两者之间的数据沟通方式，此类配置也不是随意配置的，并且不同的缓存解决方案提供的数据沟通方式差异化很大，需要查询官方文档进行设置。

**ehcache.xml：**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:noNamespaceSchemaLocation="http://ehcache.org/ehcache.xsd"
         updateCheck="false">
    <diskStore path="C:\Users\Joker_Monster\Desktop\学习资料\Springboot2\开发实用篇—资料（更新中···）\springboot源代码（更新至实用开发篇第五章）补充\springboot_19_cache\ehcache" />

    <!--默认缓存策略 -->
    <!-- external：是否永久存在，设置为true则不会被清除，此时与timeout冲突，通常设置为false-->
    <!-- diskPersistent：是否启用磁盘持久化-->
    <!-- maxElementsInMemory：最大缓存数量-->
    <!-- overflowToDisk：超过最大缓存数量是否持久化到磁盘-->
    <!-- timeToIdleSeconds：最大不活动间隔，设置过长缓存容易溢出，设置过短无效果，可用于记录时效性数据，例如验证码-->
    <!-- timeToLiveSeconds：最大存活时间-->
    <!-- memoryStoreEvictionPolicy：缓存清除策略-->
    <defaultCache
        eternal="false"
        diskPersistent="false"
        maxElementsInMemory="1000"
        overflowToDisk="false"
        timeToIdleSeconds="60"
        timeToLiveSeconds="60"
        memoryStoreEvictionPolicy="LRU" />

    <!-- name：java里面指定的缓存命名空间（value = "smCode"）-->
    <cache
        name="smCode"
        eternal="false"
        diskPersistent="false"
        maxElementsInMemory="1000"
        overflowToDisk="false"
        timeToIdleSeconds="10"
        timeToLiveSeconds="10"
        memoryStoreEvictionPolicy="LRU" />

</ehcache>
```

**步骤③**：使用缓存

```java
@Service
public class SMSCodeServiceImpl implements SMSCodeService {
    @Autowired
    private CodeUtils codeUtils;

    @Autowired
    private CacheChannel cacheChannel;

    public String sendCodeToSMS(String tele) {
        String code = codeUtils.generator(tele);
        cacheChannel.set("sms",tele,code);
        return code;
    }

    public boolean checkCode(SMSCode smsCode) {
        String code = cacheChannel.get("sms",smsCode.getTele()).asString();
        return smsCode.getCode().equals(code);
    }
}
```

​		j2cache的使用和jetcache比较类似，但是无需开启使用的开关，直接定义缓存对象即可使用，缓存对象名CacheChannel。

​		j2cache的使用不复杂，配置是j2cache的核心，毕竟是一个整合型的缓存框架。缓存相关的配置过多，可以查阅j2cache-core核心包中的j2cache.properties文件中的说明。如下：

```properties
#J2Cache configuration
#########################################
# Cache Broadcast Method
# values:
# jgroups -> use jgroups's multicast
# redis -> use redis publish/subscribe mechanism (using jedis)
# lettuce -> use redis publish/subscribe mechanism (using lettuce, Recommend)
# rabbitmq -> use RabbitMQ publisher/consumer mechanism
# rocketmq -> use RocketMQ publisher/consumer mechanism
# none -> don't notify the other nodes in cluster
# xx.xxxx.xxxx.Xxxxx your own cache broadcast policy classname that implement net.oschina.j2cache.cluster.ClusterPolicy
#########################################
j2cache.broadcast = redis

# jgroups properties
jgroups.channel.name = j2cache
jgroups.configXml = /network.xml

# RabbitMQ properties
rabbitmq.exchange = j2cache
rabbitmq.host = localhost
rabbitmq.port = 5672
rabbitmq.username = guest
rabbitmq.password = guest

# RocketMQ properties
rocketmq.name = j2cache
rocketmq.topic = j2cache
# use ; to split multi hosts
rocketmq.hosts = 127.0.0.1:9876

#########################################
# Level 1&2 provider
# values:
# none -> disable this level cache
# ehcache -> use ehcache2 as level 1 cache
# ehcache3 -> use ehcache3 as level 1 cache
# caffeine -> use caffeine as level 1 cache(only in memory)
# redis -> use redis as level 2 cache (using jedis)
# lettuce -> use redis as level 2 cache (using lettuce)
# readonly-redis -> use redis as level 2 cache ,but never write data to it. if use this provider, you must uncomment `j2cache.L2.config_section` to make the redis configurations available.
# memcached -> use memcached as level 2 cache (xmemcached),
# [classname] -> use custom provider
#########################################

j2cache.L1.provider_class = caffeine
j2cache.L2.provider_class = redis

# When L2 provider isn't `redis`, using `L2.config_section = redis` to read redis configurations
# j2cache.L2.config_section = redis

# Enable/Disable ttl in redis cache data (if disabled, the object in redis will never expire, default:true)
# NOTICE: redis hash mode (redis.storage = hash) do not support this feature)
j2cache.sync_ttl_to_redis = true

# Whether to cache null objects by default (default false)
j2cache.default_cache_null_object = true

#########################################
# Cache Serialization Provider
# values:
# fst -> using fast-serialization (recommend)
# kryo -> using kryo serialization
# json -> using fst's json serialization (testing)
# fastjson -> using fastjson serialization (embed non-static class not support)
# java -> java standard
# fse -> using fse serialization
# [classname implements Serializer]
#########################################

j2cache.serialization = json
#json.map.person = net.oschina.j2cache.demo.Person

#########################################
# Ehcache configuration
#########################################

# ehcache.configXml = /ehcache.xml

# ehcache3.configXml = /ehcache3.xml
# ehcache3.defaultHeapSize = 1000

#########################################
# Caffeine configuration
# caffeine.region.[name] = size, xxxx[s|m|h|d]
#
#########################################
caffeine.properties = /caffeine.properties

#########################################
# Redis connection configuration
#########################################

#########################################
# Redis Cluster Mode
#
# single -> single redis server
# sentinel -> master-slaves servers
# cluster -> cluster servers (数据库配置无效，使用 database = 0）
# sharded -> sharded servers  (密码、数据库必须在 hosts 中指定，且连接池配置无效 ; redis://user:password@127.0.0.1:6379/0）
#
#########################################

redis.mode = single

#redis storage mode (generic|hash)
redis.storage = generic

## redis pub/sub channel name
redis.channel = j2cache
## redis pub/sub server (using redis.hosts when empty)
redis.channel.host =

#cluster name just for sharded
redis.cluster_name = j2cache

## redis cache namespace optional, default[empty]
redis.namespace =

## redis command scan parameter count, default[1000]
#redis.scanCount = 1000

## connection
# Separate multiple redis nodes with commas, such as 192.168.0.10:6379,192.168.0.11:6379,192.168.0.12:6379

redis.hosts = 127.0.0.1:6379
redis.timeout = 2000
redis.password =
redis.database = 0
redis.ssl = false

## redis pool properties
redis.maxTotal = 100
redis.maxIdle = 10
redis.maxWaitMillis = 5000
redis.minEvictableIdleTimeMillis = 60000
redis.minIdle = 1
redis.numTestsPerEvictionRun = 10
redis.lifo = false
redis.softMinEvictableIdleTimeMillis = 10
redis.testOnBorrow = true
redis.testOnReturn = false
redis.testWhileIdle = true
redis.timeBetweenEvictionRunsMillis = 300000
redis.blockWhenExhausted = false
redis.jmxEnabled = false

#########################################
# Lettuce scheme
#
# redis -> single redis server
# rediss -> single redis server with ssl
# redis-sentinel -> redis sentinel
# redis-cluster -> cluster servers
#
#########################################

#########################################
# Lettuce Mode
#
# single -> single redis server
# sentinel -> master-slaves servers
# cluster -> cluster servers (数据库配置无效，使用 database = 0）
# sharded -> sharded servers  (密码、数据库必须在 hosts 中指定，且连接池配置无效 ; redis://user:password@127.0.0.1:6379/0）
#
#########################################

## redis command scan parameter count, default[1000]
#lettuce.scanCount = 1000
lettuce.mode = single
lettuce.namespace =
lettuce.storage = hash
lettuce.channel = j2cache
lettuce.scheme = redis
lettuce.hosts = 127.0.0.1:6379
lettuce.password =
lettuce.database = 0
lettuce.sentinelMasterId =
lettuce.maxTotal = 100
lettuce.maxIdle = 10
lettuce.minIdle = 10
# timeout in milliseconds
lettuce.timeout = 10000
# redis cluster topology refresh interval in milliseconds
lettuce.clusterTopologyRefresh = 3000

#########################################
# memcached server configurations
# refer to https://gitee.com/mirrors/XMemcached
#########################################

memcached.servers = 127.0.0.1:11211
memcached.username =
memcached.password =
memcached.connectionPoolSize = 10
memcached.connectTimeout = 1000
memcached.failureMode = false
memcached.healSessionInterval = 1000
memcached.maxQueuedNoReplyOperations = 100
memcached.opTimeout = 100
memcached.sanitizeKeys = false
```

### 5.2 任务

#### Quartz

​		Quartz技术是一个比较成熟的定时任务框架，怎么说呢？有点繁琐，用过的都知道，配置略微复杂。再学习springboot整合Quartz前先普及几个Quartz的概念。

- 工作（Job）：用于定义具体执行的工作
- 工作明细（JobDetail）：用于描述定时工作相关的信息
- 触发器（Trigger）：描述了工作明细与调度器的对应关系
- 调度器（Scheduler）：用于描述触发工作的执行规则，通常使用cron表达式定义规则

简单说就是你定时干什么事情，这就是工作，工作不可能就是一个简单的方法，还要设置一些明细信息。工作啥时候执行，设置一个调度器，可以简单理解成设置一个工作执行的时间。工作和调度都是独立定义的，它们两个怎么配合到一起呢？用触发器。完了，就这么多。下面开始springboot整合Quartz。

**步骤①**：导入springboot整合Quartz的starter

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-quartz</artifactId>
</dependency>
```

**步骤②**：定义工作（Job），按照Quartz的开发规范制作，继承QuartzJobBean

```java
public class MyQuartz extends QuartzJobBean {
    @Override
    protected void executeInternal(JobExecutionContext context) throws JobExecutionException {
        System.out.println("quartz task run...");
    }
}
```

**步骤③**：创建Quartz配置类，定义工作明细（JobDetail）与触发器的（Trigger）bean，触发器内部定义调度器（Scheduler）

```java
@Configuration
public class QuartzConfig {
    @Bean
    public JobDetail printJobDetail(){
        //绑定具体的工作(QuartzJobBean)
        return JobBuilder.newJob(MyQuartz.class).storeDurably().build();
    }
    @Bean
    public Trigger printJobTrigger(){
      // 定义调度器（Scheduler）
        ScheduleBuilder schedBuilder = CronScheduleBuilder.cronSchedule("0/5 * * * * ?");
        //绑定对应的工作明细(JobDetail),使用调度器
        return TriggerBuilder.newTrigger().forJob(printJobDetail()).withSchedule(schedBuilder).build();
    }
}
```

​		有关cron表达式的规则，各位小伙伴可以去参看相关课程学习，略微复杂，而且格式不能乱设置，不是写个格式就能用的，写不好就会出现冲突问题。

#### Task

Quartz将其中的对象划分粒度过细，导致开发的时候有点繁琐，spring针对上述规则进行了简化，开发了自己的任务管理组件——Task。spring根据定时任务的特征，将定时任务的开发简化到了极致，6666

**步骤①**：使用注解@EnableScheduling开启SP的定时任务功能

```java
@SpringBootApplication
//开启定时任务功能
@EnableScheduling
public class Springboot22TaskApplication {
    public static void main(String[] args) {
        SpringApplication.run(Springboot22TaskApplication.class, args);
    }
}
```

**步骤②**：定义一个Bean，在对应要定时执行的操作上方，使用注解@Scheduled定义执行的时间，执行时间的描述方式还是cron表达式

```java
@Component
public class MyBean {
    @Scheduled(cron = "0/1 * * * * ?")
    public void print(){
        System.out.println(Thread.currentThread().getName()+" :spring task run...");
    }
}
```

完事，这就完成了定时任务的配置。总体感觉其实什么东西都没少，只不过没有将所有的信息都抽取成bean，而是直接使用注解绑定定时执行任务的事情而已。

如果想对定时任务进行相关配置，可以通过配置文件进行

```yaml
spring:
  task:
   	scheduling:
      pool:
       	size: 1							# 任务调度线程池大小 默认 1
      thread-name-prefix: ssm_      	# 调度线程名称前缀 默认 scheduling-      
        shutdown:
          await-termination: false		# 线程池关闭时等待所有任务完成
          await-termination-period: 10s	# 调度线程关闭前最大等待时间，确保最后一定关闭
```

### 5.3 邮件

学习邮件发送之前先了解3个概念，这些概念规范了邮件操作过程中的标准。

- SMTP（Simple Mail Transfer Protocol）：简单邮件传输协议，用于**发送**电子邮件的传输协议
- POP3（Post Office Protocol - Version 3）：用于**接收**电子邮件的标准协议
- IMAP（Internet Mail Access Protocol）：互联网消息协议，是POP3的替代协议

简单说就是SMPT是发邮件的标准，POP3是收邮件的标准，IMAP是对POP3的升级。我们java程序中操作邮件，通常是发邮件，所以SMTP是使用的重点，收邮件大部分都是通过邮件客户端完成.

#### 发送简单邮件

**步骤①**：导入springboot整合javamail的starter

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-mail</artifactId>
</dependency>
```

**步骤②**：配置邮箱的登录信息

```yaml
spring:
  mail:
    host: smtp.qq.com  # 使用的邮件协议以及邮件系统供应商
    username: 903379411@qq.com # 我们的邮箱名
    password: fbocxxnnrcdsbcha # 邮件供应商提供的加密密码
```

因为java程序仅用于发送邮件，邮件的功能还是邮件供应商提供的，所以这里是用别人的邮件服务，要配置对应信息。

其中这个 password的获取方式如下：

<img src="images/image-20220509161144129.png" alt="image-20220509161144129" style="zoom:80%;" />

**步骤③**：使用JavaMailSender接口发送邮件

```java
@Service
public class SendMailServiceImpl implements SendMailService {

    @Autowired
    private JavaMailSender javaMailSender;

    // 发送人
    private String from = "903379411@qq.com(小甜甜)";  // 发件人就会显示“小甜甜”
    // 接收人
    private String to = "13973034756@163.com";
    // 标题
    private String title = "测试邮件标题";
    // 正文
    private String context = "测试邮件正文内容";

    @Override
    public void sendMail() {
        SimpleMailMessage mailMessage = new SimpleMailMessage();
        mailMessage.setFrom(from);
        mailMessage.setTo(to);
        mailMessage.setSubject(title);
        mailMessage.setText(context);
        javaMailSender.send(mailMessage);

    }
}
```

#### 发送复杂邮件

​		发送简单邮件仅需要提供对应的4个基本信息就可以了，如果想发送复杂的邮件，需要更换邮件对象。使用MimeMessage可以发送特殊的邮件。

**发送带附件邮件**

```JAVA
@Service
public class SendMailServiceImpl2 implements SendMailService {

    @Autowired
    private JavaMailSender javaMailSender;

    // 发送人
    private String from = "903379411@qq.com";
    // 接收人
    private String to = "13973034756@163.com";
    // 标题
    private String title = "测试邮件标题";
    // 正文
    private String context = "<a href='https://www.douyu.com/directory/myFollow'>测试邮件正文内容</a>";

    @Override
    public void sendMail() {
        MimeMessage mailMessage = javaMailSender.createMimeMessage();
        try {
          // 开启添加附件功能
            MimeMessageHelper helper = new MimeMessageHelper(mailMessage,true); 
            helper.setFrom(from);
            helper.setTo(to);
            helper.setSubject(title);
            helper.setText(context,true); // 开启解析html的文本功能

           // 添加附件
            File f1 = new File("C:/Users/Joker_Monster/Desktop/请假条.pdf");
            File f2 = new File("C:\\Users\\Joker_Monster\\Desktop\\ensemble.png");

            helper.addAttachment("第一个附件.pdf",f1);
            helper.addAttachment("第二个附件.png",f2);

            javaMailSender.send(mailMessage);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### 5.4 消息

#### 简介

从广义角度来说，消息其实就是信息，但是和信息又有所不同。信息通常被定义为一组数据，而消息除了具有数据的特征之外，还有消息的来源与接收的概念。通常发送消息的一方称为消息的生产者，接收消息的一方称为消息的消费者。

消息的工作模式也分为两者，同步消息与异步消息。

同步消息：生产者发送完消息后，必须等消费者回复了，再进行下一步业务。

异步消息：生产者不用等消费者回复，直接是该发送什么就发送什么，与消费者无关，并且可以给多个消费者发送消息。一般企业都是使用这种模式。

在我们Java中，消息又是如何应用的呢？当我们浏览器发送了无数个请求到业务系统时，就会给业务系统带来巨大的压力，肯定是不行的，此时业务系统多新建了几个子业务系统，让他们来具体处理请求，当业务系统拿到这么多请求后，他就把他们以消息的形式先存起来，放到一个叫做MQ的里面，然后子业务系统再从MQ里面拿到消息，进行处理。这个MQ就是我们学习的重点——**MessageQuene。**

#### Java处理消息的标准规范

企业级应用中广泛使用的异步消息传递技术共三大类：

- **JMS**
- **AMQP**
- **MQTT**

为什么是三大类，而不是三个技术呢？因为这些都是规范，就想JDBC技术，是个规范，开发针对规范开发，运行还要靠实现类，例如MySQL提供了JDBC的实现，最终运行靠的还是实现。下面我们简单对三种消息规范做一下简介。

##### JMS

JMS（Java Message Service）：一个规范，等同于JDBC规范，提供了与消息服务相关的API接口。

**JMS消息模型。**

- peer-2-peer：点对点模型，消息发送到一个队列中，队列保存消息。队列的消息只能被一个消费者消费，或超时
- publish-subscribe：发布订阅模型，消息可以被多个消费者消费，生产者和消费者完全独立，不需要感知对方的存在

**JMS消息种类**

- TextMessage
- MapMessage
- BytesMessage（重点）
- StreamMessage
- ObjectMessage
- Message （只有消息头和属性）

**JMS实现：**ActiveMQ、Redis、HornetMQ、RabbitMQ、RocketMQ（没有完全遵守JMS规范）

**缺点：**它只争对java语言，其他语言就不行，为什么这么说，因为有可能我发送者采用的是java语言写的消息系统，但我接收者那边不是用的java语言，而是c++，这就不行了，但用什么语言是没有限制的啊，这就出现了问题——不能跨平台 

##### AMQP

AMQP（advanced message queuing protocol）：一种协议（高级消息队列协议，也是消息代理规范），规范了网络交换的数据格式，兼容JMS

**优点：**具有跨平台性，服务器供应商，生产者，消费者可以使用不同的语言来实现

**AMQP消息模型：**

- direct exchange
- fanout exchange
- topic exchange（重点）
- headers exchange
- system exchange

**AMQP消息种类**：byte[]（跨平台的核心—都是字节数据）

**AMQP实现**：RabbitMQ、StormMQ、RocketMQ

##### MQTT

MQTT（Message Queueing Telemetry Transport）消息队列遥测传输，专为小设备设计，是物联网（IOT）生态系统中主要成分之一由于与JavaEE企业级开发没有交集，此处不作过多的说明。

##### KafKa

除了上述3种J2EE企业级应用中广泛使用的三种异步消息传递技术，还有一种技术也不能忽略，Kafka。

Kafka，一种高吞吐量的分布式发布订阅消息系统，提供实时消息功能。Kafka技术并不是作为消息中间件为主要功能的产品，但是其拥有发布订阅的工作模式，也可以充当消息中间件来使用，而且目前企业级开发中其身影也不少见。

#### SpringBoot整合ActiveMQ

​		ActiveMQ是MQ产品中的元老级产品，早期标准MQ产品之一，在AMQP协议没有出现之前，占据了消息中间件市场的绝大部分份额，后期因为AMQP系列产品的出现，迅速走弱，目前仅在一些线上运行的产品中出现，新产品开发较少采用。

##### 安装

windows版安装包下载地址：[https://activemq.apache.org/components/classic/download](https://activemq.apache.org/components/classic/download/)[/](https://activemq.apache.org/components/classic/download/)

下载的安装包是解压缩就能使用的zip文件，解压缩完毕后会得到如下文件

![image-20220228160001620](images/image-20220228160001620.png)

**启动服务器**

```CMD
activemq.bat
```

​		运行bin目录下的win32或win64目录下的activemq.bat命令即可，根据自己的操作系统选择即可，默认对外服务端口**61616**。

**访问web管理服务**

ActiveMQ启动后会启动一个Web控制台服务，可以通过该服务管理ActiveMQ。

```CMD
http://127.0.0.1:8161/
```

web管理服务默认端口8161，访问后可以打开ActiveMQ的管理界面，如下：

<img src="images/image-20220228160844972.png" alt="image-20220228160844972" style="zoom:67%;" />

​		首先输入访问用户名和密码，初始化用户名和密码相同，均为：admin，成功登录后进入管理后台界面，如下：

![image-20220228161010401](images/image-20220228161010401.png)

​		看到上述界面视为启动ActiveMQ服务成功。

**启动失败**

​		在ActiveMQ启动时要占用多个端口，占用了就关闭。
