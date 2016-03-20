---
title: gradle学习笔记 - 命令行、守护进程、包装器
tags:
  - gradle
date: 2015-12-10 22:56:36
---

### Gradle命令行使用

列出所有可用的tasks

<figure class="highlight stylus"><table><tr><td class="code"><pre><span class="line">gradle -<span class="tag">q</span> tasks</span>
</pre></td></tr></table></figure>

需要更多task信息，使用–all选项

<figure class="highlight stylus"><table><tr><td class="code"><pre><span class="line">gradle -<span class="tag">q</span> tasks -all</span>
</pre></td></tr></table></figure>

gradle能够以驼峰式的缩写再命令行上运行任务(任务名字的缩写必须是唯一的)

<figure class="highlight gradle"><table><tr><td class="code"><pre><span class="line"></span>
<span class="line"><span class="keyword">task</span> startSession &lt;&lt; &#123;</span>
<span class="line">    chant()</span>
<span class="line">&#125;</span>
<span class="line"></span>
<span class="line"><span class="keyword">def</span> chant() &#123;</span>
<span class="line">    <span class="keyword">ant</span>.echo(message: <span class="string">'Repeat after me...'</span>)</span>
<span class="line">&#125;</span>
<span class="line"></span>
<span class="line"><span class="number">3</span>.<span class="keyword">times</span> &#123;</span>
<span class="line">    <span class="keyword">task</span> <span class="string">"yayGradle$it"</span> &lt;&lt; &#123;</span>
<span class="line">        <span class="keyword">println</span> <span class="string">'Gradle rocks'</span></span>
<span class="line">    &#125;</span>
<span class="line">&#125;</span>
<span class="line"></span>
<span class="line">yayGradle0.dependsOn startSession</span>
<span class="line">yayGradle2.dependsOn yayGradle1, yayGradle0</span>
<span class="line"><span class="keyword">task</span> groupTerapy(dependsOn: yayGradle2)</span>
</pre></td></tr></table></figure>
<figure class="highlight elixir"><table><tr><td class="code"><pre><span class="line">➜  gradle  gradle gT</span>
<span class="line"><span class="symbol">:startSession</span></span>
<span class="line">[<span class="symbol">ant:</span>echo] <span class="constant">Repeat </span>after me...</span>
<span class="line"><span class="symbol">:yayGradle0</span></span>
<span class="line"><span class="constant">Gradle </span>rocks</span>
<span class="line"><span class="symbol">:yayGradle1</span></span>
<span class="line"><span class="constant">Gradle </span>rocks</span>
<span class="line"><span class="symbol">:yayGradle2</span></span>
<span class="line"><span class="constant">Gradle </span>rocks</span>
<span class="line"><span class="symbol">:groupTerapy</span></span>
<span class="line"></span>
<span class="line"><span class="constant">BUILD SUCCESSFUL</span></span>
<span class="line"></span>
<span class="line"><span class="constant">Total </span><span class="symbol">time:</span> <span class="number">1.474</span> secs</span>
<span class="line"></span>
<span class="line"><span class="constant">This </span>build could be faster, please consider using the <span class="constant">Gradle Daemon:</span> <span class="symbol">https:</span>/<span class="regexp">/docs.gradle.org/</span><span class="number">2.9</span>/userguide/gradle_daemon.html</span>
</pre></td></tr></table></figure>

执行时排除某个一个任务(-x)

<figure class="highlight livecodeserver"><table><tr><td class="code"><pre><span class="line">➜  gradle  gradle <span class="variable">gT</span> -x yayGradle0</span>
<span class="line">:yayGradle1</span>
<span class="line">Gradle rocks</span>
<span class="line">:yayGradle2</span>
<span class="line">Gradle rocks</span>
<span class="line">:groupTerapy</span>
<span class="line"></span>
<span class="line">BUILD SUCCESSFUL</span>
<span class="line"></span>
<span class="line">Total <span class="built_in">time</span>: <span class="number">1.28</span> <span class="built_in">secs</span></span>
<span class="line"></span>
<span class="line">This build could be faster, please consider <span class="keyword">using</span> <span class="operator">the</span> Gradle Daemon: <span class="keyword">https</span>://docs.gradle.org/<span class="number">2.9</span>/userguide/gradle_daemon.html</span>
</pre></td></tr></table></figure>

### 命令行选项

*   -?,h,–help 打印出所有可用的命令行选项
*   -b,–build-file Gradle构建脚本的默认命名月设定是buidle.gradle。使用这个命令行选项可以执行一个特定名字的构建脚本（ex：gradle -b test.gradle）
*   –offline 通常构建中声明的依赖必须在离线仓库中存在才可用，如果这些依赖在本地缓存中没有，那么运行在一个没有网络的环境中的构建都会失败。使用这个选项可以让你以离线模式运行构建，仅仅在本地缓存中检查依赖是否存在

参数选项

*   -D,–system-prop gradle是以一个JVM进程运行的。和所有java进程一样，你可以提供一个系统参数，就像-Dmyprop=myvalue一样
*   -P,–project-prop 项目参数是构建脚本中可用的变量，你可以使用这个选项直接向构建脚本中传入参数（ex：-Pmyprop=myvalue）

日志选项

*   -i,–info 构建发生了什么的详细日志
*   -s,–statcktrace 在有异常抛出时会打印出简单的堆栈跟踪信息
*   -q,–quiet 减少构建出错时打印出来的错误日志信息

帮助任务

*   tasks 显示项目中所有可以运行的task，包括他们的描述信息
*   properties 显示项目中所有可用的属性。

### 守护进程

*   –daemon 运行任务时加上即会开启守护进程
*   –no-daemon 不运行在守护进程内
*   –stop 停止守护进程
<figure class="highlight stylus"><table><tr><td class="code"><pre><span class="line">➜  gradle  gradle gT --stop</span>
<span class="line">Stopping <span class="function"><span class="title">daemon</span><span class="params">(s)</span></span>.</span>
<span class="line">Gradle daemon stopped.</span>
<span class="line">➜  gradle  gradle gT --daemon</span>
<span class="line">Starting <span class="tag">a</span> new Gradle Daemon <span class="keyword">for</span> this build (subsequent builds will be faster).</span>
<span class="line">:startSession</span>
<span class="line">[ant:echo] Repeat after me...</span>
<span class="line">:yayGradle0</span>
<span class="line">Gradle rocks</span>
<span class="line">:yayGradle1</span>
<span class="line">Gradle rocks</span>
<span class="line">:yayGradle2</span>
<span class="line">Gradle rocks</span>
<span class="line">:groupTerapy</span>
<span class="line"></span>
<span class="line">BUILD SUCCESSFUL</span>
<span class="line"></span>
<span class="line">Total <span class="tag">time</span>: <span class="number">2.152</span> secs</span>
</pre></td></tr></table></figure>

### 定义仓库
<figure class="highlight mathematica"><table><tr><td class="code"><pre><span class="line">respositories<span class="list">&#123;</span>
<span class="line">	mavenCentral()</span>
<span class="line">&#125;</span></span>
</pre></td></tr></table></figure>

### 定义依赖
<figure class="highlight groovy"><table><tr><td class="code"><pre><span class="line">dependencies&#123;</span>
<span class="line">	complie <span class="string">group:</span> <span class="string">'org.apache.commons'</span>, <span class="string">name:</span> <span class="string">'commons-lang3'</span>, <span class="string">version:</span> <span class="string">'3.1'</span></span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>

### Gradle包装器

能够让机器在没有安装Gradle运行环境的情况下通过中央仓库下载一个指定的Gradle版本运行构建。（独立于系统，系统配置和Gradle版本的可靠和可重复的构建）

#### 配置包装器

1.  创建包装器任务
<figure class="highlight elm"><table><tr><td class="code"><pre><span class="line"><span class="title">task</span> wrapper(<span class="typedef"><span class="keyword">type</span>: <span class="type">Wrapper</span>) <span class="container">&#123;</span>
<span class="line">    gradleVersion = '1.2'</span>
<span class="line">&#125;</span></span></span>
</pre></td></tr></table></figure>

1.  执行任务
<figure class="highlight groovy"><table><tr><td class="code"><pre><span class="line">➜  todo-webapp-wrapper <span class="string">git:</span>(master) ✗ gradle wrapper</span>
<span class="line">:wrapper</span>
<span class="line"></span>
<span class="line">BUILD SUCCESSFUL</span>
<span class="line"></span>
<span class="line">Total <span class="string">time:</span> <span class="number">2.043</span> secs</span>
<span class="line"></span>
<span class="line">This build could be faster, please consider using the Gradle <span class="string">Daemon:</span> <span class="string">https:</span><span class="comment">//docs.gradle.org/2.9/userguide/gradle_daemon.html</span></span>
</pre></td></tr></table></figure>

#### 定制包装器
<figure class="highlight scala"><table><tr><td class="code"><pre><span class="line">task wrapper(<span class="class"><span class="keyword">type</span>:</span> <span class="type">Wrapper</span>) &#123;</span>
<span class="line">    gradleVersion = <span class="symbol">'2</span><span class="number">.8</span>'</span>
<span class="line">    <span class="comment">//获取gradle包装器url</span></span>
<span class="line">    distributionUrl = <span class="symbol">'https</span>:<span class="comment">//services.gradle.org/distributions/gradle-2.8-bin.zip'</span></span>
<span class="line">    <span class="comment">//包装器解压后存放相对路径</span></span>
<span class="line">    distributionPath = <span class="symbol">'gradle</span>-dists'</span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>