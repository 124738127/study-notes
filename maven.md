# maven notes
in windows OS

## documentation
maven documentation: http://maven.apache.org/guides/index.html

## install
install url: http://maven.apache.org/

1.Java_Home required.

2.add system variable in Path: {{Maven_Home}}/bin.

## setting

maven配置文件分为global setting和user setting。

**global settings**: ${maven.home}/conf/settings.xml  
**user settings**: ${user.home}/.m2/settings.xml

user setting > global setting，因此对于windows用户而言，可以直接删除user setting。

#### setting elements  
  
>`<localRepository>`: maven 本地仓库配置
>
>`<interactiveMode>`: maven 交互方式，是否允许命令行，默认true
>
>`<usePluginRegistry>`: 
>
>`<offline>`: maven 是否非联机方式，默认false
>
>`<proxies>`: 代理
>
>`<servers>`: 服务器配置
>
>`<mirrors>`: maven 仓库的镜像，本地仓库不存在则到远程仓库拉取
>
>`<profiles>`: 
>
>`<activeProfiles>`: 
>
>`<pluginGroups>`: maven 插件，默认包括org.apache.maven.plugins和org.codehaus.mojo


## maven POM
pom.xml是maven项目最基础的工作单元。  

完整的pom文件可能使用的元素参考: http://maven.apache.org/ref/3.6.3/maven-model/maven.html


### super pom
所有pom文件的未定义部分均会继承自super pom文件。  
super pom的maven源码地址: 
```
{{project.home}}\maven-model-builder\src\main\resources\org\apache\maven\model
```

### minimum pom
至少需要如下五个元素才能构成最基本的pom文件：  
```
<project>
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.mycompany.app</groupId>
  <artifactId>my-app</artifactId>
  <version>1</version>
</project>
```
**&lt;project&gt;**: root element  
**&lt;modelVersion&gt;**: should be set to 4.0.0  
>modelVersion: super pom版本

**&lt;groupId&gt;**: the id of the project's group, universally unique  
>groupId: 项目或组件所属的组，全局唯一，通常使用包的完整路径，例如com.mycompany.app

**&lt;artifactId&gt;**: the id of the artifact (project)  
>artifactId: 项目或组件的名称，组内唯一，逐层描述，例如my-app
  
**&lt;version&gt;**: the version of the artifact under the specified group  
>version: 当前项目或组件名称的版本，向下兼容或不兼容均可

>fully qualified artifact name: <groupId&gt;:<artifactId&gt;: <version&gt;  
>eg: com.mycompany.app:my-app:1  

### &lt;parent&gt; & &lt;relativePath&gt; element 
对于存在相似结构的maven工程，可以通过pom继承的方式减少配置工作。  
maven工程通常可以根据文件路径划分为父子结构（上下级）或主从结构（同级）。
  
1. 子pom或从pom使用&lt;parent&gt;元素继承父pom的属性，&lt;parent&gt;元素至少需要标识父pom或主pom的&lt;groupId&gt;、&lt;artifactId&gt;和&lt;version&gt;元素；
2. &lt;relativePath&gt;元素不出现时，可以理解为默认`..\pom.xml`，即上级目录下的pom.xml文件；
3. 对于主从结构的maven工程，由于无法找到`..\pom.xml`，因而必须手工配置相对路径。
  
此外，super pom内不包含&lt;parent&gt;元素，即：根pom不存在继承关系。  

>例1，对于父子结构的文件路径：
>```text
>com.mycompany.app
>|-- my-app
>|  |-- pom.xml
>|  |-- my-module
>|     |-- pom.xml
>```
>my-module下的pom.xml可以有:  
>```xml
><project>
>  <parent>
>    <groupId>com.mycompany.app</groupId>
>    <artifactId>my-app</artifactId>
>    <version>1</version>
>  </parent>
>  <modelVersion>4.0.0</modelVersion>
>  <artifactId>my-module</artifactId>
></project>
>```
注：由于子pom从父pom里继承了&lt;groupId&gt;和&lt;version&gt元素;，因此子pom文件可以省略这两个元素，但建议建立&lt;version&gt;元素，下同。


>例2，对于主从结构的文件路径：
>```text
>com.mycompany.app
>|-- my-app
>|  |-- pom.xml
>|-- my-module
>|  |-- pom.xml
>```
>my-module下的pom.xml可以有：  
>```xml
><project>
>  <parent>
>    <groupId>com.mycompany.app</groupId>
>    <artifactId>my-app</artifactId>
>    <version>1</version>
>    <relativePath>../parent/pom.xml</relativePath>
>  </parent>
>  <modelVersion>4.0.0</modelVersion>
>  <artifactId>my-module</artifactId>
></project>
>```
注：出现两个pom相互继承的情况将会出错。


### &lt;packaging&gt; & &lt;modules&gt;/&lt;module&gt;* element
对于多个maven工程，可以通过pom指定模块的方式聚合：  
1. 使用&lt;packaging&gt;元素标识父pom或主操作pom；
2. 使用&lt;modules&gt;/&lt;module&gt;*元素指定子pom或从操作pom模块。  

这样便可以在构建父工程或主工程时，同时构建其指定的多个maven工程。

使用方法：  

1. &lt;packaging&gt;元素和&lt;modules&gt;元素均为&lt;project&gt;root element的subelement；
2. &lt;modules&gt;下可以指定多个&lt;module&gt;；
3. &lt;module&gt;指定子模块所在目录，可以直接使用子pom的artifactId作为目录，或者使用从pom的相对路径作为目录。  

例如，对于父子结构的文件路径：
```text
com.mycompany.app
|-- my-app
|  |-- pom.xml
|  |-- my-module
|     |-- pom.xml
```
my-app下的pom.xml可以有：  
```xml
<project>
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.mycompany.app</groupId>
  <artifactId>my-app</artifactId>
  <version>1</version>

  <packaging>pom</packaging> 

  <modules>
    <module>my-module</module>
  </modules>
</project>
```

此外，对于主从结构的文件路径，也可以使用相对路径： 
```text
com.mycompany.app
|-- my-app
|  |-- pom.xml
|-- my-module
|  |-- pom.xml
```
my-app下的pom.xml可以有：  
```xml
<project>
  ...
  <packaging>pom</packaging> 

  <modules>
    <module>../my-module</module>
  </modules>
</project>
```
注：maven pom的继承和聚合互不干扰。


### &lt;properties&gt; element

pom文件中支持使用变量，变量要在&lt;properties&gt;元素内配置。  
&lt;properties&gt;是&lt;project&gt;的subelement。  

定义变量时，在&lt;properties&gt;下创建独立的元素，元素名即为变量名；  
使用变量时，使用${*variable*}的方式。  
例如：  
```xml
<project>
  ...
  <properties>
    <maven.superpom.version>4.0.0</maven.superpom.version>
  </properties>
  ...
  <sample>
    <element>${project.maven.superpom.version}</element>
  </sample>
  <sample>
    <element>${project.maven.superpom.version}</element>
  </sample>
  ...
</project>
```
注：maven存在几个默认变量  
>${project.basedir}:项目当前的路径  
>${project.baseUri}:项目当前的路径，uri的方式表示  
>${maven.build.timestamp}:maven构建的时间戳


## maven工程结构
标准maven工程结构参考super pom的定义。  
但用户可以修改pom文件的&lt;build&gt;下的元素自行定义。

```text
sample
|-- src
|   |-- main
|   |   |-- java
|   |   |-- resources
|   |   |-- scripts
|   |-- test
|   |   |-- java
|   |   |-- resources
|   |-- ...
|-- target
|   |-- classes
|   |-- test-classes
|   |-- site
|   |-- ...
|-- pom.xml
|-- README.txt
|-- LICENSE.txt
|-- NOTICE.txt
|-- ...
```

## maven lifecycle
maven lifecycle存在三个内置的lifecycle：
>clean: 处理maven工程的清理  
>>default: 处理maven工程的部署  
>site: 处理maven工程的站点文档  

### clean lifecycle
clean lifecycle的主要操作：
#### maven clean
清除maven工程的`${project.basedir}/target`文件夹，即清除maven构建后生成的文件。

### default lifecycle
default lifecycle的主要操作：
#### maven compile
maven工程编译源代码，包括main目录和test目录

#### maven test
maven工程执行编译后测试单元测试

#### maven package
maven工程编译后打包，生成jar或其他类型的可分发文件

#### maven verify
maven工程可分发文件验证有效性和质量标准

#### maven install
maven工程的可分发文件存储至本地仓库

#### maven deploy
maven工程本地仓库的可分发文件推送至远程仓库，需要配置如下：

1.配置pom.xml &lt;distributionManagement&gt;&&lt;repository&gt;
```xml
<project>
  ...
  <distributionManagement>
    <repository>
      <id>sample-repository</id>
      <name>sample</name>
      <url>https://repository.sample.com/repository/maven</url>
    </repository>
  </distributionManagement>
  ...
</project>
```
2.配置setting.xml &lt;servers&gt;&&lt;server&gt;
```xml
<settings>
...
  <servers>
    <server>
      <id>sample-repository</id>
      <username>sample_user</username>
      <privateKey>~/.ssh/id_dsa</privateKey>
      <passphrase>sample_key_passphrase</passphrase>
    </server>
  </servers>
...
</settings>
```

### site lifecycle
site lifecycle的主要操作：
#### maven site
maven工程生成站点目录


## maven dependency
There is no limit to the number of levels that dependencies can be gathered from. A problem arises only if a cyclic dependency is discovered.

### &lt;DependencyManagement&gt;
Section for management of default dependency information for use in a group of POMs.

只声明依赖，并不实际引入。  
因此子项目需要声明依赖。如果不在子项目中声明依赖，是不会从父项目中继承的；只有在子项目中写了该依赖项，并且没有指定具体版本，才会从父项目中继承该项，并且version和scope都会被读取。

### &lt;Dependencies&gt;
The dependencies specified here are not used until they are referenced in a POM within the group.  
This allows the specification of a "standard" version for a particular dependency.

所有声明在父项目中 dependencies 里的依赖都会被子项目自动引入，并默认被所有的子项目继承。  
dependencyManagement中的 dependencies 并不影响项目的依赖项。
```xml
<project>
  ...
  <dependencyManagement><!--声明依赖项，用于管理-->
    <dependencies>
      <dependency>
        ...
      </dependency>
    </dependencies>
  </dependencyManagement>

  <dependencies><!--引入依赖项，实际使用-->
    <dependency>
      ...
    </dependency>
  </dependencies>
  ...
</project>
```

### &lt;Dependency&gt;
The &lt;Dependency&gt; element contains information about a dependency of the project.
```xml
<project>
  ...
  <dependencies><!--引入依赖项，实际使用-->
    <dependency>
      <groupId>The project group that produced the dependency.</groupId>
      <artifactId>The unique id for an artifact produced by the project group.</artifactId>
      ...
    </dependency>
  </dependencies>
  ...
</project>
```

## maven build

The &lt;build&gt; element contains informations required to build the project. Default values are defined in Super POM.

### maven plugin
In Maven, there are two kinds of plugins, build and reporting:  
1.**Build plugins** are executed during the build and configured in the &lt;build/&gt; element.  
2.**Reporting plugins** are executed during the site generation and configured in the &lt;reporting/&gt; element.

All plugins should have minimal required information: groupId, artifactId and version.

#### &lt;pluginManagement&gt;
Section for management of default plugin information for use in a group of POMs.

#### &lt;plugins&gt;

```xml
<project>
  ...
  <build>
    <pluginManagement><!--声明插件，用于管理-->
      <plugins>
        <plugin>
          ...
        </plugin>
      </plugins>
    </pluginManagement>
    <plugins><!--引入插件，实际使用-->
      <plugin>
        ...
      </plugin>
    </plugins>
  </build>
  ...
</project>
```

#### &lt;plugin&gt;

```xml
<project>
  ...
  <build>
    <plugins><!--引入插件，实际使用-->
      <plugin>
        <groupId>The group ID of the plugin in the repository.</groupId>
        <artifactId>The artifact ID of the plugin in the repository.</artifactId>
        ...
        <dependencies>
          <dependency>
            the dependencies of the Build plugins.
          </dependency>
        </dependencies>
        ...
      </plugin>
    </plugins>
  </build>
  ...
</project>
```

plugin 和 dependency 的区别：  
>dependency: maven工程编译后执行时依赖。  
>plugin: maven工程构建或编译过程中依赖，但编译后执行时不依赖。





