> 这是一些比较常用的命令, 大家可以复制后用typora做成pdf格式，方便快速查询
后续不定期更新



##### maven

###### settings.xml位置
1. <localRepository>标签指定本地下载的依赖在本地的保存位置
> <localRepository>${user.home}/.m2/repository</localRepository>表示C:\Users\username\.m2\settings.xml
2. idea中File | Settings | Build, Execution, Deployment | Build Tools | Maven的local repository可以覆盖


> settings.xml内容
```xml
<?xml version="1.0" encoding="UTF-8"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">

   <pluginGroups></pluginGroups>

   <proxies></proxies>

   <localRepository>${user.home}/.m2/repository</localRepository>

   <mirrors>
      <mirror>
         <id>settings-mirror</id>
         <url>https://maven.aliyun.com/repository/public</url>
         <mirrorOf>central</mirrorOf>
      </mirror>
   </mirrors>

 <profiles>
        <profile>
            <id>xxx-product</id>
            <repositories>
                <repository>
                    <id>xxx-xxx-repo</id>
                    <url>url</url>
                    <releases>
                        <enabled>true</enabled>
                        <updatePolicy>always</updatePolicy>
                    </releases>
                    <snapshots>
                        <enabled>true</enabled>
                        <updatePolicy>always</updatePolicy>
                    </snapshots>
                </repository>
            <repositories>
        </profile>
</settings>
```

> 超级POM的位置(中央仓库的默认配置)
中央仓库的默认配置是http://repo1.maven.org/maven2, 配置在maven-model-buider-版本号.jar的
org\apache\maven\model\pom-4.0.9.xml超级pom中, 所有的POM均继承此文件


###### 依赖下载基本优先级
1. 本地所有Maven项目都复用一个本地仓库
2. 中央仓库从远程仓库/私服(可配置)下载必要的构建
> 本地仓库 > 特定仓库reporitory的镜像mirror > settings中配置的仓库repository
详细优先级: local-repo > settings-profile-repository > pom-profile-repository > pom-repository > central
> 本地仓库 > 私服 （profile）> 远程仓库（repository）和 镜像 （mirror） > 中央仓库 （central）

##### Maven核心概念

###### 仓库地址和坐标之间的关系
```
groupId com.huawei.xxxcommon.xxx
artifactid xxx-service
version 1.0
packaging jar(bundle war...)
classifier 附加构建信息

groupid artifactid version是必须定义的
仓库地址: base + com/huawei/xxcommon/dmq/xxx-service/1.0/xxx.jar
```

###### Maven依赖优选原则
1. 依赖最短路径优先原则
A-B-C-X{1.0} A-D-X{2.0} 选择X{2.0}
2. pom文件中声明顺序优先
在依赖路径长度相等的前提下,在POM中依赖声明的顺序决定了谁会被解析使用.顺序最靠前的那个依赖优胜
如: 先声明的依赖 比如依赖了A-B-X{1.0}, A-C-X{2.0}. 路径长度相同, maven会根据pom文件声明的顺序
加载，如果先声明了B,后声明了C,那就最后的依赖就会是X{1.0}
在同一个pom文件中,相同的资源,后面的会覆盖前面的
3. 覆盖优先原则
子pom内声明的优先于父pom的依赖


可选依赖
可选依赖指对外隐藏当前所依赖的资源---不透明
```property
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
    <optional>true</optional>
</dependency>
```

排除依赖
排除依赖指主动断开依赖的资源,被排除的资源无需指定版本---不需要
```property
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
    <exclusions>
        <exclusion>
        <groupId>org.hamcrest</groupId>
        <artifactId>hamcrest-core</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```


##### 依赖的范围
依赖的jar默认情况可以在任何地方使用,可以通过scope标签设定其作用范围(默认是compile)
```
<scope>标签
五种范围: compile provided runtime system test import
```

scope取值和作用表
```
scope取值 有效范围    依赖传递    是否打入jar包    例子
compile(默认)    all         是           是   spring-core
provided compile,test  否            否   servlet-api
system   compile,test                是       
runtime  runtime,test   是           是   JDBC驱动
test     test           否           否   JUnit
import      是       
```

eg:
test的Junit,只在测试时有效, 包括测试代码的编译,执行,不会打入jar包

runtime的JDBC,在运行、测试时有效，但是在编译代码时无效，打入jar包. 因为项目代码编译
只需要JDK提供的JDBC接口,只有测试或运行项目时,才需要实现上述接口的具体JDBC驱动

provided的servlet-api, 在我们编写servlet时 HttpServletRequest和HttpServletResponse
等对象的是由servlet-api提供的，不导入servlet-api我们无法获取相应对象，但是在运行中，
tomcat中也有这些对象，为了防止冲突，在配置pom文件时，在servlet-api 下设置作用范围<scope>provided</scope>，指该依赖不进行打包，该工程在运行tomcat时就会使用tomcat中的HttpServletRequest对象，在编译时使用servlet-api 的对象

system与provided类似，不过你必须显式指定一个本地系统路径的jar,此类依赖应该一直有效,maven也不会去仓库中寻找它。但是，使用system范围依赖时必须通过systemPath元素显示地指定依赖文件的路径。

##### maven生命周期
1. 三套生命周期相互独立
clean(pre-clean clean post-clean) > default(validate compile test package verify install deploy) > site(pre-site site post-site site-deploy)
2. 一套生命周期中，生命周期阶段前后依赖
3. mvn命令指向生命周期阶段

* package和install的区别
package指令做了一件事情:
1. 将项目打包(jar/war),将打包结果放到项目下的target目录下
install指令做了两件事情:
1. 将项目打包(jar/war),将打包结果放到项目下的target目录下
2. 同时将上述打包结果放到本地仓库的相应目录中,供其它项目或模块引用 


##### maven插件
* Maven本身是一个框架，实际的任务都由插件完成
* 插件与生命周期阶段绑定，用户通过指定生命周期阶段就能够隐式的通过插件执行任务
* 打包类型(packaging)控制default生命周期与插件目标(plugin goal)的绑定
默认maven在各个生命周期上绑定有预设的功能, 通过插件可以自定义其他功能


在generate-test-resources中加入插件执行对应的目标
`https://maven.apache.org/plugins/maven-source-plugin/`
```property
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-source-plugin</artifactId>
            <version>2.2.1</version>
            <executions>
                <execution>
                    <goals>
                        <goal>jar</goal>
                        <goal>test-jar</goal>
                    </goals>
                    <phase>generate-test-resources</phase>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```

###### 常见label的含义
* mirrorOf

> 为某个仓库repository做镜像, 填写的是repository id, * 匹配所有的仓库
> 相当于一个拦截器，它会拦截maven对remote repository的相关请求，
> 把请求里的remote repository地址，重定向到mirror里配置的地址
```
mirrorOf=“*”  //刚才经过，mirror一切，你配置的repository不起作用了

mirrorOf=my-repo-id //镜像my-repo-id，你配置的my-repo-id仓库不起作用了

mirrorOf=*,!my-repo-id  //!表示非运算，排除你配置的my-repo-id仓库，其他仓库都被镜像了
就是请求下载my-repo-id的仓库的jar不使用mirror的url下载，其他都是用mirror配置的url下载

mirrorOf=external:*  //如果本地库存在就用本地库的，如果本地没有所有下载就用mirror配置的url下载
```

> 将apache默认的中央仓库http://repo1.maven.org/maven2替换成aliyun的镜像仓库
```property
<mirrors>
      <mirror>
         <!-- 指定镜像id(自己取名) -->
         <id>nexus-aliyun</id>
         <!-- 匹配中央仓库 -->
         <mirrorOf>central</mirrorOf>
         <!-- 指定镜像名称(自己取名) -->
         <name>Nexus aliyun</name>
         <!-- 指定镜像地址 -->
         <url>https://maven.aliyun.com/repository/public</url>
      </mirror>
</mirrors>
```


* packaging

> 项目的发布形式jar war rar pom maven-plugin ear ejb par


* profile
profile可以让我们定义一系列的配置信息,然后指定其激活条件。这样就可以定义多个profile,然后每个profile对应不同的激活条件和配置信息，从而达到不同环境使用不同配置信息的效果.

> 在settings.xml中指定编译和运行的jdk
```property
<profile>
      <!--  setting.xml中的id指定jdk版本号-->
      <id>jdk-1.8</id>
      <!-- 开启编译器的使用 -->
      <activation>
        <activeByDefault>true</activeByDefault>
        <jdk>1.8</jdk>
      </activation>

      <properties>
        <!-- 配置编译器信息 -->
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
      </properties>
    </profile>
```

* dependencyManagement
使用denpendencyManagement可以统一管理项目的版本号，确保应用的各个项目的依赖和版本一致，不用
每个模块项目都弄一个版本号，不利于管理，当需要变更版本号的时候只需要在父类容器里更新，
不需要任何一个子项目的修改;如果某个子项目需要另一个特殊的版本号时，只需要在自己的模块denpendencies
中声明一个版本号即可。子类就会使用子类声明的版本号，不继承于父类版本号。

eg:
```property
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.44</version>
            <!-- 父工程设置继承了父工程的子工程必须使用这个版本 -->
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

```property
<dependencies>
       <dependency>
           <groupId>mysql</groupId>
           <artifactId>mysql-connector-java</artifactId>
       </dependency>
</dependencies>
```


dependencyManagement和dependncyManagement区别:
1. Dependencies相对于dependencyManagement，所有生命在dependencies里的依赖都会自动引入，并默认被所有的子项目继承。

2. dependencyManagement里只是声明依赖，并不自动实现引入，因此子项目需要显示的声明需要用的依赖。如果不在子项目中声明依赖，
是不会从父项目中继承下来的；只有在子项目中写了该依赖项，并且没有指定具体版本，才会从父项目中继承该项，并且version和scope
都读取自父pom;另外如果子项目中指定了版本号，那么会使用子项目中指定的jar版本。

###### multi-moduls

> maven3支持maven项目的多模块结构(聚合项目), 通常由一个父模块和若干个子模块构成
> 父模块必须以pom打包类型，同时以<modules>给出所有的子模块


* relativePath

> 是maven为了寻找父模块pom.xml所额外增加的一个寻找路径
```xml
<parent>
    <groupId>com.company.department.group</groupId>
    <artifactId>reponame</artifactId>
    <version>xxx-SNAPSHOT</version>
    <relativePath>../../../pom.xml</relativePath>
</parent>
```

___

##### maven插件创建工程
```
创建工程
mvn archetype:generate
-DgroupId={project-packaging}
-DartifactId={project-name}
-DarchetypeArtifactId=maven-archetype-quickstart
-DinteractiveMode=false

创建Java工程
mvn archetype:generate
-DgroupId=com.youaresherlock
-DartifactId=java-project
-DarchetypeArtifactId=maven-archetype-quickstart
-Dversion=0.0.1-snapshot
-DinteractiveMode=false

创建web工程
mvn archetype:generate
-DgroupId=com.youaresherlock
-DartifactId=web-project
-DarchetypeArtifactId=maven-archetype-webapp
-Dversion=0.0.1-snapshot
-DinteractiveMode=false
```

___
##### maven command

* 显示版本信息
mvn -v

* maven构建生命周期

> maven的lifecyle是对构建过程进行的抽象
> 它包含项目的清理、初始化、编译、测试、打包、集成测试、验证、部署和站点生成等几乎所有的构建步骤
> 这些步骤全都由plugin完成
> 开始-clean-validate-compile-test-package-verify-install-site-deploy-结束

> Maven 的内部有三个标准生命周期，分别是 : clean, default, site
标准生命周期	作用
clean	项目清理
default(build)	项目部署
site	项目站点文档创建



* defualt(build)生命周期各阶段详解
```
validate	验证项目是否正确，所有必要信息是否可用（很少单独使用）
compile	编译项目的源代码（将src/main中的java代码编译成class文件，输出到targe目录下）
test	将单元测试的资源文件和代码进行编译，生成的文件位于target/test-classes （打包部署请跳过该阶段）
package	把class文件，resources文件打包成jar包（也可以是war包），生成的jar包位于target目录下
verify	检查包是否有效（很少单独使用）
install	将jar部署到本地仓库，本地的其他模块依赖该jar包时，可以直接从本地仓库去获取
deploy	将jar包部署到远端仓库，需要在maven的setting.xml中配置私服的用户名和密码，以及在pom.xml配置
```

mvn clean install -Dmaven.test.skip=true

* 查看项目依赖
```console
在pom.xml所在的目录下执行, 先从本地仓库进行查找，如果找不到才会到远程仓库进行查找，并下载到本地仓库
查看二级层级的依赖关系
mvn dependency:tree  -Doutput=jar包依赖关系.txt
查看完整依赖
mvn dependency:tree -Dverbose -Doutput=jar包依赖关系.txt
```

___

##### JVM command
* 基本的命令
javac -encoding utf8 xxx.java(制定了file.encoding为utf8)
-Djava.security.manager


###### JVM参数
* -Xss
> 用于设置线程堆栈大小，默认是1M,操作系统的堆栈大小限制可以使用
> ulimit -s查看

* 调试场景
> 在一下两种情况中的任何一种情况下触摸它:
> StackOverflowError(堆栈大小大于限制), 增加值
> OutofMemoryError: 无法创建新的本地线程(线程太多，每个线程都有很大的堆栈), 减少它
