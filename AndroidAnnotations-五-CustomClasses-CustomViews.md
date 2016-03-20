---
title: AndroidAnnotations (五) - CustomClasses/CustomViews
tags:
  - Android
  - AndroidAnnotations
date: 2015-11-25 07:39:35
---

### Custom Classes

#### 定义注解classes
<figure class="highlight groovy"><table><tr><td class="code"><pre><span class="line"><span class="annotation">@EBean</span></span>
<span class="line"><span class="keyword">public</span> <span class="class"><span class="keyword">class</span> <span class="title">MyClass</span> &#123;</span></span>
<span class="line"></span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>
> @EBean类只能有一个构造函数，并且这个构造函数必须没有参数,
> AndroidAnnotations 2.7以后它可以有一个Context参数。

#### 使用注解classes
<figure class="highlight groovy"><table><tr><td class="code"><pre><span class="line"><span class="annotation">@EBean</span></span>
<span class="line"><span class="keyword">public</span> <span class="class"><span class="keyword">class</span> <span class="title">MyOtherClass</span> &#123;</span></span>
<span class="line"></span>
<span class="line">	<span class="annotation">@Bean</span></span>
<span class="line">	MyClass myClass;</span>
<span class="line"></span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>
<figure class="highlight scala"><table><tr><td class="code"><pre><span class="line"><span class="annotation">@EActivity</span></span>
<span class="line">public <span class="class"><span class="keyword">class</span> <span class="title">MyActivity</span> <span class="keyword"><span class="keyword">extends</span></span> <span class="title">Activity</span> &#123;</span></span>
<span class="line"></span>
<span class="line">	<span class="annotation">@Bean</span></span>
<span class="line">	<span class="type">MyOtherClass</span> myOtherClass;</span>
<span class="line">	</span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>
> 当你用@Bean时就会得到一个新实例，bean是一个单例除外
<figure class="highlight scala"><table><tr><td class="code"><pre><span class="line"><span class="annotation">@EActivity</span></span>
<span class="line">public <span class="class"><span class="keyword">class</span> <span class="title">MyChildActivity</span> <span class="keyword"><span class="keyword">extends</span></span> <span class="title">MyActivity</span> &#123;</span></span>
<span class="line">	<span class="annotation">@Override</span></span>
<span class="line">	<span class="keyword">protected</span> void onCreate(<span class="type">Bundle</span> savedInstanceState) &#123;</span>
<span class="line">		<span class="keyword">super</span>.onCreate(savedInstanceState);</span>
<span class="line">		myOtherClass.myClass.toString();</span>
<span class="line">	&#125;</span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>

#### 使用实现关系classes
> Since AndroidAnnotations 2.5

如果你想在代码中使用超类或接口，您可以再@Bean内指定实现类。

<figure class="highlight actionscript"><table><tr><td class="code"><pre><span class="line"><span class="keyword">public</span> <span class="class"><span class="keyword">class</span> <span class="title">MyInterface</span> </span>&#123;</span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>
<figure class="highlight scala"><table><tr><td class="code"><pre><span class="line">public <span class="class"><span class="keyword">class</span> <span class="title">MyImplementation</span> <span class="keyword"><span class="keyword">extends</span></span> <span class="title">MyInterface</span> &#123;</span></span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>
<figure class="highlight scala"><table><tr><td class="code"><pre><span class="line"><span class="annotation">@EActivity</span></span>
<span class="line">public <span class="class"><span class="keyword">class</span> <span class="title">OtherActivity</span> <span class="keyword"><span class="keyword">extends</span></span> <span class="title">Activity</span> &#123;</span></span>
<span class="line">    <span class="annotation">@Bean</span>(<span class="type">MyImplementation</span>.<span class="keyword">class</span>)</span>
<span class="line">    <span class="type">MyInterface</span> myInterface;</span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>

#### 支持的annotations

可以使用所有AA 支持的的annotations

<figure class="highlight java"><table><tr><td class="code"><pre><span class="line"><span class="annotation">@EBean</span></span>
<span class="line"><span class="keyword">public</span> <span class="class"><span class="keyword">class</span> <span class="title">MyClass</span> </span>&#123;</span>
<span class="line">	<span class="annotation">@SystemService</span></span>
<span class="line">	NotificationManager notificationManager;</span>
<span class="line"></span>
<span class="line">	<span class="annotation">@UiThread</span></span>
<span class="line">	<span class="function"><span class="keyword">void</span> <span class="title">updateUI</span><span class="params">()</span> </span>&#123;</span>
<span class="line"></span>
<span class="line">	&#125;</span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>

#### 视图相关annotations

可以使用@View，@Click…等

<figure class="highlight java"><table><tr><td class="code"><pre><span class="line"><span class="annotation">@EBean</span></span>
<span class="line"><span class="keyword">public</span> <span class="class"><span class="keyword">class</span> <span class="title">MyClass</span> </span>&#123;</span>
<span class="line">	<span class="annotation">@ViewById</span></span>
<span class="line">	TextView myTextView;</span>
<span class="line"></span>
<span class="line">	<span class="annotation">@Click</span>(R.id.myButton)</span>
<span class="line">	<span class="function"><span class="keyword">void</span> <span class="title">handleButtonClick</span><span class="params">()</span> </span>&#123;</span>
<span class="line"></span>
<span class="line">	&#125;</span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>

#### RootContext

可以使用@RootContext在@EBean class里注解Android组件，在对应的组件内实例此Bean时，不属于此组件的RootContext将为null。

<figure class="highlight less"><table><tr><td class="code"><pre><span class="line"><span class="variable">@EBean</span></span>
<span class="line">public class MyClass &#123;</span>
<span class="line">	<span class="variable">@RootContext</span></span>
<span class="line">	Context context;</span>
<span class="line"></span>
<span class="line">	<span class="comment">// Only injected if the root context is an activity</span></span>
<span class="line">	<span class="variable">@RootContext</span></span>
<span class="line">	Activity activity;</span>
<span class="line"></span>
<span class="line">	<span class="comment">// Only injected if the root context is a service</span></span>
<span class="line">	<span class="variable">@RootContext</span></span>
<span class="line">	Service service;</span>
<span class="line"></span>
<span class="line">	<span class="comment">// Only injected if the root context is an instance of MyActivity</span></span>
<span class="line">	<span class="variable">@RootContext</span></span>
<span class="line">	MyActivity myActivity;</span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>
<figure class="highlight cpp"><table><tr><td class="code"><pre><span class="line">@EActivity(R.layout.layout_other)</span>
<span class="line"><span class="keyword">public</span> <span class="keyword">class</span> OtherActivity extends Activity &#123;</span>
<span class="line"></span>
<span class="line">    @Bean</span>
<span class="line">    MyClass myClass;</span>
<span class="line"></span>
<span class="line">    @<span class="function">Override</span>
<span class="line">    <span class="keyword">protected</span> <span class="keyword">void</span> <span class="title">onCreate</span><span class="params">(Bundle savredInstanceState)</span> </span>&#123;</span>
<span class="line">        super.onCreate(savredInstanceState);</span>
<span class="line">        </span>
<span class="line">        Log.e(<span class="string">"cim"</span>,<span class="string">""</span> + (myClass.activity == null));</span>
<span class="line"></span>
<span class="line">        Log.e(<span class="string">"cim"</span>,<span class="string">""</span> + (myClass.service == null));</span>
<span class="line"></span>
<span class="line">        Log.e(<span class="string">"cim"</span>,<span class="string">""</span> + (myClass.context == null));</span>
<span class="line"></span>
<span class="line">        Log.e(<span class="string">"cim"</span>,<span class="string">""</span> + (myClass.otherActivity == null));</span>
<span class="line">    &#125;</span>
<span class="line">&#125;</span>
<span class="line"></span>
<span class="line">\\Result</span>
<span class="line"><span class="number">11</span>-<span class="number">24</span> <span class="number">08</span>:<span class="number">24</span>:<span class="number">39.303</span> <span class="number">2279</span>-<span class="number">2279</span>/? E/cim: <span class="literal">false</span></span>
<span class="line"><span class="number">11</span>-<span class="number">24</span> <span class="number">08</span>:<span class="number">24</span>:<span class="number">39.303</span> <span class="number">2279</span>-<span class="number">2279</span>/? E/cim: <span class="literal">true</span></span>
<span class="line"><span class="number">11</span>-<span class="number">24</span> <span class="number">08</span>:<span class="number">24</span>:<span class="number">39.303</span> <span class="number">2279</span>-<span class="number">2279</span>/? E/cim: <span class="literal">false</span></span>
<span class="line"><span class="number">11</span>-<span class="number">24</span> <span class="number">08</span>:<span class="number">24</span>:<span class="number">39.303</span> <span class="number">2279</span>-<span class="number">2279</span>/? E/cim: <span class="literal">false</span></span>
</pre></td></tr></table></figure>

#### Executing code after dependency injection
<figure class="highlight cpp"><table><tr><td class="code"><pre><span class="line">@EBean</span>
<span class="line"><span class="keyword">public</span> <span class="keyword">class</span> MyClass &#123;</span>
<span class="line"></span>
<span class="line">    @SystemService</span>
<span class="line">    NotificationManager notificationManager;</span>
<span class="line"></span>
<span class="line">    @Bean</span>
<span class="line">    MyOtherClass dependency;</span>
<span class="line"></span>
<span class="line">    <span class="function"><span class="keyword">public</span> <span class="title">MyClass</span><span class="params">()</span> </span>&#123;</span>
<span class="line">        <span class="comment">// notificationManager and dependency are null</span></span>
<span class="line">        Log.e(<span class="string">"cim"</span>,<span class="string">""</span> + (dependency == null));</span>
<span class="line">        Log.e(<span class="string">"cim"</span>, <span class="string">""</span> + (notificationManager == null));</span>
<span class="line">    &#125;</span>
<span class="line"></span>
<span class="line">    @<span class="function">AfterInject</span>
<span class="line">    <span class="keyword">public</span> <span class="keyword">void</span> <span class="title">doSomethingAfterInjection</span><span class="params">()</span> </span>&#123;</span>
<span class="line">        <span class="comment">// notificationManager and dependency are set</span></span>
<span class="line">        Log.e(<span class="string">"cim"</span>,<span class="string">""</span> + (dependency == null));</span>
<span class="line">        Log.e(<span class="string">"cim"</span>, <span class="string">""</span> + (notificationManager == null));</span>
<span class="line">    &#125;</span>
<span class="line">&#125;</span>
<span class="line"></span>
<span class="line">Result:</span>
<span class="line"><span class="number">11</span>-<span class="number">24</span> <span class="number">08</span>:<span class="number">43</span>:<span class="number">04.980</span> <span class="number">3138</span>-<span class="number">3138</span>/? E/cim: <span class="literal">true</span></span>
<span class="line"><span class="number">11</span>-<span class="number">24</span> <span class="number">08</span>:<span class="number">43</span>:<span class="number">04.980</span> <span class="number">3138</span>-<span class="number">3138</span>/? E/cim: <span class="literal">true</span></span>
<span class="line"><span class="number">11</span>-<span class="number">24</span> <span class="number">08</span>:<span class="number">43</span>:<span class="number">04.980</span> <span class="number">3138</span>-<span class="number">3138</span>/? E/cim: <span class="literal">false</span></span>
<span class="line"><span class="number">11</span>-<span class="number">24</span> <span class="number">08</span>:<span class="number">43</span>:<span class="number">04.980</span> <span class="number">3138</span>-<span class="number">3138</span>/? E/cim: <span class="literal">false</span></span>
</pre></td></tr></table></figure>

Bean中当构造函数被调用时，所有注解对象都为null，此时对象没还没有被注入。如果你需要在构建Bean时执行某些代码，应该使用@AfterInject注解一个方法，在此方法内执行。

#### Warning
> If the parent and child classes have @AfterViews, @AfterInject or @AfterExtras annotated methods with the same name, the generated code will be buggy. See [issue #591](https://github.com/excilys/androidannotations/issues/591) for more details.

* * *

> Also, while there is a guaranteed order about when we call @AfterViews, -Inject or -Extras annotated methods, there is no guaranteed order for calling each of the methods with the same @AfterXXX annotation (see [issue #810](https://github.com/excilys/androidannotations/issues/810)).
> 
> Details about when the methods with one of those annotations are called you can find [here](https://github.com/excilys/androidannotations/wiki/%40AfterXXX-call-order).

#### 作用域
> Since AndroidAnnotations 2.5

AndroidAnnotations目前支持两种作用域:

*   默认范围:每次创建一个新的bean实例时必须被injected。
*   单例对象范围:新的bean实例第一次创建被injected，然后保留，之后总是注入相同的实例。
<figure class="highlight d"><table><tr><td class="code"><pre><span class="line"><span class="keyword">@EBean</span>(<span class="keyword">scope</span> = Scope.Singleton)</span>
<span class="line"><span class="keyword">public</span> <span class="keyword">class</span> MySingleton &#123;</span>
<span class="line"></span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>
> **Caution**
> 
> 1.  If your bean has a Singleton scope, it will only keep a reference to the application context (using context.getApplicationContext(). That’s because it lives longer than any activity / service, so it shouldn’t keep a reference to any such Android component, to avoid memory leaks.
> 
> 2.  We also disable view injection and view event binding in @EBean components with Singleton scope, for the exact same reason (views have a reference to the activity, which may therefore create a leak).

### CustomView
> 翻译了半天，发现还是原文能更准确的传达意思，所以后面都不翻译了。Youdao，Goodbye~

@EView and @EViewGroup are the annotations to use if you want to create [custom components](http://developer.android.com/guide/topics/ui/custom-components.html).

#### Custom Views with @EView
> Since AndroidAnnotations 2.4

Just create a new class that extends View, and annotate it with @EView. You can than start using annotations in this view:

<figure class="highlight scala"><table><tr><td class="code"><pre><span class="line"><span class="annotation">@EView</span></span>
<span class="line">public <span class="class"><span class="keyword">class</span> <span class="title">CustomButton</span> <span class="keyword"><span class="keyword">extends</span></span> <span class="title">Button</span> &#123;</span></span>
<span class="line"></span>
<span class="line">	<span class="annotation">@App</span></span>
<span class="line">	<span class="type">MyApplication</span> application;</span>
<span class="line"></span>
<span class="line">	<span class="annotation">@StringRes</span></span>
<span class="line">	<span class="type">String</span> someStringResource;</span>
<span class="line"></span>
<span class="line">	public <span class="type">CustomButton</span>(<span class="type">Context</span> context, <span class="type">AttributeSet</span> attrs) &#123;</span>
<span class="line">		<span class="keyword">super</span>(context, attrs);</span>
<span class="line">	&#125;</span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>

You can then start using it in your layouts (don’t forget the _):

<figure class="highlight xml"><table><tr><td class="code"><pre><span class="line"><span class="pi">&lt;?xml version="1.0" encoding="utf-8"?&gt;</span></span>
<span class="line"><span class="tag">&lt;<span class="title">LinearLayout</span> <span class="attribute">xmlns:android</span>=<span class="value">"http://schemas.android.com/apk/res/android"</span></span>
<span class="line">    <span class="attribute">android:layout_width</span>=<span class="value">"match_parent"</span></span>
<span class="line">    <span class="attribute">android:layout_height</span>=<span class="value">"match_parent"</span></span>
<span class="line">    <span class="attribute">android:orientation</span>=<span class="value">"vertical"</span> &gt;</span></span>
<span class="line"></span>
<span class="line">    <span class="tag">&lt;<span class="title">com.androidannotations.view.CustomButton_</span></span>
<span class="line">        <span class="attribute">android:layout_width</span>=<span class="value">"match_parent"</span></span>
<span class="line">        <span class="attribute">android:layout_height</span>=<span class="value">"wrap_content"</span> /&gt;</span></span>
<span class="line"></span>
<span class="line">    <span class="comment">&lt;!-- ... --&gt;</span></span>
<span class="line"></span>
<span class="line"><span class="tag">&lt;/<span class="title">LinearLayout</span>&gt;</span></span>
</pre></td></tr></table></figure>

You can also create it programmatically:

<figure class="highlight armasm"><table><tr><td class="code"><pre><span class="line"><span class="label">CustomButton</span> <span class="keyword">button </span>= CustomButton_.<span class="keyword">build(context);</span></span>
</pre></td></tr></table></figure>

#### Custom ViewGroups with @EViewGroup
> Since AndroidAnnotations 2.2

**How to create it ?**

* * *

First of all, let’s create a layout XML file for this component.

<figure class="highlight xml"><table><tr><td class="code"><pre><span class="line"><span class="pi">&lt;?xml version="1.0" encoding="utf-8"?&gt;</span></span>
<span class="line"><span class="tag">&lt;<span class="title">merge</span> <span class="attribute">xmlns:android</span>=<span class="value">"http://schemas.android.com/apk/res/android"</span> &gt;</span></span>
<span class="line"></span>
<span class="line">    <span class="tag">&lt;<span class="title">ImageView</span></span>
<span class="line">        <span class="attribute">android:id</span>=<span class="value">"@+id/image"</span></span>
<span class="line">        <span class="attribute">android:layout_alignParentRight</span>=<span class="value">"true"</span></span>
<span class="line">        <span class="attribute">android:layout_alignBottom</span>=<span class="value">"@+id/title"</span></span>
<span class="line">        <span class="attribute">android:layout_width</span>=<span class="value">"wrap_content"</span></span>
<span class="line">        <span class="attribute">android:layout_height</span>=<span class="value">"wrap_content"</span></span>
<span class="line">        <span class="attribute">android:src</span>=<span class="value">"@drawable/check"</span> /&gt;</span></span>
<span class="line"></span>
<span class="line">    <span class="tag">&lt;<span class="title">TextView</span></span>
<span class="line">        <span class="attribute">android:id</span>=<span class="value">"@+id/title"</span></span>
<span class="line">        <span class="attribute">android:layout_toLeftOf</span>=<span class="value">"@+id/image"</span></span>
<span class="line">        <span class="attribute">android:layout_width</span>=<span class="value">"match_parent"</span></span>
<span class="line">        <span class="attribute">android:layout_height</span>=<span class="value">"wrap_content"</span></span>
<span class="line">        <span class="attribute">android:textColor</span>=<span class="value">"@android:color/white"</span></span>
<span class="line">        <span class="attribute">android:textSize</span>=<span class="value">"12pt"</span> /&gt;</span></span>
<span class="line"></span>
<span class="line">    <span class="tag">&lt;<span class="title">TextView</span></span>
<span class="line">        <span class="attribute">android:id</span>=<span class="value">"@+id/subtitle"</span></span>
<span class="line">        <span class="attribute">android:layout_width</span>=<span class="value">"match_parent"</span></span>
<span class="line">        <span class="attribute">android:layout_height</span>=<span class="value">"wrap_content"</span></span>
<span class="line">        <span class="attribute">android:layout_below</span>=<span class="value">"@+id/title"</span></span>
<span class="line">        <span class="attribute">android:textColor</span>=<span class="value">"#FFdedede"</span></span>
<span class="line">        <span class="attribute">android:textSize</span>=<span class="value">"10pt"</span> /&gt;</span></span>
<span class="line"></span>
<span class="line"><span class="tag">&lt;/<span class="title">merge</span>&gt;</span></span>
</pre></td></tr></table></figure>
> Did you know about [the merge tag](http://developer.android.com/training/improving-layouts/reusing-layouts.html#Merge) ? When this layout will get inflated, the childrens will be added directly to the parent, you’ll save a level in the view hierarchy.

As you can see I used some RelativeLayout specific layout attributes (layout_alignParentRight, layout_alignBottom, layout_toLeftOf, etc…), it’s because I’m assuming my layout will be inflated in a RelativeLayout. And that’s indeed what I’ll do !

<figure class="highlight scala"><table><tr><td class="code"><pre><span class="line"><span class="annotation">@EViewGroup</span>(<span class="type">R</span>.layout.title_with_subtitle)</span>
<span class="line">public <span class="class"><span class="keyword">class</span> <span class="title">TitleWithSubtitle</span> <span class="keyword"><span class="keyword">extends</span></span> <span class="title">RelativeLayout</span> &#123;</span></span>
<span class="line"></span>
<span class="line">    <span class="annotation">@ViewById</span></span>
<span class="line">    <span class="keyword">protected</span> <span class="type">TextView</span> title, subtitle;</span>
<span class="line"></span>
<span class="line">    public <span class="type">TitleWithSubtitle</span>(<span class="type">Context</span> context, <span class="type">AttributeSet</span> attrs) &#123;</span>
<span class="line">        <span class="keyword">super</span>(context, attrs);</span>
<span class="line">    &#125;</span>
<span class="line"></span>
<span class="line">    public void setTexts(<span class="type">String</span> titleText, <span class="type">String</span> subTitleText) &#123;</span>
<span class="line">        title.setText(titleText);</span>
<span class="line">        subtitle.setText(subTitleText);</span>
<span class="line">    &#125;</span>
<span class="line"></span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>

There you are ! Easy, isn’t it ?

Now let’s see how to use this brand new component.

#### How to use it ?

A custom component can be declared and placed in a layout just as any other View:

<figure class="highlight xml"><table><tr><td class="code"><pre><span class="line"><span class="pi">&lt;?xml version="1.0" encoding="utf-8"?&gt;</span></span>
<span class="line"><span class="tag">&lt;<span class="title">LinearLayout</span> <span class="attribute">xmlns:android</span>=<span class="value">"http://schemas.android.com/apk/res/android"</span></span>
<span class="line">    <span class="attribute">android:layout_width</span>=<span class="value">"match_parent"</span></span>
<span class="line">    <span class="attribute">android:layout_height</span>=<span class="value">"match_parent"</span></span>
<span class="line">    <span class="attribute">android:gravity</span>=<span class="value">"center"</span></span>
<span class="line">    <span class="attribute">android:orientation</span>=<span class="value">"vertical"</span> &gt;</span></span>
<span class="line"></span>
<span class="line">    <span class="tag">&lt;<span class="title">com.androidannotations.viewgroup.TitleWithSubtitle_</span></span>
<span class="line">        <span class="attribute">android:id</span>=<span class="value">"@+id/firstTitle"</span></span>
<span class="line">        <span class="attribute">android:layout_width</span>=<span class="value">"match_parent"</span></span>
<span class="line">        <span class="attribute">android:layout_height</span>=<span class="value">"wrap_content"</span> /&gt;</span></span>
<span class="line"></span>
<span class="line">    <span class="tag">&lt;<span class="title">com.androidannotations.viewgroup.TitleWithSubtitle_</span></span>
<span class="line">        <span class="attribute">android:id</span>=<span class="value">"@+id/secondTitle"</span></span>
<span class="line">        <span class="attribute">android:layout_width</span>=<span class="value">"match_parent"</span></span>
<span class="line">        <span class="attribute">android:layout_height</span>=<span class="value">"wrap_content"</span> /&gt;</span></span>
<span class="line"></span>
<span class="line">    <span class="tag">&lt;<span class="title">com.androidannotations.viewgroup.TitleWithSubtitle_</span></span>
<span class="line">        <span class="attribute">android:id</span>=<span class="value">"@+id/thirdTitle"</span></span>
<span class="line">        <span class="attribute">android:layout_width</span>=<span class="value">"match_parent"</span></span>
<span class="line">        <span class="attribute">android:layout_height</span>=<span class="value">"wrap_content"</span> /&gt;</span></span>
<span class="line"></span>
<span class="line"><span class="tag">&lt;/<span class="title">LinearLayout</span>&gt;</span></span>
</pre></td></tr></table></figure>
> As always, don’t forget the _ at the end of the component name!

And because I’m using AA, I can just as easily get my custom components injected in my activity and use them!

<figure class="highlight scala"><table><tr><td class="code"><pre><span class="line"><span class="annotation">@EActivity</span>(<span class="type">R</span>.layout.main)</span>
<span class="line">public <span class="class"><span class="keyword">class</span> <span class="title">Main</span> <span class="keyword"><span class="keyword">extends</span></span> <span class="title">Activity</span> &#123;</span></span>
<span class="line"></span>
<span class="line">    <span class="annotation">@ViewById</span></span>
<span class="line">    <span class="keyword">protected</span> <span class="type">TitleWithSubtitle</span> firstTitle, secondTitle, thirdTitle;</span>
<span class="line"></span>
<span class="line">    <span class="annotation">@AfterViews</span></span>
<span class="line">    <span class="keyword">protected</span> void init() &#123;</span>
<span class="line"></span>
<span class="line">        firstTitle.setTexts(<span class="string">"decouple your code"</span>,</span>
<span class="line">                <span class="string">"Hide the component logic from the code using it."</span>);</span>
<span class="line"></span>
<span class="line">        secondTitle.setTexts(<span class="string">"write once, reuse anywhere"</span>,</span>
<span class="line">                <span class="string">"Declare you component in multiple "</span> +</span>
<span class="line">                <span class="string">"places, just as easily as you "</span> +</span>
<span class="line">                <span class="string">"would put a single basic View."</span>);</span>
<span class="line"></span>
<span class="line">        thirdTitle.setTexts(<span class="string">"Let's get stated!"</span>,</span>
<span class="line">                <span class="string">"Let's see how AndroidAnnotations can make it easier!"</span>);</span>
<span class="line">    &#125;</span>
<span class="line"></span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>

Most AA annotations are available in an @EViewGroup, give it a try !

### @AfterXXX call order

#### Calls for method with the same @AfterXXX annotation

The calling order of methods with the same annotation within a class cannot be guaranteed and you should NOT rely on it. If you want multiple methods be called in an ensured order, you should create one method that is annotated and use that to call the other methods in their desired order. If you have to deal with parent/child classes you could try this.

**Example**

<figure class="highlight aspectj"><table><tr><td class="code"><pre><span class="line"><span class="annotation">@AfterInject</span></span>
<span class="line"><span class="keyword">protected</span> <span class="function"><span class="keyword">void</span> <span class="title">callInOrder</span><span class="params">()</span> </span>&#123;</span>
<span class="line">    methodA();</span>
<span class="line">    methodB();</span>
<span class="line">    methodC();</span>
<span class="line">&#125;</span>
<span class="line"></span>
<span class="line"><span class="keyword">private</span> <span class="function"><span class="keyword">void</span> <span class="title">methodA</span><span class="params">()</span> </span>&#123;</span>
<span class="line">    <span class="comment">// your code</span></span>
<span class="line">&#125;</span>
<span class="line"></span>
<span class="line"><span class="keyword">private</span> <span class="function"><span class="keyword">void</span> <span class="title">methodB</span><span class="params">()</span> </span>&#123;</span>
<span class="line">    <span class="comment">// your code</span></span>
<span class="line">&#125;</span>
<span class="line"></span>
<span class="line"><span class="keyword">private</span> <span class="function"><span class="keyword">void</span> <span class="title">methodC</span><span class="params">()</span> </span>&#123;</span>
<span class="line">    <span class="comment">// your code</span></span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>

#### Initial calls (after creation)

1.  [@AfterExtras](https://github.com/excilys/androidannotations/wiki/Extras#executing-code-after-extras-injection)
2.  [@AfterInject](https://github.com/excilys/androidannotations/wiki/Enhance-custom-classes#executing-code-after-dependency-injection)
3.  [@AfterViews](https://github.com/excilys/androidannotations/wiki/Injecting-Views)

Note: When @AfterExtras annotated methods are called, all injections that are not related to views are done. Therefore it is safe to use fields that are annotated with e.g. @Bean. The views are only available when the @AfterViews annotated methods get called.

#### Subsequent calls

*   [@AfterExtras](https://github.com/excilys/androidannotations/wiki/Extras#executing-code-after-extras-injection) methods get called every time a new Intent reaches the Activity.
*   [@AfterViews](https://github.com/excilys/androidannotations/wiki/Enhance-custom-classes#executing-code-after-dependency-injection) will be called everytime setContentView() is called. (Activity only)