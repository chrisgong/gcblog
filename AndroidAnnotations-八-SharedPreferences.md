---
title: AndroidAnnotations (八) - SharedPreferences
tags:
  - Android
  - AndroidAnnotations
date: 2015-11-25 08:11:04
---

### Defining the preferences

First, you should create an interface annotated with `@SharedPref` to define the SharedPreferences :

<figure class="highlight java"><table><tr><td class="code"><pre><span class="line"><span class="annotation">@SharedPref</span></span>
<span class="line"><span class="keyword">public</span> <span class="class"><span class="keyword">interface</span> <span class="title">MyPrefs</span> </span>&#123;</span>
<span class="line"></span>
<span class="line">        <span class="comment">// The field name will have default value "John"</span></span>
<span class="line">    <span class="annotation">@DefaultString</span>(<span class="string">"John"</span>)</span>
<span class="line">    <span class="function">String <span class="title">name</span><span class="params">()</span></span>;</span>
<span class="line"></span>
<span class="line">        <span class="comment">// The field age will have default value 42</span></span>
<span class="line">    <span class="annotation">@DefaultInt</span>(<span class="number">42</span>)</span>
<span class="line">    <span class="function"><span class="keyword">int</span> <span class="title">age</span><span class="params">()</span></span>;</span>
<span class="line"></span>
<span class="line">        <span class="comment">// The field lastUpdated will have default value 0</span></span>
<span class="line">    <span class="function"><span class="keyword">long</span> <span class="title">lastUpdated</span><span class="params">()</span></span>;</span>
<span class="line"></span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>

Based on that specification, AndroidAnnotations builds a SharedPreferences Helper that has the same name plus an underscore. You can get an instance of the generated helper in any enhanced class with the `@Pref` annotation.

**Important**: The type of the field **MUST** be the generated class instead of the source class. It’s the only exception in AA.

<figure class="highlight scala"><table><tr><td class="code"><pre><span class="line"><span class="annotation">@EActivity</span></span>
<span class="line">public <span class="class"><span class="keyword">class</span> <span class="title">MyActivity</span> <span class="keyword"><span class="keyword">extends</span></span> <span class="title">Activity</span> &#123;</span></span>
<span class="line"></span>
<span class="line">    <span class="annotation">@Pref</span></span>
<span class="line">    <span class="type">MyPrefs_</span> myPrefs;</span>
<span class="line"></span>
<span class="line">    <span class="comment">// ...</span></span>
<span class="line"></span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>

You can then start using it:

<figure class="highlight processing"><table><tr><td class="code"><pre><span class="line"><span class="comment">// Simple edit</span></span>
<span class="line">myPrefs.name().put(<span class="string">"John"</span>);</span>
<span class="line"></span>
<span class="line"><span class="comment">// Batch edit</span></span>
<span class="line">myPrefs.edit()</span>
<span class="line">  .name()</span>
<span class="line">  .put(<span class="string">"John"</span>)</span>
<span class="line">  .age()</span>
<span class="line">  .put(<span class="number">42</span>)</span>
<span class="line">  .apply();</span>
<span class="line"></span>
<span class="line"><span class="comment">// Preference clearing:</span></span>
<span class="line">myPrefs.<span class="built_in">clear</span>();</span>
<span class="line"></span>
<span class="line"><span class="comment">// Check if a value exists:</span></span>
<span class="line"><span class="built_in">boolean</span> nameExists = myPrefs.name().exists();</span>
<span class="line"></span>
<span class="line"><span class="comment">// Reading a value</span></span>
<span class="line"><span class="keyword">long</span> lastUpdated = myPrefs.lastUpdated().<span class="built_in">get</span>();</span>
<span class="line"></span>
<span class="line"><span class="comment">// Reading a value and providing a fallback default value</span></span>
<span class="line"><span class="keyword">long</span> now = System.currentTimeMillis();</span>
<span class="line"><span class="keyword">long</span> lastUpdated = myPrefs.lastUpdated().getOr(now);</span>
</pre></td></tr></table></figure>

### Default resource value
> Since AndroidAnnotations 3.0

It’s now possible to inject a default value from Android resources with `@DefaultRes`:

<figure class="highlight autoit"><table><tr><td class="code"><pre><span class="line"><span class="constant">@SharedPref</span></span>
<span class="line">public interface MyPrefs &#123;</span>
<span class="line">    <span class="constant">@DefaultRes</span>(R.<span class="built_in">string</span>.defaultPrefName)</span>
<span class="line">    <span class="built_in">String</span> resourceName()<span class="comment">;</span></span>
<span class="line"></span>
<span class="line">    <span class="constant">@DefaultRes</span> // uses <span class="string">'R.string.defaultPrefAge'</span> <span class="keyword">to</span> set <span class="keyword">default</span> value</span>
<span class="line">    <span class="built_in">String</span> defaultPrefAge()<span class="comment">;</span></span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>

### String resource as preference key
> Since AndroidAnnotations 3.1

You can specify a String resource by its id to be used as the preference key instead of the method name. This is useful when you specify your preferences in an xml file, and you use String resource keys there. Example:

<figure class="highlight cs"><table><tr><td class="code"><pre><span class="line">@SharedPref</span>
<span class="line"><span class="keyword">public</span> <span class="keyword">interface</span> <span class="title">MyPrefs</span> &#123;</span>
<span class="line">    @DefaultString(<span class="keyword">value</span> = <span class="string">"John"</span>, keyRes = R.<span class="keyword">string</span>.myPrefKey)</span>
<span class="line">    <span class="function">String <span class="title">name</span>(<span class="params"></span>)</span>;</span>
<span class="line"></span>
<span class="line">    @DefaultRes(keyRes = R.<span class="keyword">string</span>.myOtherPrefKey)</span>
<span class="line">    <span class="function">String <span class="title">defaultPrefAge</span>(<span class="params"></span>)</span>;</span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>

### Scope

Observe that you can name the shared preference by setting `value` to one of the following:

*   `ACTIVITY`, for a shared preference named `MyActivity_MyPrefs`;
*   `ACTIVITY_DEFAULT`, for a shared preference named `MyActivity` (also available through `activity.getPreferences()`);
*   `APPLICATION_DEFAULT`, for the default `SharedPreference` or `UNIQUE`, for a shared preference named `MyPrefs`.
Therefore, if a single shared preference is needed for the interface defined, in order to all activities of a given application to share the same preferences, the following should be used:
<figure class="highlight cs"><table><tr><td class="code"><pre><span class="line">@SharedPref(<span class="keyword">value</span>=SharedPref.Scope.UNIQUE)</span>
<span class="line"><span class="keyword">public</span> <span class="keyword">interface</span> <span class="title">MyPrefs</span> &#123;</span>
<span class="line">...</span>
</pre></td></tr></table></figure>

### Using it with a `PreferenceActivity`

The Android `PreferenceActivity` or `PreferenceFragment` can edit the values of your shared preferences.

<figure class="highlight groovy"><table><tr><td class="code"><pre><span class="line"><span class="annotation">@SharedPref</span>(SharedPref.Scope.UNIQUE)</span>
<span class="line"><span class="keyword">public</span> <span class="class"><span class="keyword">interface</span> <span class="title">MyPrefs</span> &#123;</span></span>
<span class="line">...</span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>
<figure class="highlight processing"><table><tr><td class="code"><pre><span class="line"><span class="keyword">public</span> <span class="keyword">static</span> <span class="keyword">String</span> PREF_NAME = <span class="string">"MyPrefs"</span>;</span>
<span class="line"></span>
<span class="line"><span class="comment">// in onCreate</span></span>
<span class="line"></span>
<span class="line"><span class="comment">// Using your MyPrefs values </span></span>
<span class="line"><span class="keyword">this</span>.getPreferenceManager().setSharedPreferencesName(PREF_NAME);</span>
<span class="line"></span>
<span class="line"><span class="comment">// Opening the layout </span></span>
<span class="line">addPreferencesFromResource(R.xml.prefs);</span>
</pre></td></tr></table></figure>