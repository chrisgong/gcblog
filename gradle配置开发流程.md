---
title: gradle配置开发流程
tags:
  - gradle
date: 2015-12-02 17:18:20
---

最近开始学习gradle的使用。今天跑起了第一个task。记录一下过程。

### Step 1

下载地址:

[**Complete distribution**](https://services.gradle.org/distributions/gradle-2.9-all.zip) (binaries, sources and offline documentation)

[**Binary only distribution**](https://services.gradle.org/distributions/gradle-2.9-bin.zip) (no documentation or source code)

[**Gradle source code**](https://services.gradle.org/distributions/gradle-2.9-src.zip) (just the Gradle source code; not a usable Gradle installation)

[文档地址](http://gongchaodemacbook-pro.local:62328/Dash/pghvcpxd/userguide/userguide.html)

### Step 2

下载完解压缩，配置gralde命令路径

<figure class="highlight xquery"><table><tr><td class="code"><pre><span class="line">vim ~/.bash_profile</span>
<span class="line">export PATH=$&#123;PATH&#125;:~/mac/app/gradle-<span class="number">2.9</span>/bin:<span class="variable">$PATH</span></span>
</pre></td></tr></table></figure>

### Step 3

用gradle init来初始化当前目录生成gradle运行环境

<figure class="highlight stata"><table><tr><td class="code"><pre><span class="line">➜  <span class="keyword">mac</span>  <span class="keyword">mkdir</span> gradle</span>
<span class="line">➜  <span class="keyword">mac</span>  <span class="keyword">cd</span> gradle</span>
<span class="line">➜  gradle  gradle init</span>
<span class="line">:wrapper</span>
<span class="line">:init</span>
<span class="line"></span>
<span class="line">BUILD SUCCESSFUL</span>
<span class="line"></span>
<span class="line"><span class="keyword">Total</span> time: 1.668 secs</span>
<span class="line"></span>
<span class="line">This build could be faster, please consider using the Gradle Daemon: https:<span class="comment">//docs.gradle.org/2.9/userguide/gradle_daemon.html</span></span>
</pre></td></tr></table></figure>

至此gradle环境配置完成

### Step 4

开始编写gradle文件

<figure class="highlight css"><table><tr><td class="code"><pre><span class="line"><span class="tag">gradle</span>  <span class="tag">vim</span> <span class="tag">build</span><span class="class">.gradle</span></span>
</pre></td></tr></table></figure>

build.gradle:

<figure class="highlight gradle"><table><tr><td class="code"><pre><span class="line">...</span>
<span class="line"><span class="keyword">task</span> hello &#123;</span>
<span class="line">    <span class="keyword">doLast</span> &#123;</span>
<span class="line">        <span class="keyword">println</span> <span class="string">'Hello world!'</span></span>
<span class="line">    &#125;</span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>
<figure class="highlight stylus"><table><tr><td class="code"><pre><span class="line">➜  gradle  gradle -<span class="tag">q</span> hello</span>
<span class="line">Hello world!</span>
</pre></td></tr></table></figure>

**第一个Gradle Task finish！**