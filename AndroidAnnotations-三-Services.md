---
title: AndroidAnnotations (三) - Services
tags:
  - Android
  - AndroidAnnotations
date: 2015-11-24 15:25:04
---

### Service

**声明EService注解**

<figure class="highlight scala"><table><tr><td class="code"><pre><span class="line"><span class="annotation">@EService</span></span>
<span class="line">public <span class="class"><span class="keyword">class</span> <span class="title">MyService</span> <span class="keyword"><span class="keyword">extends</span></span> <span class="title">IntentService</span> &#123;</span></span>
<span class="line"></span>
<span class="line">    <span class="comment">/**</span>
<span class="line">     * Creates an IntentService.  Invoked by your subclass's constructor.</span>
<span class="line">     */</span></span>
<span class="line">    public <span class="type">MyService</span>() &#123;</span>
<span class="line">        <span class="keyword">super</span>(<span class="type">MyService</span>.<span class="keyword">class</span>.getSimpleName());</span>
<span class="line">    &#125;</span>
<span class="line"></span>
<span class="line">    <span class="annotation">@Override</span></span>
<span class="line">    <span class="keyword">protected</span> void onHandleIntent(<span class="type">Intent</span> intent) &#123;</span>
<span class="line">        showToast();</span>
<span class="line">    &#125;</span>
<span class="line"></span>
<span class="line">    <span class="annotation">@UiThread</span> <span class="comment">//在UI主线程运行</span></span>
<span class="line">    void showToast() &#123;</span>
<span class="line">        <span class="type">Toast</span>.makeText(getApplicationContext(), <span class="string">"Hello World!"</span>, <span class="type">Toast</span>.<span class="type">LENGTH_LONG</span>).show();</span>
<span class="line">    &#125;</span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>

**启动Service：**

<figure class="highlight css"><table><tr><td class="code"><pre><span class="line"><span class="tag">MyService_</span><span class="class">.intent</span>(<span class="tag">getApplication</span>())<span class="class">.start</span>();</span>
</pre></td></tr></table></figure>

**停止Service：**

<figure class="highlight css"><table><tr><td class="code"><pre><span class="line"><span class="tag">MyService_</span><span class="class">.intent</span>(<span class="tag">getApplication</span>())<span class="class">.stop</span>();</span>
</pre></td></tr></table></figure>

**IntentServices：**

*   创建IntentSerive
<figure class="highlight java"><table><tr><td class="code"><pre><span class="line"><span class="annotation">@EIntentService</span></span>
<span class="line"><span class="keyword">public</span> <span class="class"><span class="keyword">class</span> <span class="title">MyIntentService</span> <span class="keyword">extends</span> <span class="title">IntentService</span> </span>&#123;</span>
<span class="line"></span>
<span class="line">    <span class="function"><span class="keyword">public</span> <span class="title">MyIntentService</span><span class="params">()</span> </span>&#123;</span>
<span class="line">        <span class="keyword">super</span>(<span class="string">"MyIntentService"</span>);</span>
<span class="line">    &#125;</span>
<span class="line"></span>
<span class="line">    <span class="annotation">@ServiceAction</span> </span>
<span class="line">    <span class="function"><span class="keyword">void</span> <span class="title">mySimpleAction</span><span class="params">()</span> </span>&#123;</span>
<span class="line">        <span class="comment">// ...</span></span>
<span class="line">    &#125;</span>
<span class="line"></span>
<span class="line">    <span class="annotation">@ServiceAction</span></span>
<span class="line">    <span class="function"><span class="keyword">void</span> <span class="title">myAction</span><span class="params">(String param)</span> </span>&#123;</span>
<span class="line">        <span class="comment">// ...</span></span>
<span class="line">    &#125;</span>
<span class="line"></span>
<span class="line">    <span class="annotation">@Override</span></span>
<span class="line">    <span class="function"><span class="keyword">protected</span> <span class="keyword">void</span> <span class="title">onHandleIntent</span><span class="params">(Intent intent)</span> </span>&#123;</span>
<span class="line">        <span class="comment">// Do nothing here</span></span>
<span class="line">    &#125;</span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>

*   带自定义方法启动IntentService
<figure class="highlight 1c"><table><tr><td class="code"><pre><span class="line">MyIntentService_.intent(getApplication()) <span class="comment">//</span></span>
<span class="line">    .myAction(<span class="string">"test"</span>) <span class="comment">//对应 void myAction(String param)</span></span>
<span class="line">    .start();</span>
</pre></td></tr></table></figure>

_如果你调用多个Action,只有最后的会被执行。_

* * *

> AndroidAnnotations 3.3版本后新增功能

Note： 由于IntentService # onHandleIntent是抽象方法, 所以必须添加一个空的实现. 为了方便起见,可以继承AbstractIntentService类，代码里就可以忽略此方法.

<figure class="highlight scala"><table><tr><td class="code"><pre><span class="line"><span class="annotation">@EService</span></span>
<span class="line">public <span class="class"><span class="keyword">class</span> <span class="title">MyService</span> <span class="keyword"><span class="keyword">extends</span></span> <span class="title">AbstractIntentService</span> &#123;</span></span>
<span class="line"></span>
<span class="line">    <span class="comment">/**</span>
<span class="line">     * Creates an IntentService.  Invoked by your subclass's constructor.</span>
<span class="line">     */</span></span>
<span class="line">    public <span class="type">MyService</span>() &#123;</span>
<span class="line">        <span class="keyword">super</span>(<span class="type">MyService</span>.<span class="keyword">class</span>.getSimpleName());</span>
<span class="line">    &#125;</span>
<span class="line"></span>
<span class="line">    <span class="annotation">@UiThread</span></span>
<span class="line">    void showToast() &#123;</span>
<span class="line">        <span class="type">Toast</span>.makeText(getApplicationContext(), <span class="string">"Hello World!"</span>, <span class="type">Toast</span>.<span class="type">LENGTH_LONG</span>).show();</span>
<span class="line">    &#125;</span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>