---
title: gradle学习笔记 - Task
tags:
  - gradle
date: 2015-12-02 22:19:39
---

### 构建脚本

task：

<figure class="highlight gradle"><table><tr><td class="code"><pre><span class="line"><span class="keyword">task</span> hello &#123;</span>
<span class="line">    <span class="keyword">doLast</span> &#123;</span>
<span class="line">        <span class="keyword">println</span> <span class="string">'Hello world!'</span></span>
<span class="line">    &#125;</span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>

闭包写法：

<figure class="highlight gradle"><table><tr><td class="code"><pre><span class="line"><span class="keyword">task</span> hello &lt;&lt; &#123;</span>
<span class="line">    <span class="keyword">println</span> <span class="string">'Hello world!'</span></span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>

* * *

构建脚本：

1.  使用Groovy语言
<figure class="highlight gradle"><table><tr><td class="code"><pre><span class="line"><span class="keyword">task</span> upper &lt;&lt; &#123;</span>
<span class="line">    String someString = <span class="string">'mY_nAmE'</span></span>
<span class="line">    <span class="keyword">println</span> <span class="string">"Original: "</span> + someString </span>
<span class="line">    <span class="keyword">println</span> <span class="string">"Upper case: "</span> + someString.toUpperCase()</span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>
<figure class="highlight gradle"><table><tr><td class="code"><pre><span class="line"><span class="keyword">task</span> <span class="keyword">count</span> &lt;&lt; &#123;</span>
<span class="line">    <span class="number">4</span>.<span class="keyword">times</span> &#123; <span class="keyword">print</span> <span class="string">"$it "</span> &#125;</span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>

### 任务依赖
<figure class="highlight gradle"><table><tr><td class="code"><pre><span class="line"><span class="keyword">task</span> hello &lt;&lt; &#123;</span>
<span class="line">    <span class="keyword">println</span> <span class="string">'Hello world!'</span></span>
<span class="line">&#125;</span>
<span class="line"><span class="keyword">task</span> intro(dependsOn: hello) &lt;&lt; &#123;</span>
<span class="line">    <span class="keyword">println</span> <span class="string">"I'm Gradle"</span></span>
<span class="line">&#125;</span>
<span class="line"></span>
<span class="line">➜  gradle  gradle -q intro</span>
<span class="line">Hello world!</span>
<span class="line">I<span class="string">'m Gradle</span></span>
</pre></td></tr></table></figure>

Lazy dependsOn - the other task does not exist (yet)

<figure class="highlight gradle"><table><tr><td class="code"><pre><span class="line"><span class="keyword">task</span> taskX(dependsOn: <span class="string">'taskY'</span>) &lt;&lt; &#123;</span>
<span class="line">    <span class="keyword">println</span> <span class="string">'taskX'</span></span>
<span class="line">&#125;</span>
<span class="line"><span class="keyword">task</span> taskY &lt;&lt; &#123;</span>
<span class="line">    <span class="keyword">println</span> <span class="string">'taskY'</span></span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>

### 动态任务
<figure class="highlight perl"><table><tr><td class="code"><pre><span class="line"><span class="number">4</span>.<span class="keyword">times</span> &#123; counter -&gt;</span>
<span class="line">    task <span class="string">"task<span class="variable">$counter</span>"</span> &lt;&lt; &#123;</span>
<span class="line">        println <span class="string">"I'm task number <span class="variable">$counter</span>"</span></span>
<span class="line">    &#125;</span>
<span class="line">&#125;</span>
<span class="line"></span>
<span class="line">➜  gradle  gradle -<span class="keyword">q</span> task1</span>
<span class="line">I<span class="string">'m task number 1</span>
<span class="line">➜  gradle  gradle -q task2</span>
<span class="line">I'</span><span class="keyword">m</span> task number <span class="number">2</span></span>
<span class="line">➜  gradle  gradle -<span class="keyword">q</span> task3</span>
<span class="line">I<span class="string">'m task number 3</span>
<span class="line">➜  gradle  gradle -q task4</span>
<span class="line"></span>
<span class="line">FAILURE: Build failed with an exception.</span></span>
</pre></td></tr></table></figure>

### 使用已存在任务
<figure class="highlight perl"><table><tr><td class="code"><pre><span class="line"><span class="number">4</span>.<span class="keyword">times</span> &#123; counter -&gt;</span>
<span class="line">    task <span class="string">"task<span class="variable">$counter</span>"</span> &lt;&lt; &#123;</span>
<span class="line">        println <span class="string">"I'm task number <span class="variable">$counter</span>"</span></span>
<span class="line">    &#125;</span>
<span class="line">&#125;</span>
<span class="line">task<span class="number">0</span>.dependsOn task2, task3</span>
<span class="line"></span>
<span class="line">➜  gradle  gradle -<span class="keyword">q</span> task<span class="number">0</span></span>
<span class="line">I<span class="string">'m task number 2</span>
<span class="line">I'</span><span class="keyword">m</span> task number <span class="number">3</span></span>
<span class="line">I<span class="string">'m task number 0</span></span>
</pre></td></tr></table></figure>

Accessing a task via API - adding behaviour

<figure class="highlight gradle"><table><tr><td class="code"><pre><span class="line"><span class="keyword">task</span> hello &lt;&lt; &#123;</span>
<span class="line">    <span class="keyword">println</span> <span class="string">'Hello Earth'</span></span>
<span class="line">&#125;</span>
<span class="line">hello.<span class="keyword">doFirst</span> &#123;</span>
<span class="line">    <span class="keyword">println</span> <span class="string">'Hello Venus'</span></span>
<span class="line">&#125;</span>
<span class="line">hello.<span class="keyword">doLast</span> &#123;</span>
<span class="line">    <span class="keyword">println</span> <span class="string">'Hello Mars'</span></span>
<span class="line">&#125;</span>
<span class="line">hello &lt;&lt; &#123;</span>
<span class="line">    <span class="keyword">println</span> <span class="string">'Hello Jupiter'</span></span>
<span class="line">&#125;</span>
<span class="line"></span>
<span class="line">➜  gradle  gradle -q hello</span>
<span class="line">Hello Venus</span>
<span class="line">Hello Earth</span>
<span class="line">Hello Mars</span>
<span class="line">Hello Jupiter</span>
</pre></td></tr></table></figure>

_The calls doFirst and doLast can be executed multiple times. They add an action to the beginning or the end of the task’s actions list. When the task executes, the actions in the action list are executed in order. The &lt;&lt; operator is simply an alias for doLast._

### 缩写

使用属性来访问方法

<figure class="highlight gradle"><table><tr><td class="code"><pre><span class="line"><span class="keyword">task</span> hello &lt;&lt; &#123;</span>
<span class="line">    <span class="keyword">println</span> <span class="string">'Hello world!'</span></span>
<span class="line">&#125;</span>
<span class="line">hello.<span class="keyword">doLast</span> &#123;</span>
<span class="line">    <span class="keyword">println</span> <span class="string">"Greetings from the $hello.name task."</span></span>
<span class="line">&#125;</span>
<span class="line"></span>
<span class="line">➜  gradle  gradle -q hello</span>
<span class="line">Hello world!</span>
<span class="line">Greetings <span class="keyword">from</span> the hello <span class="keyword">task</span>.</span>
</pre></td></tr></table></figure>

### 扩展属性
<figure class="highlight gradle"><table><tr><td class="code"><pre><span class="line"><span class="keyword">task</span> myTask &#123;</span>
<span class="line">    ext.myProperty = <span class="string">"myValue"</span></span>
<span class="line">&#125;</span>
<span class="line"></span>
<span class="line"><span class="keyword">task</span> printTaskProperties &lt;&lt; &#123;</span>
<span class="line">    <span class="keyword">println</span> myTask.myProperty</span>
<span class="line">&#125;</span>
<span class="line"></span>
<span class="line">➜  gradle  gradle -q printTaskProperties</span>
<span class="line">myValue</span>
</pre></td></tr></table></figure>

### 使用Ant任务

遍历文件夹并打印文件名和属性

<figure class="highlight livecodeserver"><table><tr><td class="code"><pre><span class="line">task loadfile &lt;&lt; &#123;</span>
<span class="line">    def <span class="built_in">files</span> = <span class="built_in">file</span>(<span class="string">'t'</span>).listFiles().<span class="built_in">sort</span>()</span>
<span class="line">    <span class="built_in">files</span>.<span class="keyword">each</span> &#123; File <span class="built_in">file</span> -&gt;</span>
<span class="line">        <span class="keyword">if</span> (<span class="built_in">file</span>.isFile()) &#123;</span>
<span class="line">            ant.loadfile(srcFile: <span class="built_in">file</span>, property: <span class="built_in">file</span>.name)</span>
<span class="line">            println <span class="string">" *** $file.name ***"</span></span>
<span class="line">            println <span class="string">"$&#123;ant.properties[file.name]&#125;"</span></span>
<span class="line">        &#125;</span>
<span class="line">    &#125;</span>
<span class="line">&#125;</span>
<span class="line"></span>
<span class="line">➜  gradle  mkdir t</span>
<span class="line">➜  gradle  touch <span class="keyword">text</span></span>
<span class="line">➜  gradle  vim t/<span class="keyword">text</span></span>
<span class="line">...</span>
<span class="line">➜  gradle  gradle -q loadfile</span>
<span class="line"> *** <span class="keyword">text</span> ***</span>
<span class="line">Using AntBuilder <span class="built_in">to</span> execute ant.loadfile target</span>
</pre></td></tr></table></figure>

### 使用方法
<figure class="highlight sql"><table><tr><td class="code"><pre><span class="line">task <span class="operator"><span class="keyword">checksum</span> &lt;&lt; &#123;</span>
<span class="line">    fileList(<span class="string">'../antLoadfileResources'</span>).<span class="keyword">each</span> &#123;<span class="keyword">File</span> <span class="keyword">file</span> -&gt;</span>
<span class="line">        ant.<span class="keyword">checksum</span>(<span class="keyword">file</span>: <span class="keyword">file</span>, property: <span class="string">"cs_$file.name"</span>)</span>
<span class="line">        println <span class="string">"$file.name Checksum: $&#123;ant.properties["</span>cs_$<span class="keyword">file</span>.<span class="keyword">name</span><span class="string">"]&#125;"</span></span>
<span class="line">    &#125;</span>
<span class="line">&#125;</span>
<span class="line"></span>
<span class="line">task loadfile &lt;&lt; &#123;</span>
<span class="line">    fileList(<span class="string">'../antLoadfileResources'</span>).<span class="keyword">each</span> &#123;<span class="keyword">File</span> <span class="keyword">file</span> -&gt;</span>
<span class="line">        ant.loadfile(srcFile: <span class="keyword">file</span>, property: <span class="keyword">file</span>.<span class="keyword">name</span>)</span>
<span class="line">        println <span class="string">"I'm fond of $file.name"</span></span>
<span class="line">    &#125;</span>
<span class="line">&#125;</span>
<span class="line"></span>
<span class="line"><span class="keyword">File</span>[] fileList(<span class="keyword">String</span> dir) &#123;</span>
<span class="line">    <span class="keyword">file</span>(dir).listFiles(&#123;<span class="keyword">file</span> -&gt; <span class="keyword">file</span>.isFile() &#125; <span class="keyword">as</span> FileFilter).<span class="keyword">sort</span>()</span>
<span class="line">&#125;</span>
<span class="line"></span>
<span class="line"></span>
<span class="line">➜  gradle  gradle -q loadfile</span>
<span class="line"><span class="keyword">I</span><span class="string">'m fond of ab</span>
<span class="line">I'</span><span class="keyword">m</span> fond <span class="keyword">of</span> <span class="built_in">text</span></span>
<span class="line"></span>
<span class="line">➜  gradle  gradle -q <span class="keyword">checksum</span></span>
<span class="line">ab <span class="keyword">Checksum</span>: <span class="number">03862</span>a037618f9566ac4d6cbdb5dd075</span>
<span class="line"><span class="built_in">text</span> <span class="keyword">Checksum</span>: <span class="number">865943</span>c62f297e63f6a381350c9dc0e1</span></span>
</pre></td></tr></table></figure>

### 默认任务
<figure class="highlight gradle"><table><tr><td class="code"><pre><span class="line">defaultTasks <span class="string">'clean'</span>, <span class="string">'run'</span></span>
<span class="line"></span>
<span class="line"><span class="keyword">task</span> clean &lt;&lt; &#123;</span>
<span class="line">    <span class="keyword">println</span> <span class="string">'Default Cleaning!'</span></span>
<span class="line">&#125;</span>
<span class="line"></span>
<span class="line"><span class="keyword">task</span> run &lt;&lt; &#123;</span>
<span class="line">    <span class="keyword">println</span> <span class="string">'Default Running!'</span></span>
<span class="line">&#125;</span>
<span class="line"></span>
<span class="line"><span class="keyword">task</span> other &lt;&lt; &#123;</span>
<span class="line">    <span class="keyword">println</span> <span class="string">"I'm not a default task!"</span></span>
<span class="line">&#125;</span>
<span class="line"></span>
<span class="line">➜  gradle  gradle -q</span>
<span class="line"><span class="keyword">Default</span> Cleaning!</span>
<span class="line"><span class="keyword">Default</span> Running!</span>
</pre></td></tr></table></figure>

### 配置DAG
<figure class="highlight livecodeserver"><table><tr><td class="code"><pre><span class="line">task distribution &lt;&lt; &#123;</span>
<span class="line">    println <span class="string">"We build the zip with version=$version"</span></span>
<span class="line">&#125;</span>
<span class="line"></span>
<span class="line">task release(dependsOn: <span class="string">'distribution'</span>) &lt;&lt; &#123;</span>
<span class="line">    println <span class="string">'We release now'</span></span>
<span class="line">&#125;</span>
<span class="line"></span>
<span class="line">gradle.taskGraph.whenReady &#123;taskGraph -&gt;</span>
<span class="line">    <span class="keyword">if</span> (taskGraph.hasTask(release)) &#123;</span>
<span class="line">        <span class="built_in">version</span> = <span class="string">'1.0'</span></span>
<span class="line">    &#125; <span class="keyword">else</span> &#123;</span>
<span class="line">        <span class="built_in">version</span> = <span class="string">'1.0-SNAPSHOT'</span></span>
<span class="line">    &#125;</span>
<span class="line">&#125;</span>
<span class="line"></span>
<span class="line">➜  gradle  gradle -q distribution</span>
<span class="line">We build <span class="operator">the</span> zip <span class="operator">with</span> <span class="built_in">version</span>=<span class="number">1.0</span>-SNAPSHOT</span>
<span class="line">➜  gradle  gradle -q release</span>
<span class="line">We build <span class="operator">the</span> zip <span class="operator">with</span> <span class="built_in">version</span>=<span class="number">1.0</span></span>
<span class="line">We release now</span>
</pre></td></tr></table></figure>