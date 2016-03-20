---
title: gradle学习笔记 构建脚本概要
tags:
  - gradle
date: 2015-12-19 12:57:39
---

### 构建块

一个gradle构建包含三个基本构建块：project、task、property。每个构建包含至少一个project，进而又包含一个或多个task。

#### project

#### task

##### 扩展属性
<figure class="highlight gradle"><table><tr><td class="code"><pre><span class="line"><span class="keyword">project</span>.ext.myProp = <span class="string">'myValue'</span></span>
<span class="line"></span>
<span class="line">ext &#123;</span>
<span class="line">    someOtherProp = <span class="number">123</span></span>
<span class="line">&#125;</span>
<span class="line"></span>
<span class="line">assert myProp == <span class="string">'myValue'</span></span>
<span class="line"><span class="keyword">println</span> <span class="keyword">project</span>.someOtherProp</span>
<span class="line">ext.someOtherProp = <span class="number">567</span></span>
</pre></td></tr></table></figure>

##### Gradle属性

通过再gradle.properties文件中声明直接添加到项目中，这个文件中声明的属性对所有的项目可用。

gradle.properties

<figure class="highlight ini"><table><tr><td class="code"><pre><span class="line"><span class="setting">exampleProp = <span class="value">myValue</span></span></span>
<span class="line"><span class="setting">someOtherProp = <span class="value"><span class="number">455</span></span></span></span>
</pre></td></tr></table></figure>

builde.gradle

<figure class="highlight gradle"><table><tr><td class="code"><pre><span class="line">assert <span class="keyword">project</span>.exampleProp == <span class="string">'myValue'</span></span>
<span class="line"></span>
<span class="line"><span class="keyword">task</span> printGradleProperty &lt;&lt; &#123;</span>
<span class="line">    <span class="keyword">println</span> <span class="string">"Second property: $someOtherProp"</span></span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>
<figure class="highlight groovy"><table><tr><td class="code"><pre><span class="line">➜  gradle-properties <span class="string">git:</span>(master) ✗ gradle pGP</span>
<span class="line">:printGradleProperty</span>
<span class="line">Second <span class="string">property:</span> <span class="number">455</span></span>
<span class="line"></span>
<span class="line">BUILD SUCCESSFUL</span>
<span class="line"></span>
<span class="line">Total <span class="string">time:</span> <span class="number">1.24</span> secs</span>
<span class="line"></span>
<span class="line">This build could be faster, please consider using the Gradle <span class="string">Daemon:</span> <span class="string">https:</span><span class="comment">//docs.gradle.org/2.9/userguide/gradle_daemon.html</span></span>
</pre></td></tr></table></figure>

##### 声明属性的其他方式

前面两种方式，大多用来声明自定义变量及值。Gradle也提供了很多其他方式为构建提供属性：

*   项目属性 -P
*   系统属性 -D
*   环境属性 按此模式提供 ORG_GRADLE_PROJECT_propertyName=someValue