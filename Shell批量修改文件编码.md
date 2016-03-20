---
title: Shell批量修改文件编码
tags:
  - Shell
date: 2015-11-27 14:07:34
---

> 第一个Shell脚本，很成功，值得记录一下。
<figure class="highlight stata"><table><tr><td class="code"><pre><span class="line">inputDir=`<span class="keyword">ls</span> ~/<span class="keyword">mac</span>/temp/SDK\ v1.2.0.F源文件/`</span>
<span class="line">outDir=~/<span class="keyword">mac</span>/temp/<span class="keyword">out</span>/</span>
<span class="line"></span>
<span class="line"><span class="keyword">for</span> <span class="keyword">file</span> <span class="keyword">in</span> <span class="label">$inputDir</span></span>
<span class="line"><span class="keyword">do</span></span>
<span class="line"> new_file=<span class="string">"$outDir$file"</span></span>
<span class="line"> iconv -f GBK -t UTF-8 <span class="label">$&#123;file&#125;</span> &gt; <span class="label">$&#123;new_file&#125;</span> </span>
<span class="line">done</span>
</pre></td></tr></table></figure>
> 改进版
<figure class="highlight bash"><table><tr><td class="code"><pre><span class="line"><span class="shebang">#!/bin/bash</span>
<span class="line"></span></span>
<span class="line">DIR=<span class="variable">$1</span> <span class="comment"># 转换编码文件目录</span></span>
<span class="line">FT=<span class="variable">$2</span>  <span class="comment"># 需要转换的文件类型（扩展名）</span></span>
<span class="line">SE=<span class="variable">$3</span>  <span class="comment"># 原始编码</span></span>
<span class="line">DE=<span class="variable">$4</span>  <span class="comment"># 目标编码</span></span>
<span class="line"></span>
<span class="line"><span class="keyword">for</span> file <span class="keyword">in</span> `find <span class="variable">$DIR</span> -type f -name <span class="string">"*.<span class="variable">$FT</span>"</span>`; <span class="keyword">do</span></span>
<span class="line">	<span class="built_in">echo</span> <span class="string">"conversion <span class="variable">$file</span> encoding <span class="variable">$SE</span> to <span class="variable">$DE</span>"</span></span>
<span class="line">    iconv <span class="operator">-f</span> <span class="variable">$SE</span> -t <span class="variable">$DE</span> <span class="string">"<span class="variable">$file</span>"</span> &gt; <span class="string">"<span class="variable">$file</span>"</span>.tmp</span>
<span class="line">    mv <span class="operator">-f</span> <span class="string">"<span class="variable">$file</span>"</span>.tmp <span class="string">"<span class="variable">$file</span>"</span></span>
<span class="line"><span class="keyword">done</span></span>
</pre></td></tr></table></figure>