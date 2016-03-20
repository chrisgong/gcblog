---
title: AndroidAnnotations (六) - Resources
tags:
  - Android
  - AndroidAnnotations
date: 2015-11-24 22:51:50
---

> 对应Android本身各种资源标签，没什么探究的了，实在记不住用得时候查一下就好了。

All @XXXRes annotations indicate that an activity field should be injected with the corresponding Android resource from your res folder. The resource id can be set in the annotation parameter, ie @StringRes(R.string.hello).

### @StringRes

* * *

<figure class="highlight scala"><table><tr><td class="code"><pre><span class="line"><span class="annotation">@EActivity</span></span>
<span class="line">public <span class="class"><span class="keyword">class</span> <span class="title">MyActivity</span> <span class="keyword"><span class="keyword">extends</span></span> <span class="title">Activity</span> &#123;</span></span>
<span class="line"></span>
<span class="line">  <span class="annotation">@StringRes</span>(<span class="type">R</span>.string.hello)</span>
<span class="line">  <span class="type">String</span> myHelloString;</span>
<span class="line"></span>
<span class="line">  <span class="annotation">@StringRes</span></span>
<span class="line">  <span class="type">String</span> hello;</span>
<span class="line"></span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>

### @ColorRes

* * *

<figure class="highlight scala"><table><tr><td class="code"><pre><span class="line"><span class="annotation">@EActivity</span></span>
<span class="line">public <span class="class"><span class="keyword">class</span> <span class="title">MyActivity</span> <span class="keyword"><span class="keyword">extends</span></span> <span class="title">Activity</span> &#123;</span></span>
<span class="line"></span>
<span class="line">  <span class="annotation">@ColorRes</span>(<span class="type">R</span>.color.backgroundColor)</span>
<span class="line">  int someColor;</span>
<span class="line"></span>
<span class="line">  <span class="annotation">@ColorRes</span></span>
<span class="line">  int backgroundColor;</span>
<span class="line"></span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>

### @AnimationRes

* * *

<figure class="highlight scala"><table><tr><td class="code"><pre><span class="line"><span class="annotation">@EActivity</span></span>
<span class="line">public <span class="class"><span class="keyword">class</span> <span class="title">MyActivity</span> <span class="keyword"><span class="keyword">extends</span></span> <span class="title">Activity</span> &#123;</span></span>
<span class="line"></span>
<span class="line">  <span class="annotation">@AnimationRes</span>(<span class="type">R</span>.anim.fadein)</span>
<span class="line">  <span class="type">XmlResourceParser</span> xmlResAnim;</span>
<span class="line"></span>
<span class="line">  <span class="annotation">@AnimationRes</span></span>
<span class="line">  <span class="type">Animation</span> fadein;</span>
<span class="line"></span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>

### @DimensionRes

* * *

<figure class="highlight scala"><table><tr><td class="code"><pre><span class="line"><span class="annotation">@EActivity</span></span>
<span class="line">public <span class="class"><span class="keyword">class</span> <span class="title">MyActivity</span> <span class="keyword"><span class="keyword">extends</span></span> <span class="title">Activity</span> &#123;</span></span>
<span class="line"></span>
<span class="line">  <span class="annotation">@DimensionRes</span>(<span class="type">R</span>.dimen.fontsize)</span>
<span class="line">  float fontSizeDimension;</span>
<span class="line"></span>
<span class="line">  <span class="annotation">@DimensionRes</span></span>
<span class="line">  float fontsize;</span>
<span class="line"></span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>

### @DimensionPixelOffsetRes

* * *

<figure class="highlight scala"><table><tr><td class="code"><pre><span class="line"><span class="annotation">@EActivity</span></span>
<span class="line">public <span class="class"><span class="keyword">class</span> <span class="title">MyActivity</span> <span class="keyword"><span class="keyword">extends</span></span> <span class="title">Activity</span> &#123;</span></span>
<span class="line"></span>
<span class="line">  <span class="annotation">@DimensionPixelOffsetRes</span>(<span class="type">R</span>.string.fontsize)</span>
<span class="line">  int fontSizeDimension;</span>
<span class="line"></span>
<span class="line">  <span class="annotation">@DimensionPixelOffsetRes</span></span>
<span class="line">  int fontsize;</span>
<span class="line"></span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>

### @DimensionPixelSizeRes

* * *

<figure class="highlight scala"><table><tr><td class="code"><pre><span class="line"><span class="annotation">@EActivity</span></span>
<span class="line">public <span class="class"><span class="keyword">class</span> <span class="title">MyActivity</span> <span class="keyword"><span class="keyword">extends</span></span> <span class="title">Activity</span> &#123;</span></span>
<span class="line"></span>
<span class="line">  <span class="annotation">@DimensionPixelSizeRes</span>(<span class="type">R</span>.string.fontsize)</span>
<span class="line">  int fontSizeDimension;</span>
<span class="line"></span>
<span class="line">  <span class="annotation">@DimensionPixelSizeRes</span></span>
<span class="line">  int fontsize;</span>
<span class="line"></span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>

### Other @XXXRes

* * *

Here is the list of other supported resource annotations:

*   `@BooleanRes`
*   `@ColorStateListRes`
*   `@DrawableRes`
*   `@IntArrayRes`
*   `@IntegerRes`
*   `@LayoutRes`
*   `@MovieRes`
*   `@TextRes`
*   `@TextArrayRes`
*   `@StringArrayRes`