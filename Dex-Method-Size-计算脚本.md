---
title: Dex Method Size 计算脚本
tags:
  - Android
date: 2015-12-27 22:43:17
---

The script：

<figure class="highlight bash"><table><tr><td class="code"><pre><span class="line"><span class="shebang">#!/bin/bash</span>
<span class="line"></span></span>
<span class="line">SDK_DIR=<span class="variable">$1</span> <span class="comment"># SDK dexdump路径</span></span>
<span class="line">APK_DIR=<span class="variable">$2</span>  <span class="comment"># 需要计算的APK地址</span></span>
<span class="line"></span>
<span class="line"><span class="variable">$1</span>/./dexdump <span class="operator">-f</span> <span class="variable">$2</span> | grep method_ids_size</span>
</pre></td></tr></table></figure>

The results:

<figure class="highlight groovy"><table><tr><td class="code"><pre><span class="line">➜  temp  .<span class="regexp">/dex_method.sh /</span>Users<span class="regexp">/chrisgong/</span>Library<span class="regexp">/Android/</span>sdk<span class="regexp">/build-tools/</span><span class="number">23.0</span><span class="number">.2</span> <span class="regexp">~/mac/</span>git<span class="regexp">/cim-android/</span>build<span class="regexp">/outputs/</span>apk/cim-android-debug-v1<span class="number">.3</span><span class="number">.2</span>.apk</span>
<span class="line"></span>
<span class="line"><span class="string">method_ids_size     :</span> <span class="number">40899</span></span>
</pre></td></tr></table></figure>