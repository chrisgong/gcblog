---
title: AndroidAnnotations (九) - Event Handling
tags:
  - Android
  - AndroidAnnotations
date: 2015-11-25 09:54:42
---

### ClickEvents

#### @Click
<figure class="highlight less"><table><tr><td class="code"><pre><span class="line"><span class="variable">@Click</span>(R.id.myButton)</span>
<span class="line">void <span class="function">myButtonWasClicked</span>() &#123;</span>
<span class="line">    <span class="attr_selector">[...]</span></span>
<span class="line">&#125;</span>
<span class="line"></span>
<span class="line"><span class="variable">@Click</span></span>
<span class="line">void <span class="function">anotherButton</span>() &#123;</span>
<span class="line">    <span class="attr_selector">[...]</span></span>
<span class="line">&#125;</span>
<span class="line"></span>
<span class="line"><span class="variable">@Click</span></span>
<span class="line">void <span class="function">yetAnotherButton</span>(View clickedView) &#123;</span>
<span class="line">    <span class="attr_selector">[...]</span></span>
<span class="line">&#125;</span>
<span class="line"></span>
<span class="line"><span class="variable">@Click</span>(&#123;R.id.myButton, R.id.myOtherButton&#125;)</span>
<span class="line">void <span class="function">handlesTwoButtons</span>() &#123;</span>
<span class="line">    <span class="attr_selector">[...]</span></span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>

### Other events

Currently, AndroidAnnotations supports the following [events](http://developer.android.com/guide/topics/ui/ui-events.html#EventListeners) on views:

[Clicks](http://developer.android.com/reference/android/view/View.OnClickListener.html) with @Click
[Long clicks](http://developer.android.com/reference/android/view/View.OnLongClickListener.html) with @LongClick
[Touches](http://developer.android.com/reference/android/view/View.OnTouchListener.html) with @Touch

### AdapterViewEvents

You can bind methods to handle events on items in an AdapterView:

*   [Item clicks](http://developer.android.com/reference/android/widget/AdapterView.OnItemClickListener.html) with @ItemClick
*   [Long item clicks](http://developer.android.com/reference/android/widget/AdapterView.OnItemLongClickListener.html) with @ItemLongClick
*   [Item selection](http://developer.android.com/reference/android/widget/AdapterView.OnItemSelectedListener.html) with @ItemSelect

The annotation value should be one or several of R.id._ fields. If not set, the method name will be used as the R.id._ field name.

Methods annotated with @ItemClick or @ItemLongClick must have one parameter. This parameter can be of any type, it’s the object retrieved when calling adapter.getItem(position).

Methods annotated with @ItemSelect may have one or two parameters. The first parameter must be a boolean, and the second is the object from the adapter, at the selected position.

<figure class="highlight less"><table><tr><td class="code"><pre><span class="line"><span class="variable">@EActivity</span>(R.layout.my_list)</span>
<span class="line">public class MyListActivity extends Activity &#123;</span>
<span class="line"></span>
<span class="line">    <span class="comment">// ...</span></span>
<span class="line"></span>
<span class="line">    <span class="variable">@ItemClick</span></span>
<span class="line">    public void <span class="function">myListItemClicked</span>(MyItem clickedItem) &#123;</span>
<span class="line"></span>
<span class="line">    &#125;</span>
<span class="line"></span>
<span class="line">    <span class="variable">@ItemLongClick</span></span>
<span class="line">    public void <span class="function">myListItemLongClicked</span>(MyItem clickedItem) &#123;</span>
<span class="line"></span>
<span class="line">    &#125;</span>
<span class="line"></span>
<span class="line">    <span class="variable">@ItemSelect</span></span>
<span class="line">    public void <span class="function">myListItemSelected</span>(boolean selected, MyItem selectedItem) &#123;</span>
<span class="line"></span>
<span class="line">    &#125;</span>
<span class="line"></span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>
> Since AndroidAnnotations 2.4
> 
> For `@ItemClick`, `@ItemLongClick` and `@ItemSelect`, if the parameter is of type int, then the position is given instead of the object coming from the adapter.
<figure class="highlight less"><table><tr><td class="code"><pre><span class="line"><span class="variable">@EActivity</span>(R.layout.my_list)</span>
<span class="line">public class MyListActivity extends Activity &#123;</span>
<span class="line"></span>
<span class="line">    <span class="comment">// ...</span></span>
<span class="line"></span>
<span class="line">    <span class="variable">@ItemClick</span></span>
<span class="line">    public void <span class="function">myListItemClicked</span>(int position) &#123;</span>
<span class="line"></span>
<span class="line">    &#125;</span>
<span class="line"></span>
<span class="line">    <span class="variable">@ItemLongClick</span></span>
<span class="line">    public void <span class="function">myListItemLongClicked</span>(int position) &#123;</span>
<span class="line"></span>
<span class="line">    &#125;</span>
<span class="line"></span>
<span class="line">    <span class="variable">@ItemSelect</span></span>
<span class="line">    public void <span class="function">myListItemSelected</span>(boolean selected, int position) &#123;</span>
<span class="line"></span>
<span class="line">    &#125;</span>
<span class="line"></span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>

### TextChangeEvents

#### @TextChange

This annotation is intended to be used on methods to receive events defined by `android.text.TextWatcher.onTextChanged(CharSequence s, int start, int before, int count)`when the text is changed on the targeted TextView or subclass of TextView. The annotation value should be one or several R.id._ fields that refers to TextView or subclasses of TextView. If not set, the [method name](https://github.com/excilys/androidannotations/wiki/InferringIDFromMethodName) will be used as the R.id._ field name. The method may have multiple parameter:

*   A `android.widget.TextView` parameter to know which view has received this event
*   A `java.lang.CharSequence` parameter to get the modified text.
*   An int parameter named start to get the start position of the modified text.
*   An int parameter named before to know the text length before modification.
*   An int parameter named count to know the number of modified characters.

Some usage examples of `@BeforeTextChange` annotation:

<figure class="highlight less"><table><tr><td class="code"><pre><span class="line"><span class="variable">@TextChange</span>(R.id.helloTextView)</span>
<span class="line">void <span class="function">onTextChangesOnHelloTextView</span>(CharSequence text, TextView hello, int before, int start, int count) &#123;</span>
<span class="line">   <span class="comment">// Something Here</span></span>
<span class="line">&#125;</span>
<span class="line"></span>
<span class="line"><span class="variable">@TextChange</span></span>
<span class="line">void <span class="function">helloTextViewTextChanged</span>(TextView hello) &#123;</span>
<span class="line">   <span class="comment">// Something Here</span></span>
<span class="line">&#125;</span>
<span class="line"></span>
<span class="line"><span class="variable">@TextChange</span>(&#123;R.id.editText, R.id.helloTextView&#125;)</span>
<span class="line">void <span class="function">onTextChangesOnSomeTextViews</span>(TextView tv, CharSequence text) &#123;</span>
<span class="line">   <span class="comment">// Something Here</span></span>
<span class="line">&#125;</span>
<span class="line"></span>
<span class="line"><span class="variable">@TextChange</span>(R.id.helloTextView)</span>
<span class="line">void <span class="function">onTextChangesOnHelloTextView</span>() &#123;</span>
<span class="line">   <span class="comment">// Something Here</span></span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>

#### @BeforeTextChange

This annotation is intended to be used on methods to receive events defined by android.text.TextWatcher.beforeTextChanged(CharSequence s, int start, int count, int after) before the text is changed on the targeted TextView or subclass of TextView. The annotation value should be one or several R.id._ fields that refers to TextView or subclasses of TextView. If not set, the [method name](https://github.com/excilys/androidannotations/wiki/InferringIDFromMethodName) will be used as the R.id._ field name. The method may have multiple parameters:

*   A `android.widget.TextView` parameter to know which view has targeted this event
*   An `java.lang.CharSequence` parameter to get the text before modification.
*   An int parameter named start to get the start position of the modified text.
*   An int parameter named count to know the number of modified characters.
*   An int parameter named after to know the text length after the text modification.

Some usage examples of `@BeforeTextChange` annotation:

<figure class="highlight less"><table><tr><td class="code"><pre><span class="line"><span class="variable">@BeforeTextChange</span>(R.id.helloTextView)</span>
<span class="line">void <span class="function">beforeTextChangedOnHelloTextView</span>(TextView hello, CharSequence text, int start, int count, int after) &#123;</span>
<span class="line"><span class="comment">// Something Here</span></span>
<span class="line">&#125;</span>
<span class="line"></span>
<span class="line"><span class="variable">@BeforeTextChange</span></span>
<span class="line">void <span class="function">helloTextViewBeforeTextChanged</span>(TextView hello) &#123;</span>
<span class="line"><span class="comment">// Something Here</span></span>
<span class="line">&#125;</span>
<span class="line"></span>
<span class="line"><span class="variable">@BeforeTextChange</span>(&#123;R.id.editText, R.id.helloTextView&#125;)</span>
<span class="line">void <span class="function">beforeTextChangedOnSomeTextViews</span>(TextView tv, CharSequence text) &#123;</span>
<span class="line"><span class="comment">// Something Here</span></span>
<span class="line">&#125;</span>
<span class="line"></span>
<span class="line"><span class="variable">@BeforeTextChange</span>(R.id.helloTextView)</span>
<span class="line">void <span class="function">beforeTextChangedOnHelloTextView</span>() &#123;</span>
<span class="line"><span class="comment">// Something Here</span></span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>

#### @AfterTextChange

This annotation is intended to be used on methods to receive events defined by `android.text.TextWatcher.afterTextChanged(Editable s)` after the text is changed on the targeted TextView or subclass of TextView. The annotation value should be one or several `R.id.*` fields that refers to TextView or subclasses of TextView. If not set, the [method name](https://github.com/excilys/androidannotations/wiki/InferringIDFromMethodName) will be used as the `R.id.*` field name. The method may have multiple parameter:

*   A `android.widget.TextView` parameter to know which view has targeted this event
*   `An android.text.Editable` to make changes on modified text.

Some usage examples of `@BeforeTextChange` annotation :

<figure class="highlight less"><table><tr><td class="code"><pre><span class="line"><span class="variable">@AfterTextChange</span>(R.id.helloTextView)</span>
<span class="line">void <span class="function">afterTextChangedOnHelloTextView</span>(Editable text, TextView hello) &#123;</span>
<span class="line"><span class="comment">// Something Here</span></span>
<span class="line">&#125;</span>
<span class="line"></span>
<span class="line"><span class="variable">@AfterTextChange</span></span>
<span class="line">void <span class="function">helloTextViewAfterTextChanged</span>(TextView hello) &#123;</span>
<span class="line"><span class="comment">// Something Here</span></span>
<span class="line">&#125;</span>
<span class="line"></span>
<span class="line"><span class="variable">@AfterTextChange</span>(&#123;R.id.editText, R.id.helloTextView&#125;)</span>
<span class="line">void <span class="function">afterTextChangedOnSomeTextViews</span>(TextView tv, Editable text) &#123;</span>
<span class="line"><span class="comment">// Something Here</span></span>
<span class="line">&#125;</span>
<span class="line"></span>
<span class="line"><span class="variable">@AfterTextChange</span>(R.id.helloTextView)</span>
<span class="line">void <span class="function">afterTextChangedOnHelloTextView</span>() &#123;</span>
<span class="line"><span class="comment">// Something Here</span></span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>

#### @EditorAction

This annotation is intended to be used on methods to receive events defined by `android.widget.TextView.OnEditorActionListener#onEditorAction(android.widget.TextView, int, android.view.KeyEvent)` when an action is performed on the editor.

The annotation value should be one or several R.id._ fields that refers to TextView or subclasses of `TextView`. If not set, the [method name](https://github.com/excilys/androidannotations/wiki/InferringIDFromMethodName) will be used as the R.id._ field name.

The method may have multiple parameters:

*   A `android.widget.TextView` parameter to know which view has targeted this event.
*   An `int` parameter to get the `actionId`.
*   A `android.view.KeyEvent` parameter.

The return type of the method can be either `void` or `boolean`. In case of `boolean`, the value returned from the annotated method will be returned in the generated listener method (indicating event consumption). If the annotated method is `void`, always `true` will be returned in the listener method (so the event is consumed).

Some usage examples of `@EditorAction` annotation:

<figure class="highlight aspectj"><table><tr><td class="code"><pre><span class="line"><span class="annotation">@EditorAction</span>(R.id.helloTextView)</span>
<span class="line"><span class="function"><span class="keyword">void</span> <span class="title">onEditorActionsOnHelloTextView</span><span class="params">(TextView hello, <span class="keyword">int</span> actionId, KeyEvent keyEvent)</span> </span>&#123;</span>
<span class="line"><span class="comment">// Something Here</span></span>
<span class="line">&#125;</span>
<span class="line"></span>
<span class="line"><span class="annotation">@EditorAction</span></span>
<span class="line"><span class="function"><span class="keyword">void</span> <span class="title">helloTextViewEditorAction</span><span class="params">(TextView hello)</span> </span>&#123;</span>
<span class="line"><span class="comment">// Something Here</span></span>
<span class="line">&#125;</span>
<span class="line"></span>
<span class="line"><span class="annotation">@EditorAction</span>(&#123;R.id.editText, R.id.helloTextView&#125;)</span>
<span class="line"><span class="function"><span class="keyword">void</span> <span class="title">onEditorActionsOnSomeTextViews</span><span class="params">(TextView tv, <span class="keyword">int</span> actionId)</span> </span>&#123;</span>
<span class="line"><span class="comment">// Something Here</span></span>
<span class="line">&#125;</span>
<span class="line"></span>
<span class="line"><span class="annotation">@EditorAction</span>(R.id.helloTextView)</span>
<span class="line"><span class="function"><span class="keyword">void</span> <span class="title">onEditorActionsOnHelloTextView</span><span class="params">()</span> </span>&#123;</span>
<span class="line"><span class="comment">// Something Here</span></span>
<span class="line">&#125;</span>
<span class="line"></span>
<span class="line"></span>
<span class="line"><span class="annotation">@EditorAction</span>(R.id.helloTextView)</span>
<span class="line"><span class="function"><span class="keyword">boolean</span> <span class="title">onEditorActionsOnHelloTextView</span><span class="params">()</span> </span>&#123;</span>
<span class="line"><span class="comment">// Something Here</span></span>
<span class="line"><span class="keyword">return</span> <span class="keyword">false</span>;</span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>

### [Handling options menu](https://github.com/excilys/androidannotations/wiki/Handling%20Options%20Menu)

### [KeyEvents](https://github.com/excilys/androidannotations/wiki/KeyEvents)

### [SeekBarEvents](https://github.com/excilys/androidannotations/wiki/SeekBarEvents)