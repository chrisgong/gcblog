---
title: AndroidAnnotations (四) - Broadcastreceivers
tags:
  - Android
  - AndroidAnnotations
date: 2015-11-24 16:56:15
---

### 声明Broadcastreceivers注解
<figure class="highlight scala"><table><tr><td class="code"><pre><span class="line"><span class="annotation">@EReceiver</span></span>
<span class="line">public <span class="class"><span class="keyword">class</span> <span class="title">MyReceiver</span> <span class="keyword"><span class="keyword">extends</span></span> <span class="title">BroadcastReceiver</span> &#123;</span></span>
<span class="line"></span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>

### @ReceiverAction
> Since AndroidAnnotations 3.2

默认Recevier用法里，只能通过onReceive()来分发不同的action。

在Annotations可以通过@ReceiverAction注解来自定义接收方法

根据方法参数分类：

*   Intent
<figure class="highlight less"><table><tr><td class="code"><pre><span class="line"><span class="variable">@EReceiver</span></span>
<span class="line">public class MyIntentService extends BroadcastReceiver &#123;</span>
<span class="line"></span>
<span class="line">	<span class="variable">@ReceiverAction</span>(<span class="string">"BROADCAST_ACTION_NAME"</span>)</span>
<span class="line">	void <span class="function">mySimpleAction</span>(Intent intent) &#123;</span>
<span class="line">	    <span class="comment">// ...</span></span>
<span class="line">	&#125;</span>
<span class="line">	</span>
<span class="line">	<span class="comment">//多个行为</span></span>
<span class="line">	<span class="variable">@ReceiverAction</span>(&#123;<span class="string">"MULTI_BROADCAST_ACTION1"</span>, <span class="string">"MULTI_BROADCAST_ACTION2"</span>&#125;)</span>
<span class="line">	void <span class="function">multiAction</span>(Intent intent) &#123;</span>
<span class="line">		<span class="comment">// ...</span></span>
<span class="line">	&#125;</span>
<span class="line">	</span>
<span class="line">	<span class="variable">@Override</span></span>
<span class="line">	public void <span class="function">onReceive</span>(Context context, Intent intent) &#123;</span>
<span class="line">	    <span class="comment">// empty, will be overridden in generated subclass</span></span>
<span class="line">	&#125;</span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>

*   任何android.os.Parcelable 或者 java.io.Serializable 参数.
<figure class="highlight less"><table><tr><td class="code"><pre><span class="line"><span class="variable">@EReceiver</span></span>
<span class="line">public class MyIntentService extends BroadcastReceiver &#123;</span>
<span class="line"></span>
<span class="line">	<span class="variable">@ReceiverAction</span></span>
<span class="line">	void <span class="function">myAction</span>(<span class="variable">@ReceiverAction</span>.Extra String valueString, Context context) &#123;</span>
<span class="line">		<span class="comment">// ...</span></span>
<span class="line">	&#125;</span>
<span class="line"></span>
<span class="line">	<span class="variable">@ReceiverAction</span></span>
<span class="line">	void <span class="function">anotherAction</span>(<span class="variable">@ReceiverAction</span>.<span class="function">Extra</span>(<span class="string">"specialExtraName"</span>) String valueString, <span class="variable">@ReceiverAction</span>.Extra long valueLong) &#123;</span>
<span class="line">		<span class="comment">// ...</span></span>
<span class="line">	&#125;</span>
<span class="line"></span>
<span class="line">	<span class="variable">@Override</span></span>
<span class="line">	public void <span class="function">onReceive</span>(Context context, Intent intent) &#123;</span>
<span class="line">		<span class="comment">// empty, will be overridden in generated subclass</span></span>
<span class="line">	&#125;</span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>

* * *

> Since AndroidAnnotations 3.3

**_Note:_** 由于BroadcastReceiver # onReceive 是抽象方法, 所以必须添加一个空的实现. 为了方便起见,可以继承AbstractBroadcastReceiver类，代码里就可以忽略此方法.

### Data Schemes
<figure class="highlight scala"><table><tr><td class="code"><pre><span class="line"><span class="annotation">@EReceiver</span></span>
<span class="line">public <span class="class"><span class="keyword">class</span> <span class="title">MyIntentService</span> <span class="keyword"><span class="keyword">extends</span></span> <span class="title">BroadcastReceiver</span> &#123;</span></span>
<span class="line"></span>
<span class="line">    <span class="annotation">@ReceiverAction</span>(value = <span class="type">Intent</span>.<span class="type">ACTION_VIEW</span>,dataSchemes = <span class="string">"http"</span>)</span>
<span class="line">    <span class="keyword">protected</span> void onHttp() &#123;</span>
<span class="line">        <span class="comment">// Will be called when an App wants to open a http website but not for https.</span></span>
<span class="line">    &#125;</span>
<span class="line"></span>
<span class="line">    <span class="annotation">@ReceiverAction</span>(value = <span class="type">Intent</span>.<span class="type">ACTION_VIEW</span>, dataSchemes = &#123;<span class="string">"http"</span>, <span class="string">"https"</span>&#125;)</span>
<span class="line">    <span class="keyword">protected</span> void onHttps() &#123;</span>
<span class="line">        <span class="comment">// Will be called when an App wants to open a http or https website.</span></span>
<span class="line">    &#125;</span>
<span class="line"></span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>

### @Receiver

注册广播：

<figure class="highlight less"><table><tr><td class="code"><pre><span class="line"><span class="variable">@EActivity</span></span>
<span class="line">public class MyActivity extends Activity &#123;</span>
<span class="line">	</span>
<span class="line">	<span class="comment">//监听的Action</span></span>
<span class="line">	<span class="variable">@Receiver</span>(actions = <span class="string">"org.androidannotations.ACTION_1"</span>)</span>
<span class="line">	protected void <span class="function">onAction1</span>() &#123;</span>
<span class="line"></span>
<span class="line">	&#125;</span>
<span class="line">	</span>
<span class="line">	<span class="comment">//Action: myAction 未指定情况下Action及方法名称</span></span>
<span class="line">	<span class="variable">@Receiver</span></span>
<span class="line">	void <span class="function">myAction</span>(<span class="variable">@Receiver</span>.Extra String valueString, Context context) &#123;</span>
<span class="line">		<span class="comment">// ...</span></span>
<span class="line">	&#125;</span>
<span class="line"></span>
<span class="line">	<span class="variable">@Receiver</span></span>
<span class="line">	void <span class="function">anotherAction</span>(<span class="variable">@Receiver</span>.<span class="function">Extra</span>(<span class="string">"specialExtraName"</span>) String valueString, <span class="variable">@Receiver</span>.Extra long valueLong) &#123;</span>
<span class="line">		<span class="comment">// ...</span></span>
<span class="line">	&#125;</span>
<span class="line">	</span>
<span class="line">	<span class="variable">@Receiver</span>(value = Intent.ACTION_VIEW,dataSchemes = <span class="string">"http"</span>)</span>
<span class="line">	protected void <span class="function">onHttp</span>() &#123;</span>
<span class="line">		<span class="comment">// Will be called when an App wants to open a http website but not for https.</span></span>
<span class="line">	&#125;</span>
<span class="line"></span>
<span class="line">	<span class="variable">@Receiver</span>(value = Intent.ACTION_VIEW, dataSchemes = &#123;<span class="string">"http"</span>, <span class="string">"https"</span>&#125;)</span>
<span class="line">	protected void <span class="function">onHttps</span>() &#123;</span>
<span class="line">		<span class="comment">// Will be called when an App wants to open a http or https website.</span></span>
<span class="line">	&#125;</span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>

### Registration

在Activity, Fragment, Service的生命周期里指定BroadcastReceiver的registers 和 unregisters

通过registerAt来指定在哪个生命周期方法里注册和销毁，默认值是OnCreateOnDestroy

<table>
<thead>
<tr>
<th></th>
<th>Register</th>
<th>Unregister</th>
<th>Activity</th>
<th>Fragment</th>
<th>Service</th>
</tr>
</thead>
<tbody>
<tr>
<td>OnCreateOnDestroy</td>
<td>onCreate</td>
<td>onDestroy</td>
<td>▲</td>
<td>▲</td>
<td>▲</td>
</tr>
<tr>
<td>OnStartOnStop</td>
<td>onStart</td>
<td>onStop</td>
<td>▲</td>
<td>▲</td>
<td></td>
</tr>
<tr>
<td>OnResumeOnPause</td>
<td>onResume</td>
<td>onPause</td>
<td>▲</td>
<td>▲</td>
<td></td>
</tr>
<tr>
<td>OnAttachOnDetach</td>
<td>onAttach</td>
<td>onDetach</td>
<td></td>
<td>▲</td>
</tr>
</tbody>
</table>

example:

<figure class="highlight less"><table><tr><td class="code"><pre><span class="line"><span class="variable">@EFragment</span></span>
<span class="line">public class MyFragment extends Fragment &#123;</span>
<span class="line"></span>
<span class="line">	<span class="variable">@Receiver</span>(actions = <span class="string">"org.androidannotations.ACTION_1"</span>)</span>
<span class="line">	protected void <span class="function">onAction1RegisteredOnCreateOnDestroy</span>() &#123;</span>
<span class="line"></span>
<span class="line">	&#125;</span>
<span class="line"></span>
<span class="line">	<span class="variable">@Receiver</span>(actions = <span class="string">"org.androidannotations.ACTION_2"</span>, registerAt = Receiver.RegisterAt.OnAttachOnDetach)</span>
<span class="line">	protected void <span class="function">onAction2RegisteredOnAttachOnDetach</span>(Intent intent) &#123;</span>
<span class="line"></span>
<span class="line">	&#125;</span>
<span class="line"></span>
<span class="line">	<span class="variable">@Receiver</span>(actions = <span class="string">"org.androidannotations.ACTION_3"</span>, registerAt = Receiver.RegisterAt.OnStartOnStop)</span>
<span class="line">	protected void <span class="function">action3RegisteredOnStartOnStop</span>() &#123;</span>
<span class="line"></span>
<span class="line">	&#125;</span>
<span class="line"></span>
<span class="line">	<span class="variable">@Receiver</span>(actions = <span class="string">"org.androidannotations.ACTION_4"</span>, registerAt = Receiver.RegisterAt.OnResumeOnPause)</span>
<span class="line">	protected void <span class="function">action4RegisteredOnResumeOnPause</span>(Intent intent) &#123;</span>
<span class="line">	</span>
<span class="line">	&#125;</span>
<span class="line"></span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>

### Local broadcasting

LocalBroadcastManager是Android Support包提供了一个工具，是用来在同一个应用内的不同组件间发送Broadcast的。

使用LocalBroadcastManager有如下好处：

*   发送的广播只会在自己App内传播，不会泄露给其他App，确保隐私数据不会泄露
*   其他App也无法向你的App发送该广播，不用担心其他App会来搞破坏
*   比系统全局广播更加高效

和系统广播使用方式类似：

先通过LocalBroadcastManager lbm = LocalBroadcastManager.getInstance(this); 获取实例

然后通过函数 registerReceiver来注册监听器

<figure class="highlight aspectj"><table><tr><td class="code"><pre><span class="line">lbm.registerReceiver(<span class="keyword">new</span> BroadcastReceiver() &#123;  </span>
<span class="line">        <span class="annotation">@Override</span>  </span>
<span class="line">        <span class="keyword">public</span> <span class="function"><span class="keyword">void</span> <span class="title">onReceive</span><span class="params">(Context context, Intent intent)</span> </span>&#123;  </span>
<span class="line">        <span class="comment">// TODO Handle the received local broadcast  </span></span>
<span class="line">        &#125;  </span>
<span class="line">    &#125;, <span class="keyword">new</span> IntentFilter(LOCAL_ACTION));</span>
</pre></td></tr></table></figure>

通过 sendBroadcast 函数来发送广播

<figure class="highlight actionscript"><table><tr><td class="code"><pre><span class="line">lbm.sendBroadcast(<span class="keyword">new</span> Intent(LOCAL_ACTION));</span>
</pre></td></tr></table></figure>

* * *

AndroidAnnotations里使用LocalBroadcast方法

<figure class="highlight scala"><table><tr><td class="code"><pre><span class="line"><span class="annotation">@EService</span></span>
<span class="line">public <span class="class"><span class="keyword">class</span> <span class="title">MyService</span> <span class="keyword"><span class="keyword">extends</span></span> <span class="title">Service</span> &#123;</span></span>
<span class="line"></span>
<span class="line">	<span class="comment">//local默认值是false。</span></span>
<span class="line">	<span class="annotation">@Receiver</span>(actions = <span class="string">"org.androidannotations.ACTION_1"</span>, local = <span class="literal">true</span>)</span>
<span class="line">	<span class="keyword">protected</span> void onAction1OnCreate() &#123;  </span>
<span class="line">	&#125;</span>
<span class="line"></span>
<span class="line">	<span class="annotation">@Override</span></span>
<span class="line">	public <span class="type">IBinder</span> onBind(<span class="type">Intent</span> intent) &#123;</span>
<span class="line">	<span class="keyword">return</span> <span class="literal">null</span>;</span>
<span class="line">	&#125;</span>
<span class="line"></span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>