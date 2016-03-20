---
title: Logger - Android日志输出库
tags:
  - Android
date: 2015-12-19 12:55:15
---

### Logger
> 原文地址： [https://github.com/orhanobut/logger](https://github.com/orhanobut/logger)

#### 简介

Simple, pretty and powerful logger for android

Logger provides :

*   Thread information
*   Class information
*   Method information
*   Pretty-print for json content
*   Pretty-print for new line “\n”
*   Clean output
*   Jump to source

#### 依赖

gradle 导入

<figure class="highlight gradle"><table><tr><td class="code"><pre><span class="line"><span class="keyword">repositories</span> &#123;</span>
<span class="line">  <span class="comment">// ...</span></span>
<span class="line">  maven &#123; url <span class="string">"https://jitpack.io"</span> &#125;</span>
<span class="line">&#125;</span>
<span class="line"></span>
<span class="line"><span class="keyword">dependencies</span> &#123;</span>
<span class="line">  <span class="keyword">compile</span> <span class="string">'com.github.orhanobut:logger:1.12'</span></span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>

#### 使用

##### 初始化

_在程序初始化时候实现以下代码（Application，或者LAUNCHER Activity中）_

<figure class="highlight haskell"><table><tr><td class="code"><pre><span class="line"><span class="type">Logger</span></span>
<span class="line">  .init(<span class="type">YOUR_TAG</span>)                 // <span class="default"><span class="keyword">default</span> <span class="type">PRETTYLOGGER</span> or use just init<span class="container">()</span></span></span>
<span class="line">  .methodCount(<span class="number">3</span>)                 // <span class="default"><span class="keyword">default</span> 2</span></span>
<span class="line">  .hideThreadInfo()               // <span class="default"><span class="keyword">default</span> shown</span></span>
<span class="line">  .logLevel(<span class="type">LogLevel</span>.<span class="type">NONE</span>)        // <span class="default"><span class="keyword">default</span> <span class="type">LogLevel</span>.<span class="type">FULL</span></span></span>
<span class="line">  .methodOffset(<span class="number">2</span>)                // <span class="default"><span class="keyword">default</span> 0</span></span>
<span class="line">  .logTool(new <span class="type">AndroidLogTool</span>()); // custom log tool, optional</span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>

##### 支持格式
<figure class="highlight stata"><table><tr><td class="code"><pre><span class="line">Logger.<span class="literal">d</span>(<span class="string">"hello"</span>);</span>
<span class="line">Logger.<span class="literal">e</span>(<span class="string">"hello"</span>);</span>
<span class="line">Logger.<span class="literal">w</span>(<span class="string">"hello"</span>);</span>
<span class="line">Logger.v(<span class="string">"hello"</span>);</span>
<span class="line">Logger.wtf(<span class="string">"hello"</span>);</span>
<span class="line">Logger.json(JSON_CONTENT);</span>
<span class="line">Logger.xml(XML_CONTENT);</span>
</pre></td></tr></table></figure>

##### 举个栗子
<figure class="highlight stata"><table><tr><td class="code"><pre><span class="line">Logger.<span class="literal">d</span>(<span class="string">"hello"</span>);</span>
<span class="line">Logger.<span class="literal">e</span>(exception, <span class="string">"message"</span>);</span>
<span class="line">Logger.json(JSON_CONTENT);</span>
</pre></td></tr></table></figure>

![](https://raw.githubusercontent.com/orhanobut/logger/master/images/logger-log.png)