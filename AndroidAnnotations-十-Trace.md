---
title: AndroidAnnotations (十) - Trace
tags:
  - Android
  - AndroidAnnotations
date: 2015-11-25 09:54:02
---

### Trace

The @Trace annotation allows you to trace the execution of a method by writing log entries.

The method must not be private.

Usage examples:

<figure class="highlight less"><table><tr><td class="code"><pre><span class="line"><span class="variable">@Trace</span></span>
<span class="line">void <span class="function">doWork</span>() &#123;</span>
<span class="line">    <span class="comment">// ... Do Work ...</span></span>
<span class="line">&#125;</span>
<span class="line"></span>
<span class="line"><span class="variable">@Trace</span></span>
<span class="line">boolean <span class="function">doMoreWork</span>(String someString) &#123;</span>
<span class="line">    <span class="comment">// ... Do more Work ...</span></span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>

Will generate the following log entries for `doWork()`:

<figure class="highlight stylus"><table><tr><td class="code"><pre><span class="line">I/<span class="function"><span class="title">TracedMethodActivity</span><span class="params">(  <span class="number">302</span>)</span></span>: Entering [void <span class="function"><span class="title">doWork</span><span class="params">()</span></span> ]</span>
<span class="line">I/<span class="function"><span class="title">TracedMethodActivity</span><span class="params">(  <span class="number">302</span>)</span></span>: Exiting [void <span class="function"><span class="title">doWork</span><span class="params">()</span></span> ], duration <span class="keyword">in</span> ms: <span class="number">1002</span></span>
</pre></td></tr></table></figure>

And this log entries for `doMoreWork()`:

> Since AndroidAnnotations 3.1
<figure class="highlight stylus"><table><tr><td class="code"><pre><span class="line">I/<span class="function"><span class="title">TracedMethodActivity</span><span class="params">(  <span class="number">302</span>)</span></span>: Entering [boolean <span class="function"><span class="title">doMoreWork</span><span class="params">(someString = Hello World)</span></span>]</span>
<span class="line">I/<span class="function"><span class="title">TracedMethodActivity</span><span class="params">(  <span class="number">302</span>)</span></span>: Exiting [boolean <span class="function"><span class="title">doMoreWork</span><span class="params">(String)</span></span> returning: true], duration <span class="keyword">in</span> ms: <span class="number">651</span></span>
</pre></td></tr></table></figure>

Customization:

<figure class="highlight aspectj"><table><tr><td class="code"><pre><span class="line"><span class="annotation">@Trace</span>(tag=<span class="string">"CustomTag"</span>, level=Log.WARN)</span>
<span class="line"><span class="function"><span class="keyword">void</span> <span class="title">doWork</span><span class="params">()</span> </span>&#123;</span>
<span class="line">    <span class="comment">// ... Do Work ...</span></span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>