---
title: RxJava调度器Scheduler
tags:
  - RxJava
date: 2016-01-07 21:04:00
---

如果你想给Observable操作符链添加多线程功能，你可以指定操作符（或者特定的Observable）在特定的调度器(Scheduler)上执行。

某些ReactiveX的Observable操作符有一些变体，它们可以接受一个Scheduler参数。这个参数指定操作符将它们的部分或全部任务放在一个特定的调度器上执行。

使用ObserveOn和SubscribeOn操作符，你可以让Observable在一个特定的调度器上执行，ObserveOn指示一个 Observable在一个特定的调度器上调用观察者的onNext, onError和onCompleted方法，SubscribeOn更进一步，它指示Observable将全部的处理过程（包括发射数据和通知）放在特定的调度器上执行。

##### 调度器的种类

下表展示了RxJava中可用的调度器种类：

<table>
<thead>
<tr>
<th>调度器类型</th>
<th>效果</th>
</tr>
</thead>
<tbody>
<tr>
<td>Schedulers.computation( )</td>
<td>用于计算任务，如事件循环或和回调处理，不要用于IO操作(IO操作请使用Schedulers.io())；默认线程数等于处理器的数量。它也是许多RxJava方法的默认调度器：buffer(),debounce(),delay(),interval(),sample(),skip()。</td>
</tr>
<tr>
<td>Schedulers.from(executor)</td>
<td>使用指定的Executor作为调度器</td>
</tr>
<tr>
<td>Schedulers.immediate( )</td>
<td>这个调度器允许你立即在当前线程执行你指定的工作。它是timeout(),timeInterval(),以及timestamp()方法默认的调度器。</td>
</tr>
<tr>
<td>Schedulers.io( )</td>
<td>用于IO密集型任务，如异步阻塞IO操作，这个调度器的线程池会根据需要增长；对于普通的计算任务，请使用Schedulers.computation()；Schedulers.io( )默认是一个CachedThreadScheduler，很像一个有线程缓存的新线程调度器</td>
</tr>
<tr>
<td>Schedulers.newThread( )</td>
<td>为每个任务创建一个新线程</td>
</tr>
<tr>
<td>Schedulers.trampoline( )</td>
<td>当我们想在当前线程执行一个任务时，并不是立即，我们可以用.trampoline()将它入队。这个调度器将会处理它的队列并且按序运行队列中每一个任务。它是repeat()和retry()方法默认的调度器。</td>
</tr>
</tbody>
</table>

##### 默认调度器

在RxJava中，某些Observable操作符的变体允许你设置用于操作执行的调度器，其它的则不在任何特定的调度器上执行，或者在一个指定的默认调度器上执行。下面的表格个列出了一些操作符的默认调度器：

<table>
<thead>
<tr>
<th>操作符</th>
<th>调度器</th>
</tr>
</thead>
<tbody>
<tr>
<td>buffer(timespan)</td>
<td>computation</td>
</tr>
<tr>
<td>buffer(timespan, count)</td>
<td>computation</td>
</tr>
<tr>
<td>buffer(timespan, timeshift)</td>
<td>computation</td>
</tr>
<tr>
<td>debounce(timeout, unit)</td>
<td>computation</td>
</tr>
<tr>
<td>delay(delay, unit)</td>
<td>computation</td>
</tr>
<tr>
<td>delaySubscription(delay, unit)    computation</td>
</tr>
<tr>
<td>interval</td>
<td>computation</td>
</tr>
<tr>
<td>repeat</td>
<td>trampoline</td>
</tr>
<tr>
<td>replay(time, unit)</td>
<td>computation</td>
</tr>
<tr>
<td>replay(buffersize, time, unit)</td>
<td>computation</td>
</tr>
<tr>
<td>replay(selector, time, unit)</td>
<td>computation</td>
</tr>
<tr>
<td>replay(selector, buffersize, time, unit)</td>
<td>computation</td>
</tr>
<tr>
<td>retry</td>
<td>trampoline</td>
</tr>
<tr>
<td>sample(period, unit)</td>
<td>computation</td>
</tr>
<tr>
<td>skip(time, unit)</td>
<td>computation</td>
</tr>
<tr>
<td>skipLast(time, unit)</td>
<td>computation</td>
</tr>
<tr>
<td>take(time, unit)</td>
<td>computation</td>
</tr>
<tr>
<td>takeLast(time, unit)</td>
<td>computation</td>
</tr>
<tr>
<td>takeLast(count, time, unit)</td>
<td>computation</td>
</tr>
<tr>
<td>takeLastBuffer(time, unit)</td>
<td>computation</td>
</tr>
<tr>
<td>takeLastBuffer(count, time, unit)</td>
<td>computation</td>
</tr>
<tr>
<td>throttleFirst</td>
<td>computation</td>
</tr>
<tr>
<td>throttleLast</td>
<td>computation</td>
</tr>
<tr>
<td>throttleWithTimeout</td>
<td>computation</td>
</tr>
<tr>
<td>timeInterval</td>
<td>immediate</td>
</tr>
<tr>
<td>timeout(timeoutSelector)</td>
<td>immediate</td>
</tr>
<tr>
<td>timeout(firstTimeoutSelector, timeoutSelector)</td>
<td>immediate</td>
</tr>
<tr>
<td>timeout(timeoutSelector, other)</td>
<td>immediate</td>
</tr>
<tr>
<td>timeout(timeout, timeUnit)</td>
<td>computation</td>
</tr>
<tr>
<td>timeout(firstTimeoutSelector, timeoutSelector, other)</td>
<td>immediate</td>
</tr>
<tr>
<td>timeout(timeout, timeUnit, other)</td>
<td>computation</td>
</tr>
<tr>
<td>timer</td>
<td>computation</td>
</tr>
<tr>
<td>timestamp</td>
<td>immediate</td>
</tr>
<tr>
<td>window(timespan)</td>
<td>computation</td>
</tr>
<tr>
<td>window(timespan, count)</td>
<td>computation</td>
</tr>
<tr>
<td>window(timespan, timeshift)</td>
<td>computation</td>
</tr>
</tbody>
</table>

##### 使用调度器

除了将这些调度器传递给RxJava的Observable操作符，你也可以用它们调度你自己的任务。下面的示例展示了Scheduler.Worker的用法：

<figure class="highlight aspectj"><table><tr><td class="code"><pre><span class="line">worker = Schedulers.newThread().createWorker();</span>
<span class="line">worker.schedule(<span class="keyword">new</span> Action0() &#123;</span>
<span class="line"> </span>
<span class="line">    <span class="annotation">@Override</span></span>
<span class="line">    <span class="keyword">public</span> <span class="function"><span class="keyword">void</span> <span class="title">call</span><span class="params">()</span> </span>&#123;</span>
<span class="line">        yourWork();</span>
<span class="line">    &#125;</span>
<span class="line"> </span>
<span class="line">&#125;);</span>
<span class="line"><span class="comment">// some time later...</span></span>
<span class="line">worker.unsubscribe();</span>
</pre></td></tr></table></figure>

###### 递归调度器

要调度递归的方法调用，你可以使用schedule，然后再用schedule(this)，示例：

<figure class="highlight aspectj"><table><tr><td class="code"><pre><span class="line">worker = Schedulers.newThread().createWorker();</span>
<span class="line">worker.schedule(<span class="keyword">new</span> Action0() &#123;</span>
<span class="line"> </span>
<span class="line">    <span class="annotation">@Override</span></span>
<span class="line">    <span class="keyword">public</span> <span class="function"><span class="keyword">void</span> <span class="title">call</span><span class="params">()</span> </span>&#123;</span>
<span class="line">        yourWork();</span>
<span class="line">        <span class="comment">// recurse until unsubscribed (schedule will do nothing if unsubscribed)</span></span>
<span class="line">        worker.schedule(<span class="keyword">this</span>);</span>
<span class="line">    &#125;</span>
<span class="line"> </span>
<span class="line">&#125;);</span>
<span class="line"><span class="comment">// some time later...</span></span>
<span class="line">worker.unsubscribe();</span>
</pre></td></tr></table></figure>

###### 检查或设置取消订阅状态

Worker类的对象实现了Subscription接口，使用它的isUnsubscribed和unsubscribe方法，所以你可以在订阅取消时停止任务，或者从正在调度的任务内部取消订阅，示例：

<figure class="highlight sml"><table><tr><td class="code"><pre><span class="line"><span class="type">Worker</span> worker = <span class="type">Schedulers</span>.newThread<span class="literal">()</span>.createWorker<span class="literal">()</span>;</span>
<span class="line"><span class="type">Subscription</span> mySubscription = worker.schedule(new <span class="type">Action0</span><span class="literal">()</span> &#123;</span>
<span class="line"> </span>
<span class="line">    @<span class="type">Override</span></span>
<span class="line">    public void call<span class="literal">()</span> &#123;</span>
<span class="line">        <span class="keyword">while</span>(!worker.isUnsubscribed<span class="literal">()</span>) &#123;</span>
<span class="line">            status = yourWork<span class="literal">()</span>;</span>
<span class="line">            <span class="keyword">if</span>(<span class="type">QUIT</span> == status) &#123; worker.unsubscribe<span class="literal">()</span>; &#125;</span>
<span class="line">        &#125;</span>
<span class="line">    &#125;</span>
<span class="line"> </span>
<span class="line">&#125;);</span>
</pre></td></tr></table></figure>

Worker同时是Subscription，因此你可以（通常也应该）调用它的unsubscribe方法通知可以挂起任务和释放资源了。

###### 延时和周期调度器

你可以使用schedule(action,delayTime,timeUnit)在指定的调度器上延时执行你的任务，下面例子中的任务将在500毫秒之后开始执行：

<figure class="highlight css"><table><tr><td class="code"><pre><span class="line"><span class="tag">someScheduler</span><span class="class">.schedule</span>(<span class="tag">someAction</span>, 500, <span class="tag">TimeUnit</span><span class="class">.MILLISECONDS</span>);</span>
</pre></td></tr></table></figure>

使用另一个版本的schedule，schedulePeriodically(action,initialDelay,period,timeUnit)方法让你可以安排一个定期执行的任务，下面例子的任务将在500毫秒之后执行，然后每250毫秒执行一次：

<figure class="highlight cpp"><table><tr><td class="code"><pre><span class="line">someScheduler.schedulePeriodically(someAction, <span class="number">500</span>, <span class="number">250</span>, TimeUnit.MILLISECONDS);</span>
</pre></td></tr></table></figure>

###### 测试调度器

TestScheduler让你可以对调度器的时钟表现进行手动微调。这对依赖精确时间安排的任务的测试很有用处。这个调度器有三个额外的方法：

*   advanceTimeTo(time,unit) 向前波动调度器的时钟到一个指定的时间点
*   advanceTimeBy(time,unit) 将调度器的时钟向前拨动一个指定的时间段
*   triggerActions( ) 开始执行任何计划中的但是未启动的任务，如果它们的计划时间等于或者早于调度器时钟的当前时间

##### SubscribeOn and ObserveOn

RxJava提供了subscribeOn()方法来用于每个Observable对象。subscribeOn()方法用Scheduler来作为参数并在这个Scheduler上执行Observable调用。
observeOn()方法将会在指定的调度器上返回结果：如例子中的Android UI线程。
onBackpressureBuffer()方法将告诉Observable发射的数据如果比观察者消费的数据要更快的话，它必须把它们存储在缓存中并提供一个合适的时间给它们。

<figure class="highlight css"><table><tr><td class="code"><pre><span class="line">...</span>
<span class="line"><span class="class">.onBackpressureBuffer</span>()</span>
<span class="line"><span class="class">.subscribeOn</span>(<span class="tag">Schedulers</span><span class="class">.io</span>())</span>
<span class="line"><span class="class">.observeOn</span>(<span class="tag">AndroidSchedulers</span><span class="class">.mainThread</span>())</span>
<span class="line"><span class="class">.subscribe</span>(<span class="tag">new</span> <span class="tag">Observer</span>&lt;<span class="tag">Object</span>&gt;()<span class="rules">&#123;&#125;</span>);</span>
</pre></td></tr></table></figure>
