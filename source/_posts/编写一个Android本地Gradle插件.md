---
title: 编写一个Android本地Gradle插件
date: 2018-07-10 18:02:10
tags: [Android, Gradle]
---

本文主要讲述如何创建一个可以在本地使用的 gradle 插件。具体步骤如下文所示:

<!-- more -->


### 修改build.gradle

删除该 library 下的build.gradle 里面的内容， 修改为以下内容

```
apply plugin: 'groovy'
apply plugin: 'maven'

dependencies {
    compile gradleApi()
    compile localGroovy()
}

repositories {
    mavenCentral()
}

uploadArchives {
    repositories {
        mavenDeployer {
            pom.groupId = 'com.test.plugin'
            pom.version = '1.0.0'
            pom.artifactId = 'gradle-plugin-demo'
            repository (url : uri('../release'))
        }
    }
}
```

其中在uploadArchives中， pom.groupId定义了该插件的 groupId, pom.version 定义了插件的版本号，pom.artifactId 定义了插件的id。 repository (url : uri('../release')) 标明插件会生成在本地的和JavaLibrary 平级的 release文件下。


### 创建 Groovy 目录和 resources 目录
1. 从File -> New Module... 中选择Java Library, 创建一个 JavaLibrary.
2. 删除 src/main 下面的所有文件夹和文件.
3. 在 main 下面创建 groovy 目录，这个目录将用来放置之后编写的 gradle 插件的groovy 文件；
4. 在 main 下面创建 resources 目录，之下依次创建 META-INF/gradle-plugins 目录

目录创建好之后如图所示：

<img src="1.png" width=50% height=50% align=“left”/>

### 添加 Groovy 文件实现 和 properties 文件
- 在上一步创建好的文件下安装自己的习惯创建目录，并创建 groovy 文件，例如 DemoPlugin.groovy 文件。groovy 文件内容如下所示:

```
package com.demo

import org.gradle.api.Plugin
import org.gradle.api.Project;

public class DemoPlugin implements Plugin<Project> {
    @Override
    void apply(Project project) {
        project.task('demoPrint') << {
            println 'this is a gradle plugin demo'
        }
    }
}
```
这个文件主要内容是创建了一个 "demoPrint" 的task。这个task 进行的操作就是输出 "this is a gradle plugin demo" 这个信息。

- 在之前创建 resources/META-INF/gradle-plugins 创建 properties 文件，文件的前缀应该保持和 pluginId 一致。比如之前我们创建的 pluginId 为pom.artifactId 的值“gradle-plugin-demo”，所以这里应该创建一个名为 
gradle-plugin-demo.properties 的文件。文件内容如下所示，主要是声明了所实现的插件的路径

```
implementation-class=com.demo.DemoPlugin
```

经过以上两步之后，目录结构如下:

<img src="2.png" width=50% height=50% align=“left”/>


### 生成本地的gradle 插件
由于在build.gralde 配置了 uploadArchives 的相关内容，所以可以在本次生成编写的gradle 插件。在该项目中的所有gradle任务中，可以看到如下的任务:

<img src="3.png" width=50% height=50% align=“left”/>

双击运行 uploadArchives任务，可以在之前定义的 release 目录下生成了本次的gradle 插件，内容如下图所示:

<img src="4.png" width=50% height=50% align=“left”/>


### 添加依赖
按照之前的步骤，已经在本次生成了gradle 插件。为了验证编写插件的正确性，可以把它添加到 app module中进行验证。打开 app 模块下的build.gradle, 修改为以下形式: 

```
apply plugin: 'com.android.application'

buildscript {
    repositories {
        maven {
            url uri('../release')
        }
    }

    dependencies {
        classpath 'com.test.plugin:gradle-plugin-demo:1.0.0'
    }

}

apply plugin: 'gradle-plugin-demo'

...
```

点击 "sync now" 进行同步之后，在命令行敲击 "./gradlew demoPrint", 执行定义好的 demoPrint 任务，可以看到以下输出内容:

<img src="5.png" width=50% height=50% align=“left”/>

证明自己定义的task 确实生效了，得到了有效的执行。


### 注意事项

在编写中遇到了 "Plugin id 'gradle-plugin-demo' not found" 之类的提示，后来经过排查发现是因为 “META-INF” 这个文件夹创建时放错了位置，我把它创建在了和main 平级的目录中，应当放在 main 文件夹下。所以相关的文件在创建时，要注意目录顺序。