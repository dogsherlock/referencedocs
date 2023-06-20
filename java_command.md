> 这是一些比较常用的命令, 大家可以复制后用typora做成pdf格式，方便快速查询
后续不定期更新


##### maven
maven提供了一系列的成熟的插件: http://maven.apache.org/plugins/index.html


###### maven约定的目录结构
src/main/java src/main/resources src/test/java src/test/resources
target/classes/com.xxx.xx target/generated-sources/annotations target/maven-archiver 
target/maven-status target/xxx.jar

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

如下是maven-model-builder-3.8.6.jar中的pom-4.0.9.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>

<!--
Licensed to the Apache Software Foundation (ASF) under one
or more contributor license agreements.  See the NOTICE file
distributed with this work for additional information
regarding copyright ownership.  The ASF licenses this file
to you under the Apache License, Version 2.0 (the
"License"); you may not use this file except in compliance
with the License.  You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing,
software distributed under the License is distributed on an
"AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
KIND, either express or implied.  See the License for the
specific language governing permissions and limitations
under the License.
-->

<!-- START SNIPPET: superpom -->
<project>
  <modelVersion>4.0.0</modelVersion>

  <repositories>
    <repository>
      <id>central</id>
      <name>Central Repository</name>
      <url>https://repo.maven.apache.org/maven2</url>
      <layout>default</layout>
      <snapshots>
        <enabled>false</enabled>
      </snapshots>
    </repository>
  </repositories>

  <pluginRepositories>
    <pluginRepository>
      <id>central</id>
      <name>Central Repository</name>
      <url>https://repo.maven.apache.org/maven2</url>
      <layout>default</layout>
      <snapshots>
        <enabled>false</enabled>
      </snapshots>
      <releases>
        <updatePolicy>never</updatePolicy>
      </releases>
    </pluginRepository>
  </pluginRepositories>

  <build>
    <directory>${project.basedir}/target</directory>
    <outputDirectory>${project.build.directory}/classes</outputDirectory>
    <finalName>${project.artifactId}-${project.version}</finalName>
    <testOutputDirectory>${project.build.directory}/test-classes</testOutputDirectory>
    <sourceDirectory>${project.basedir}/src/main/java</sourceDirectory>
    <scriptSourceDirectory>${project.basedir}/src/main/scripts</scriptSourceDirectory>
    <testSourceDirectory>${project.basedir}/src/test/java</testSourceDirectory>
    <resources>
      <resource>
        <directory>${project.basedir}/src/main/resources</directory>
      </resource>
    </resources>
    <testResources>
      <testResource>
        <directory>${project.basedir}/src/test/resources</directory>
      </testResource>
    </testResources>
    <pluginManagement>
      <!-- NOTE: These plugins will be removed from future versions of the super POM -->
      <!-- They are kept for the moment as they are very unlikely to conflict with lifecycle mappings (MNG-4453) -->
      <plugins>
        <plugin>
          <artifactId>maven-antrun-plugin</artifactId>
          <version>1.3</version>
        </plugin>
        <plugin>
          <artifactId>maven-assembly-plugin</artifactId>
          <version>2.2-beta-5</version>
        </plugin>
        <plugin>
          <artifactId>maven-dependency-plugin</artifactId>
          <version>2.8</version>
        </plugin>
        <plugin>
          <artifactId>maven-release-plugin</artifactId>
          <version>2.5.3</version>
        </plugin>
      </plugins>
    </pluginManagement>
  </build>

  <reporting>
    <outputDirectory>${project.build.directory}/site</outputDirectory>
  </reporting>

  <profiles>
    <!-- NOTE: The release profile will be removed from future versions of the super POM -->
    <profile>
      <id>release-profile</id>

      <activation>
        <property>
          <name>performRelease</name>
          <value>true</value>
        </property>
      </activation>

      <build>
        <plugins>
          <plugin>
            <inherited>true</inherited>
            <artifactId>maven-source-plugin</artifactId>
            <executions>
              <execution>
                <id>attach-sources</id>
                <goals>
                  <goal>jar-no-fork</goal>
                </goals>
              </execution>
            </executions>
          </plugin>
          <plugin>
            <inherited>true</inherited>
            <artifactId>maven-javadoc-plugin</artifactId>
            <executions>
              <execution>
                <id>attach-javadocs</id>
                <goals>
                  <goal>jar</goal>
                </goals>
              </execution>
            </executions>
          </plugin>
          <plugin>
            <inherited>true</inherited>
            <artifactId>maven-deploy-plugin</artifactId>
            <configuration>
              <updateReleaseInfo>true</updateReleaseInfo>
            </configuration>
          </plugin>
        </plugins>
      </build>
    </profile>
  </profiles>

</project>
<!-- END SNIPPET: superpom -->
```


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


###### 常见插件
* 编译器插件
通过编译器插件，我们可以配置使用的jdk或者说编译器的版本

在settings.xml文件中配置全局编译器插件,找到profiles节点，在里面加入profile节点
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
        <!-- 配置编译器信息(source源信息、target字节码信息、compiler编译过程版本)-->
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
      </properties>
</profile>
```

可以给特定项目配置编译器插件: pom.xml配置片段
```properties
<!-- 配置maven的编译插件 -->
<build>
    <plugins>
    <!-- jdk编译插件 -->
        <plugin>
            <!-- 插件坐标 -->
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.2</version>
            <configuration>
                <!-- 源代码使用jdk版本 -->
                <source>1.7</source>
                <!-- 源代码编译为class文件的版本，要保持跟上面版本一致 -->
                <target>1.7</target>
                <encoding>UTF-8</encoding>
            </connfiguration>
        </plugin>
    </plugins>
</build>
```

* 资源拷贝插件
负责将配置文件复制到编译目录中.默认的编译目录为target/classes
maven只会关注src/main/resources、src/test/resources目录下的配置文件.
分别复制到target/classes和target/test-classes，其它目录下的配置打包时会被忽略.

如果想把非resources下面的文件也打包到classes下面,需要配置pom.xml如下:
```properties
<build>
    <resources>
        <resource>
            <directory>src/main/java</directory>
            <includes>
                <include>**/*.xml</include>
            </includes>
        </resource>
        <resource>
            <directory>src/main/resources</directory>
            <includes>
                <include>**/*.xml</include>
                <include>**/*.properties</include>
            </includes>
        </resource>
    </resources>
</build>
```


* tomcat插件
将tomcat内嵌到web项目中快速启动

##### 常见标签的含义
* modelVersion
描述这个POM文件是遵从哪个版本的项目描述符
* repositories/pluginRepositories
前者是针对项目本身的依赖，后者是对maven命令需要的插件依赖地址(clean/install)
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

> 项目的发布形式jar war rar pom maven-plugin ear ejb par, 默认是jar

pom: 用在父级工程或聚合工程中，用来做jar包的版本控制，必须指明这个聚合工程的打包方式为pom
```properties
<packaging>pom</packaging>

<modules>
    <module>guns-base</module>
    <module>guns-sys</module>
    <module>guns-vip-main</module>
    <module>guns-base-sms</module>
</modules>
```
module即子项目中为:
```properties
<packaging>jar</packaging>
或者
<packaging>war</packaging>
```
聚合工程只是用来帮助其他模块构建的工具，本身并没有实质的内容。具体每个工程代码的编写还是在
生成的工程中去写。



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
        <!-- 配置编译器信息(源信息、字节码信息、编译过程版本)-->
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
父工程设置
```property
<groupId>com.msb</groupId>
<artifactId>MavenDemo</artifactId>
<version>1.0-SNAPSHOT</version>
<packaging>pom</packaging>

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

子工程设置:
```property
<parent>
    <groupId>com.msb</groupId>
    <artifactId>MavenDemo</artifactId>
    <version>1.0-SNAPSHOT</version>
    <relativePath>../MavenDemo/pom.xml</relativePath>
</parent>

<dependencies>
       <dependency>
           <groupId>mysql</groupId>
           <artifactId>mysql-connector-java</artifactId>
       </dependency>
</dependencies>
```


dependencyManagement和dependencies的区别:
1. Dependencies相对于dependencyManagement，所有生命在dependencies里的依赖都会自动引入，并默认被所有的子项目继承。

2. dependencyManagement里只是声明依赖，并不自动实现引入，因此子项目需要显示的声明需要用的依赖。如果不在子项目中声明依赖，是不会从父项目中继承下来的；只有在子项目中写了该依赖项，并且没有指定具体版本，才会从父项目中继承该项，并且version和scope都读取自父pom;另外如果子项目中指定了版本号，那么会使用子项目中指定的jar版本。


* pluginManagement
maven使用dependencyManagement对依赖进行管理, 类似的, maven中还提供了一个名为pluginManagement的标签，可以
管理maven插件. 在pluginManagement的元素中可以声明插件及插件的配置，但不会发生实际的插件调用行为.只有在POM
中配置了真正的plugin元素,且其groupId和artifactId与pluginManagement元素中配置的插件匹配时，pluginManagement标签的配置才会影响到实际的插件行为.

eg: 
假如存在两个项目，项目A为项目B的父项目，其关系通过POM文件的关系确定.项目A的父POM文件片段如下:
```properties
<pluginManagement>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-source-plugin</artifactId>
            <version>2.1</version>
            <configuration>
                <attach>true</attach>
            </configuration>
            <executions>
                <execution>
                    <phase>compile</phase>
                    <goals>
                        <goal>jar</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
    </plugins>
</pluginManagement>
```

如果项目B也想使用该plugin配置，则在项目B的子pom文件中只需要如下配置：
```properties
<plugins>
    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-source-plugin</artifactId>
    </plugin>
</plugins>
```


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
### maven command
#### maven构建生命周期

> maven的lifecyle是对构建过程进行的抽象
> 它包含项目的清理、初始化、编译、测试、打包、集成测试、验证、部署和站点生成等几乎所有的构建步骤
> 这些步骤全都由plugin完成
> 开始-clean-validate-compile-test-package-verify-install-site-deploy-结束

> Maven 的内部有三个标准生命周期，分别是 : clean, default, site
标准生命周期	作用
clean	项目清理
default(build)	项目部署
site	项目站点文档创建

```
validate	验证项目是否正确，所有必要信息是否可用（很少单独使用）
compile	编译项目的源代码（将src/main中的java代码编译成class文件，输出到targe目录下）
test	将单元测试的资源文件和代码进行编译，生成的文件位于target/test-classes （打包部署请跳过该阶段）
package	把class文件，resources文件打包成jar包（也可以是war包），生成的jar包位于target目录下
verify	检查包是否有效（很少单独使用）
install	将jar部署到本地仓库，本地的其他模块依赖该jar包时，可以直接从本地仓库去获取
deploy	将jar包部署到远端仓库，需要在maven的setting.xml中配置私服的用户名和密码，以及在pom.xml配置
```

#### maven命令

* 显示版本信息
mvn -v

mvn clean install -Dmaven.test.skip=true

* 查看项目依赖
```console
在pom.xml所在的目录下执行, 先从本地仓库进行查找，如果找不到才会到远程仓库进行查找，并下载到本地仓库
查看二级层级的依赖关系
mvn dependency:tree  -Doutput=jar包依赖关系.txt
查看完整依赖
mvn dependency:tree -Dverbose -Doutput=jar包依赖关系.txt
```

* 下载项目依赖源码(查看源码是.class文件，找不到sources)
mvn dependency:resolve -Dclassifier=sources

___

##### JVM command
* 基本的命令
javac -encoding utf8 xxx.java(制定了file.encoding为utf8)
-Djava.security.manager

* java命令乱码

默认字符集是在java虚拟机启动时决定的，依赖于java虚拟机所在的操作系统的区域及字符集.
可以通过如 java -Dfile.encoding=UTF-8 -h来查看帮助，指定内容编码为utf8.
-Dfile.encoding是设置系统属性file.encoding为UTF-8.

###### JVM参数
* -Xss
> 用于设置线程堆栈大小，默认是1M,操作系统的堆栈大小限制可以使用
> ulimit -s查看

* 调试场景
> 在一下两种情况中的任何一种情况下触摸它:
> StackOverflowError(堆栈大小大于限制), 增加值
> OutofMemoryError: 无法创建新的本地线程(线程太多，每个线程都有很大的堆栈), 减少它


___

##### 部署

##### war包部署
war包是在进行java web开发时打包的格式，里面包括java代码,还可能有html/css/js前段代码，开发完成后，
都需要把源码打包成war到linux服务器上进行发布
* windows下使用tomcat运行war包
1. 将war包拷贝至tomcat安装路径下webapps文件夹
2. 在bin目录下运行startup.bat
3. 访问项目，访问路径为http://localhost:8080/war包名称/
4. 修改项目访问路径, shutdown.bat关闭tomcat,在安装路径下conf/server.xml中
修改Host->Context节点`<Context path="/demo" docBase="springboot-gradle-1.0-SNAPSHOT" reloadable="true"/>`
docBase指定war包名称,path指定访问路径,reloadable指定是否热部署
5. 重新启动项目并访问http://localhost:8080/指定的路径/

* linux使用tomcat运行war包
与windows类似


* 使用idea部署到外部本地的tomcat上
使用插件Smart Tomcat, EditConfiguration选择Smart Tomcat进行简单配置 

* 将tomcat内嵌到web项目中

使用maven-archetype-webapp模板快速创建一个web项目，在pom.xml进行如下配置:
```properties
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.tomcat.maven</groupId>
            <artifactId>tomcat7-maven-plugin</artifactId>
            <version>2.2</version>

            <configuration>
                <!-- 项目访问路径localhost:9090/sayhello -->
                <path>/sayhello</path>
                <port>8080</port>
                <uriEncoding>UTF-8</uriEncoding>
            </configuration>
        </plugin>
    </plugins>
</build>
```

在Maven->Plugins->tomcat7->tomcat7:run双击运行或mvn tomcat7:run运行
```
tomcat7:run   -- 启动嵌入式tomcat ，并运行当前项目
tomcat7:deploy  -- 部署一个web war包
tomcat7:reload  -- 重新加载web war包
tomcat7:start    -- 启动tomcat
tomcat7:stop    -- 停止tomcat
tomcat7:undeploy  -- 停止一个war包

mvn tomcat7:deploy   //第一次
mvn tomcat7:redeploy   //之后
```

使用内嵌的tomcat插件进行debug
Edit Configuration->Run/Debug Configurations->+->Maven->设置name,将Run填写为
tomcat7:run->Apply->Run/Debug

内嵌的tomcat插件使用的servlet api是tomcat-servlet-api-7.0.47.jar，2019年oracle将javax
捐给eclipse基金会，sprint6与spring boot3将会采用jarkarta作为新的命名空间


##### tomcat配置
tomcat8以后URIEncoding的默认值为UTF-8(原来为ISO-885-1),只影响GET，对于POST请求，默认解析编码
还是ISO-8859-1.

**用户和权限**
admin-gui — 可访问 "host管理" 页面，但"APP管理" 和 "服务器状态" 页面无查看权限
manager-gui — 无 "host管理" 页面访问权限，有"APP管理" 和 "服务器状态" 页面查看权限
manager-status — 只有"服务器状态" 页面查看权限
manager-script — 有脚本方式管理接口访问权限和"服务器状态" 页面查看权限
manager-jmx — JMX 代理接口访问权限和"服务器状态" 页面查看权限
admin-script — 只有host-manager脚本方式管理接口访问权限


> conf/tomcat-users.xml中配置
```xml
<tomcat-users>
      <role rolename="tomcat"/>
      <role rolename="role1"/>
      <role rolename="manager-script"/>
      <role rolename="manager-gui"/>
      <role rolename="manager-status"/>
      <role rolename="admin-gui"/>
      <role rolename="admin-script"/>
      <user username="tomcat" password="tomcat" roles="manager-gui,manager-script,tomcat,admin-gui,admin-script"/>
</tomcat-users>

```