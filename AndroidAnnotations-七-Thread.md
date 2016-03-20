---
title: AndroidAnnotations (七) - Thread
tags:
  - Android
  - AndroidAnnotations
date: 2015-11-25 08:03:11
---

### WorkingWithThreads

#### @Background

The `@Background` annotation indicates that a method will run in a thread other than the [ui thread](http://developer.android.com/guide/topics/fundamentals.html#threads).

Usage example:

<figure class="highlight aspectj"><table><tr><td class="code"><pre><span class="line"><span class="function"><span class="keyword">void</span> <span class="title">myMethod</span><span class="params">()</span> </span>&#123;</span>
<span class="line">    someBackgroundWork(<span class="string">"hello"</span>, <span class="number">42</span>);</span>
<span class="line">&#125;</span>
<span class="line"></span>
<span class="line"><span class="annotation">@Background</span></span>
<span class="line"><span class="function"><span class="keyword">void</span> <span class="title">someBackgroundWork</span><span class="params">(String aParam, <span class="keyword">long</span> anotherParam)</span> </span>&#123;</span>
<span class="line">    [...]</span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>
> The method is executed on a separate thread, but this doesn’t necessarily mean that a new thread will be started, because we use a shared cached thread pool executor (which can be replaced) to prevent creating too much threads.
> 
> This also means that two `@Background` methods may run in parallel

##### Id
> Since AndroidAnnotations 3.0

If you want to cancel a background task, you can use the `id` field. Every task could then be cancelled by `BackgroundExecutor.cancelAll(&quot;id&quot;)`;:

<figure class="highlight aspectj"><table><tr><td class="code"><pre><span class="line"><span class="function"><span class="keyword">void</span> <span class="title">myMethod</span><span class="params">()</span> </span>&#123;</span>
<span class="line">    someCancellableBackground(<span class="string">"hello"</span>, <span class="number">42</span>);</span>
<span class="line">    [...]</span>
<span class="line">    <span class="keyword">boolean</span> mayInterruptIfRunning = <span class="keyword">true</span>;</span>
<span class="line">    BackgroundExecutor.cancelAll(<span class="string">"cancellable_task"</span>, mayInterruptIfRunning);</span>
<span class="line">&#125;</span>
<span class="line"></span>
<span class="line"><span class="annotation">@Background</span>(id=<span class="string">"cancellable_task"</span>)</span>
<span class="line"><span class="function"><span class="keyword">void</span> <span class="title">someCancellableBackground</span><span class="params">(String aParam, <span class="keyword">long</span> anotherParam)</span> </span>&#123;</span>
<span class="line">    [...]</span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>

##### Serial(串行)
> Since AndroidAnnotations 3.0

By default, `@Background` method are processed in parallel(并行). If you want them to be executed sequentially, you can use the `serial` field. All background tasks with the same `serial` will be executed sequentially:

<figure class="highlight cpp"><table><tr><td class="code"><pre><span class="line"><span class="function"><span class="keyword">void</span> <span class="title">myMethod</span><span class="params">()</span> </span>&#123;</span>
<span class="line">    <span class="keyword">for</span> (<span class="keyword">int</span> i = <span class="number">0</span>; i &lt; <span class="number">10</span>; i++)</span>
<span class="line">        someSequentialBackgroundMethod(i);</span>
<span class="line">&#125;</span>
<span class="line"></span>
<span class="line">@Background(serial = <span class="string">"test"</span>)</span>
<span class="line"><span class="function"><span class="keyword">void</span> <span class="title">someSequentialBackgroundMethod</span><span class="params">(<span class="keyword">int</span> i)</span> </span>&#123;</span>
<span class="line">    SystemClock.sleep(<span class="keyword">new</span> Random().nextInt(<span class="number">2000</span>)+<span class="number">1000</span>);</span>
<span class="line">    Log.d(<span class="string">"AA"</span>, <span class="string">"value : "</span> + i);</span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>

##### Delay
> Since AndroidAnnotations 3.0

If you need to add a delay before a background method is run, you can use the `delay` parameter:

<figure class="highlight aspectj"><table><tr><td class="code"><pre><span class="line"><span class="annotation">@Background</span>(delay=<span class="number">2000</span>)</span>
<span class="line"><span class="function"><span class="keyword">void</span> <span class="title">doInBackgroundAfterTwoSeconds</span><span class="params">()</span> </span>&#123;</span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>

##### @UiThread

The `@UiThread` annotation indicates that a method will run in the [ui thread](http://developer.android.com/guide/topics/fundamentals.html#threads).

Usage example:

<figure class="highlight aspectj"><table><tr><td class="code"><pre><span class="line"><span class="function"><span class="keyword">void</span> <span class="title">myMethod</span><span class="params">()</span> </span>&#123;</span>
<span class="line">    doInUiThread(<span class="string">"hello"</span>, <span class="number">42</span>);</span>
<span class="line">&#125;</span>
<span class="line"></span>
<span class="line"><span class="annotation">@UiThread</span></span>
<span class="line"><span class="function"><span class="keyword">void</span> <span class="title">doInUiThread</span><span class="params">(String aParam, <span class="keyword">long</span> anotherParam)</span> </span>&#123;</span>
<span class="line">    [...]</span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>

No more `AsyncTask&lt;Param, Progress, Result&gt;`!!

##### Propagation

Prior to 3.0, `@Thread` annotated method calls was always added in the handler execution queue to ensure that execution was done in Ui thread. Since 3.0, we kept the same behavior for compatibility purpose.

But, if you want to optimize UiThread calls, you may want to change `propagation` value to `REUSE`. In this configuration, the code will make a direct call to the method if current thread is already Ui thread. If not, we’re falling back to handler call.

If you need to add a delay before a method is run on the UI Thread, you can use the `delay` parameter:

<figure class="highlight aspectj"><table><tr><td class="code"><pre><span class="line"><span class="annotation">@UiThread</span>(propagation = Propagation.REUSE)</span>
<span class="line"><span class="function"><span class="keyword">void</span> <span class="title">runInSameThreadIfOnUiThread</span><span class="params">()</span> </span>&#123;</span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>

**Note**: If combined with delay, propagation will be ignored and the call will always be posted in handler execution queue.

#### @SupposeBackground
> Since AndroidAnnotations 3.1

Annotate your methods to ensure they are called from a background thread with (optionally) restrictions by allowed serials. If it is not called from a supposed background thread, then `IllegalStateException` will be thrown (by default). The allowed IDs can be passed to the `serial` parameter, if nothing is passed, any `ID` is allowed.
If you want to override what happens when the method is not called on the supposed thread, pass an instance of `BackgroundExecutor.WrongThreadListener`to `BackgroundExecutor.setWrongThreadListener()`. Its onBgExpected method will be invoked if the annotated method is called on the wrong thread, or its `onWrongBgSerial` will be invoked if the annotated method called on a background thread but with wrong IDs.

Usage example:

<figure class="highlight less"><table><tr><td class="code"><pre><span class="line"><span class="variable">@EBean</span></span>
<span class="line">public class MyBean &#123;</span>
<span class="line"></span>
<span class="line">  <span class="variable">@SupposeBackground</span></span>
<span class="line">  void <span class="function">someMethodThatShouldNotBeCalledFromUiThread</span>() &#123;</span>
<span class="line">  	<span class="comment">//if this method will be called from the UI-thread an exception will be thrown</span></span>
<span class="line">  &#125;</span>
<span class="line"></span>
<span class="line">  <span class="variable">@SupposeBackground</span>(serial = &#123;<span class="string">"serial1"</span>, <span class="string">"serial2"</span>&#125;)</span>
<span class="line">  void <span class="function">someMethodThatShouldBeCalledFromSerial1OrSerial2</span>() &#123;</span>
<span class="line">  	<span class="comment">//if this method will be called from another thread then a background thread with a</span></span>
<span class="line"> 	<span class="comment">//serial "serial1" or "serial2", an exception will be thrown</span></span>
<span class="line">  &#125;</span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>

#### @SupposeUiThread
> Since AndroidAnnotations 3.1

Annotate your methods to ensure they are called from the UI thread. If they are not, then `IllegalStateException` will be thrown (by default).
If you want to override what happens when the method is not called on the UI thread, pass an instance of `BackgroundExecutor.WrongThreadListener`to `BackgroundExecutor.setWrongThreadListener()`. Its `onUiExpected` method will be invoked if the annotated method is called on the wrong thread.

Usage example:

<figure class="highlight java"><table><tr><td class="code"><pre><span class="line"><span class="annotation">@EBean</span></span>
<span class="line"><span class="keyword">public</span> <span class="class"><span class="keyword">class</span> <span class="title">MyBean</span> </span>&#123;</span>
<span class="line"></span>
<span class="line">  <span class="annotation">@SupposeUiThread</span></span>
<span class="line">  <span class="function"><span class="keyword">void</span> <span class="title">someMethodThatShouldBeCalledOnlyFromUiThread</span><span class="params">()</span> </span>&#123;</span>
<span class="line">  <span class="comment">//if this method will be called from a background thread an exception will be thrown</span></span>
<span class="line">  &#125;</span>
<span class="line"></span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>

### BackgroundTasksAndActivityBinding
> Since AndroidAnnotations 2.5

Like an AsyncTask, `@Background` does not handle any lifecycle changes on your activities.

For instance, if you call an `@Background` method and then the screen is rotated, then the `@Background` code will be executed, no matter what, on the instance it was called on.

If `@Background` is placed on a method that belongs to the activity, and in turns call a `@UiThread` method, there is a risk that the `@UiThread` method will be called on the wrong activity instance, if a configuration change occurred.

In Android, prior to Loaders, the usual way to handle that was to use an `AsyncTask`, keep references to those tasks in `onRetainNonConfigurationInstance()`, and rebind them to the new activity after a config change.

AndroidAnnotations provides `@NonConfigurationInstance`, which can be combined with `@EBean`, `@Bean` and `@Background` to achieve the same effect.

Here is an example, first of all the activity:

<figure class="highlight nimrod"><table><tr><td class="code"><pre><span class="line">@<span class="type">EActivity</span></span>
<span class="line">public class <span class="type">MyActivity</span> extends <span class="type">Activity</span> &#123;</span>
<span class="line"></span>
<span class="line">  /* <span class="type">Using</span> @<span class="type">NonConfigurationInstance</span> on a @<span class="type">Bean</span> will automatically </span>
<span class="line">   * update the context <span class="keyword">ref</span> on configuration changes,</span>
<span class="line">   * <span class="keyword">if</span> the bean <span class="keyword">is</span> <span class="keyword">not</span> a singleton</span>
<span class="line">   */</span>
<span class="line">  @<span class="type">NonConfigurationInstance</span></span>
<span class="line">  @<span class="type">Bean</span></span>
<span class="line">  <span class="type">MyBackgroundTask</span> task;</span>
<span class="line"></span>
<span class="line">  @<span class="type">Click</span></span>
<span class="line">  <span class="type">void</span> myButtonClicked() &#123;</span>
<span class="line">    task.doSomethingInBackground();</span>
<span class="line">  &#125;</span>
<span class="line"></span>
<span class="line">  <span class="type">void</span> showResult(<span class="type">MyResult</span> <span class="literal">result</span>) &#123;</span>
<span class="line">    // <span class="keyword">do</span> something <span class="keyword">with</span> <span class="literal">result</span></span>
<span class="line">  &#125;</span>
<span class="line"></span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>

Then the task itself:

<figure class="highlight nimrod"><table><tr><td class="code"><pre><span class="line">@<span class="type">EBean</span></span>
<span class="line">public class <span class="type">MyBackgroundTask</span> &#123;</span>
<span class="line"></span>
<span class="line">  @<span class="type">RootContext</span></span>
<span class="line">  <span class="type">MyActivity</span> activity;</span>
<span class="line"></span>
<span class="line">  @<span class="type">Background</span></span>
<span class="line">  <span class="type">void</span> doSomethingInBackground() &#123;</span>
<span class="line">    // <span class="keyword">do</span> something</span>
<span class="line">    <span class="type">MyResult</span> <span class="literal">result</span> = <span class="type">XXX</span>;</span>
<span class="line"></span>
<span class="line">    updateUI(<span class="literal">result</span>);</span>
<span class="line">  &#125;</span>
<span class="line"></span>
<span class="line">  // <span class="type">Notice</span> that we manipulate the activity <span class="keyword">ref</span> only <span class="keyword">from</span> the <span class="type">UI</span> thread</span>
<span class="line">  @<span class="type">UiThread</span></span>
<span class="line">  <span class="type">void</span> updateUI(<span class="type">MyResult</span> <span class="literal">result</span>) &#123;</span>
<span class="line">    activity.showResult(<span class="literal">result</span>);</span>
<span class="line">  &#125;</span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>
> We could probably provide even better solutions, but there are a lot of different use cases, and we need to think about them all. So right now, @Background has a very simple behavior and this is not going to change. We could, however, introduce new annotations with an advanced “thread + lifecycle” behavior.