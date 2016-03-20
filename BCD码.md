---
title: BCD码
tags:
  - Algorithm
date: 2015-12-28 23:17:30
---

#### 简介
> BCD码（Binary-Coded Decimal‎）亦称二进码十进数或二-十进制代码。用4位二进制数来表示1位十进制数中的0~9这10个数码。是一种二进制的数字编码形式，用二进制编码的十进制代码。BCD码这种编码形式利用了四个位元来储存一个十进制的数码，使二进制和十进制之间的转换得以快捷的进行。这种编码技巧最常用于会计系统的设计里，因为会计制度经常需要对很长的数字串作准确的计算。相对于一般的浮点式记数法，采用BCD码，既可保存数值的精确度，又可免去使电脑作浮点运算时所耗费的时间。此外，对于其他需要高精确度的计算，BCD编码亦很常用。

BCD码可分为有权码和无权码两类：有权BCD码有8421码、2421码、5421码，其中8421码是最常用的；无权BCD码有余3码，余3循环码等。

最常用的BCD编码，就是使用”0”至”9”这十个数值的二进码来表示。这种编码方式，在称之为“8421码”（日常所说的BCD码大都是指8421BCD码形式）。

附注：压缩BCD码与非压缩BCD码的区别—— 压缩BCD码的每一位用4位二进制表示，一个字节表示两位十进制数。例如10010110B表示十进制数96D；非压缩BCD码用1个字节表示一位十进制数，高四位总是0000，低4位的0000~1001表示0~9.例如00001000B表示十进制数8.

#### BCD码的运算法则

BCD码的运算规则：BCD码是十进制数，而运算器对数据做加减运算时，都是按二进制运算规则进行处理的。这样，当将 BCD码传送给运算器进行运算时，其结果需要修正。修正的规则是：当两个BCD码相加，如果和等于或小于 1001(即十进制数9)，不需要修正；如果相加之和在 1010 到1111(即十六进制数 0AH～0FH)之间，则需加 6 进行修正；如果相加时，本位产生了进位，也需加 6 进行修正。这样做的原因是，机器按二进制相加，所以 4 位二进制数相加时，是按“逢十六进一”的原则进行运算的，而实质上是 2 个十进制数相加，应该按“逢十进一”的原则相加，16 与10相差 6，所以当和超过 9或有进位时，都要加 6 进行修正。下面举例说明。

1.  计算 5+8；

    将 5 和 8 以 8421 BCD输入机器，则运算如下：

    0 1 0 1

    +) 1 0 0 0

    1 1 0 1 结果大于 9

    +) 0 1 1 0 加 6 修正

    1 0 0 1 1 即13 的 BCD码

    结果是 0011，即十进制数3，还产生了进位。5+8=13，结论正确。

2.  计算 8+8

     将8以8421 BCD输入机器，则运算如下：

     1 0 0 0

     +）1 0 0 0

     1 0 0 0 0 结果大于9

     +）0 1 1 0 加6修正

     1 0 1 1 0 16的BCD码

     结果是0110，即十进制的6，而且产生进位。8+8=16，结论正确。

#### Java BCD方法
<figure class="highlight cpp"><table><tr><td class="code"><pre><span class="line"><span class="comment">/**</span>
<span class="line"> * Created by chrisgong on 15/12/28.</span>
<span class="line"> */</span></span>
<span class="line"><span class="keyword">public</span> <span class="keyword">class</span> BCDUtils &#123;</span>
<span class="line"></span>
<span class="line">    <span class="comment">/**</span>
<span class="line">     * @功能: BCD码转为10进制串(阿拉伯数据)</span>
<span class="line">     * @参数: BCD码</span>
<span class="line">     * @结果: 10进制串</span>
<span class="line">     */</span></span>
<span class="line">    <span class="function"><span class="keyword">public</span> <span class="keyword">static</span> String <span class="title">bcd2Str</span><span class="params">(byte[] bytes)</span> </span>&#123;</span>
<span class="line">        StringBuffer temp = <span class="keyword">new</span> StringBuffer(bytes.length * <span class="number">2</span>);</span>
<span class="line">        <span class="keyword">for</span> (<span class="keyword">int</span> i = <span class="number">0</span>; i &lt; bytes.length; i++) &#123;</span>
<span class="line">            temp.append((byte) ((bytes[i] &amp; <span class="number">0xf0</span>) &gt;&gt;&gt; <span class="number">4</span>));</span>
<span class="line">            temp.append((byte) (bytes[i] &amp; <span class="number">0x0f</span>));</span>
<span class="line">        &#125;</span>
<span class="line">        <span class="keyword">return</span> temp.toString().substring(<span class="number">0</span>, <span class="number">1</span>).equalsIgnoreCase(<span class="string">"0"</span>) ? temp</span>
<span class="line">                .toString().substring(<span class="number">1</span>) : temp.toString();</span>
<span class="line">    &#125;</span>
<span class="line"></span>
<span class="line">    <span class="comment">/**</span>
<span class="line">     * @功能: 10进制串转为BCD码</span>
<span class="line">     * @参数: 10进制串</span>
<span class="line">     * @结果: BCD码</span>
<span class="line">     */</span></span>
<span class="line">    <span class="keyword">public</span> <span class="keyword">static</span> byte[] str2Bcd(String asc) &#123;</span>
<span class="line">        <span class="keyword">int</span> len = asc.length();</span>
<span class="line">        <span class="keyword">int</span> mod = len % <span class="number">2</span>;</span>
<span class="line">        <span class="keyword">if</span> (mod != <span class="number">0</span>) &#123;</span>
<span class="line">            asc = <span class="string">"0"</span> + asc;</span>
<span class="line">            len = asc.length();</span>
<span class="line">        &#125;</span>
<span class="line">        byte abt[] = <span class="keyword">new</span> byte[len];</span>
<span class="line">        <span class="keyword">if</span> (len &gt;= <span class="number">2</span>) &#123;</span>
<span class="line">            len = len / <span class="number">2</span>;</span>
<span class="line">        &#125;</span>
<span class="line">        byte bbt[] = <span class="keyword">new</span> byte[len];</span>
<span class="line">        abt = asc.getBytes();</span>
<span class="line">        <span class="keyword">int</span> j, k;</span>
<span class="line">        <span class="keyword">for</span> (<span class="keyword">int</span> p = <span class="number">0</span>; p &lt; asc.length() / <span class="number">2</span>; p++) &#123;</span>
<span class="line">            <span class="keyword">if</span> ((abt[<span class="number">2</span> * p] &gt;= <span class="string">'0'</span>) &amp;&amp; (abt[<span class="number">2</span> * p] &lt;= <span class="string">'9'</span>)) &#123;</span>
<span class="line">                j = abt[<span class="number">2</span> * p] - <span class="string">'0'</span>;</span>
<span class="line">            &#125; <span class="keyword">else</span> <span class="keyword">if</span> ((abt[<span class="number">2</span> * p] &gt;= <span class="string">'a'</span>) &amp;&amp; (abt[<span class="number">2</span> * p] &lt;= <span class="string">'z'</span>)) &#123;</span>
<span class="line">                j = abt[<span class="number">2</span> * p] - <span class="string">'a'</span> + <span class="number">0x0a</span>;</span>
<span class="line">            &#125; <span class="keyword">else</span> &#123;</span>
<span class="line">                j = abt[<span class="number">2</span> * p] - <span class="string">'A'</span> + <span class="number">0x0a</span>;</span>
<span class="line">            &#125;</span>
<span class="line">            <span class="keyword">if</span> ((abt[<span class="number">2</span> * p + <span class="number">1</span>] &gt;= <span class="string">'0'</span>) &amp;&amp; (abt[<span class="number">2</span> * p + <span class="number">1</span>] &lt;= <span class="string">'9'</span>)) &#123;</span>
<span class="line">                k = abt[<span class="number">2</span> * p + <span class="number">1</span>] - <span class="string">'0'</span>;</span>
<span class="line">            &#125; <span class="keyword">else</span> <span class="keyword">if</span> ((abt[<span class="number">2</span> * p + <span class="number">1</span>] &gt;= <span class="string">'a'</span>) &amp;&amp; (abt[<span class="number">2</span> * p + <span class="number">1</span>] &lt;= <span class="string">'z'</span>)) &#123;</span>
<span class="line">                k = abt[<span class="number">2</span> * p + <span class="number">1</span>] - <span class="string">'a'</span> + <span class="number">0x0a</span>;</span>
<span class="line">            &#125; <span class="keyword">else</span> &#123;</span>
<span class="line">                k = abt[<span class="number">2</span> * p + <span class="number">1</span>] - <span class="string">'A'</span> + <span class="number">0x0a</span>;</span>
<span class="line">            &#125;</span>
<span class="line">            <span class="keyword">int</span> a = (j &lt;&lt; <span class="number">4</span>) + k;</span>
<span class="line">            byte b = (byte) a;</span>
<span class="line">            bbt[p] = b;</span>
<span class="line">        &#125;</span>
<span class="line">        <span class="keyword">return</span> bbt;</span>
<span class="line">    &#125;</span>
<span class="line">&#125;</span>
</pre></td></tr></table></figure>