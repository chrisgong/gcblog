---
title: Vim命令
tags:
  - Vim
date: 2016-01-02 22:28:09
---

> 转载自：[http://my.oschina.net/u/1455799/blog/214448?fromerr=oMXPHQ26](http://my.oschina.net/u/1455799/blog/214448?fromerr=oMXPHQ26)

### 第一级 – 存活
<figure class="highlight asciidoc"><table><tr><td class="code"><pre><span class="line">i → Insert 模式，按 ESC 回到 Normal 模式.</span>
<span class="line"></span>
<span class="line">x → 删当前光标所在的一个字符。</span>
<span class="line"></span>
<span class="line"><span class="attribute">:wq → 存盘 + 退出 (:w</span> 存盘, :q 退出)   （陈皓注：:w 后可以跟文件名）</span>
<span class="line"></span>
<span class="line">dd → 删除当前行，并把删除的行存到剪贴板里</span>
<span class="line"></span>
<span class="line">p → 粘贴剪贴板</span>
<span class="line"></span>
<span class="line">推荐:</span>
<span class="line"></span>
<span class="line">hjkl (强例推荐使用其移动光标，但不必需) →你也可以使用光标键 (←↓↑→). 注: j 就像下箭头。</span>
<span class="line"></span>
<span class="line"><span class="attribute">:help &lt;command&gt; → 显示相关命令的帮助。你也可以就输入 :help</span> 而不跟命令。（陈皓注：退出帮助需要输入:q）</span>
</pre></td></tr></table></figure>

### 第二级 – 感觉良好

#### 各种插入模式
<figure class="highlight stylus"><table><tr><td class="code"><pre><span class="line"><span class="tag">a</span> → 在光标后插入</span>
<span class="line"></span>
<span class="line">o → 在当前行后插入一个新行</span>
<span class="line"></span>
<span class="line">O → 在当前行前插入一个新行</span>
<span class="line"></span>
<span class="line">cw → 替换从光标所在位置后到一个单词结尾的字符</span>
</pre></td></tr></table></figure>

#### 简单的移动光标
<figure class="highlight fortran"><table><tr><td class="code"><pre><span class="line"><span class="number">0</span> → 数字零，到行头</span>
<span class="line"></span>
<span class="line">^ → 到本行第一个不是<span class="keyword">blank</span>字符的位置（所谓<span class="keyword">blank</span>字符就是空格，tab，换行，回车等）</span>
<span class="line"></span>
<span class="line">$ → 到本行行尾</span>
<span class="line"></span>
<span class="line">g_ → 到本行最后一个不是<span class="keyword">blank</span>字符的位置。</span>
<span class="line"></span>
<span class="line">/pattern → 搜索 pattern 的字符串（陈皓注：如果搜索出多个匹配，可按n键到下一个）</span>
</pre></td></tr></table></figure>

#### 拷贝/粘贴
<figure class="highlight tp"><table><tr><td class="code"><pre><span class="line"><span class="keyword">P</span> → 粘贴（p/<span class="keyword">P</span>都可以，p是表示在当前位置之后，<span class="keyword">P</span>表示在当前位置之前）</span>
<span class="line"></span>
<span class="line">yy → 拷贝当前行当行于 ddP</span>
</pre></td></tr></table></figure>

#### Undo/Redo
<figure class="highlight mel"><table><tr><td class="code"><pre><span class="line">u → <span class="keyword">undo</span></span>
<span class="line"></span>
<span class="line">&lt;C-r&gt; → <span class="keyword">redo</span></span>
</pre></td></tr></table></figure>

#### 打开/保存/退出/改变文件(Buffer)
<figure class="highlight asciidoc"><table><tr><td class="code"><pre><span class="line">:e &lt;path/to/file&gt; → 打开一个文件</span>
<span class="line"></span>
<span class="line">:w → 存盘</span>
<span class="line"></span>
<span class="line">:saveas &lt;path/to/file&gt; → 另存为 &lt;path/to/file&gt;</span>
<span class="line"></span>
<span class="line"><span class="attribute">:x， ZZ 或 :wq</span> → 保存并退出 (:x 表示仅在需要时保存，ZZ不需要输入冒号并回车)</span>
<span class="line"></span>
<span class="line"><span class="attribute">:q! → 退出不保存 :qa!</span> 强行退出所有的正在编辑的文件，就算别的文件有更改。</span>
<span class="line"></span>
<span class="line"><span class="attribute">:bn 和 :bp</span> → 你可以同时打开很多文件，使用这两个命令来切换下一个或上一个文件。（陈皓注：我喜欢使用:n到下一个文件）</span>
</pre></td></tr></table></figure>

### 第三级 – 更好，更强，更快
<figure class="highlight livecodeserver"><table><tr><td class="code"><pre><span class="line">. → (小数点) 可以重复上一次的命令</span>
<span class="line"></span>
<span class="line">N&lt;<span class="command"><span class="keyword">command</span>&gt; → 重复某个命令<span class="title">N</span>次</span></span>
</pre></td></tr></table></figure>

下面是一个示例，找开一个文件你可以试试下面的命令：

> *   2dd → 删除2行
> 
> *   3p → 粘贴文本3次
> 
> *   100idesu [ESC] → 会写下 “desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu “
> 
> *   → 重复上一个命令—— 100 “desu “.
> 
> *   → 重复 3 次 “desu” (注意：不是 300，你看，VIM多聪明啊).

#### 更强

你要让你的光标移动更有效率，你一定要了解下面的这些命令，千万别跳过。

<figure class="highlight stata"><table><tr><td class="code"><pre><span class="line">NG → 到第 <span class="keyword">N</span> 行 （陈皓注：注意命令中的<span class="keyword">G</span>是大写的，另我一般使用 : <span class="keyword">N</span> 到第<span class="keyword">N</span>行，如 :137 到第137行）</span>
<span class="line"></span>
<span class="line">gg → 到第一行。（陈皓注：相当于1G，或 :1）</span>
<span class="line"></span>
<span class="line"><span class="keyword">G</span> → 到最后一行。</span>
</pre></td></tr></table></figure>

按单词移动：

> 如果你认为单词是由默认方式，那么就用小写的e和w。默认上来说，一个单词由字母，数字和下划线组成（陈皓注：程序变量）
> 
> 如果你认为单词是由blank字符分隔符，那么你需要使用大写的E和W。（陈皓注：程序语句）

![](http://static.oschina.net/uploads/img/201403/30213311_pgWj.jpg)

1.  w → 到下一个单词的开头。

2.  e → 到下一个单词的结尾。

下面，让我来说说最强的光标移动：

> *   % : 匹配括号移动，包括 (, {, [. （陈皓注：你需要把光标先移到括号上）
> 
> *   和 #:  匹配光标当前所在的单词，移动光标到下一个（或上一个）匹配单词（*是下一个，#是上一个）

#### 更快

你一定要记住光标的移动，因为很多命令都可以和这些移动光标的命令连动。很多命令都可以如下来干：

<start position=""><command><end position=""></end></start>

例如 0y$ 命令意味着：

*   0 → 先到行头

*   y → 从这里开始拷贝

*   $ → 拷贝到本行最后一个字符

你可可以输入 ye，从当前位置拷贝到本单词的最后一个字符。

你也可以输入 y2/foo 来拷贝2个 “foo” 之间的字符串。

还有很多时间并不一定你就一定要按y才会拷贝，下面的命令也会被拷贝：

*   d (删除 )

*   v (可视化的选择)

*   gU (变大写)

*   gu (变小写)

*   等等

### 第四级 – Vim 超能力

你只需要掌握前面的命令，你就可以很舒服的使用VIM了。但是，现在，我们向你介绍的是VIM杀手级的功能。下面这些功能是我只用vim的原因。

在当前行上移动光标: 0 ^ $ f F t T , ;

> *   0 → 到行头
> 
> *   ^ → 到本行的第一个非blank字符
> 
> *   $ → 到行尾
> 
> *   g_ → 到本行最后一个不是blank字符的位置。
> 
> *   fa → 到下一个为a的字符处，你也可以fs到下一个为s的字符。
> 
> *   t, → 到逗号前的第一个字符。逗号可以变成其它字符。
> 
> *   3fa → 在当前行查找第三个出现的a。
> 
> *   F 和 T → 和 f 和 t 一样，只不过是相反方向。

![](http://static.oschina.net/uploads/img/201403/30213311_S4ef.jpg)

还有一个很有用的命令是 dt” → 删除所有的内容，直到遇到双引号—— “。

#### 区域选择 `&lt;action&gt;a&lt;object&gt; 或 &lt;action&gt;i&lt;object&gt;`

在visual 模式下，这些命令很强大，其命令格式为

`&lt;action&gt;a&lt;object&gt; 和 &lt;action&gt;i&lt;object&gt;`

*   action可以是任何的命令，如 d (删除), y (拷贝), v (可以视模式选择)。

*   object 可能是： w 一个单词， W 一个以空格为分隔的单词， s 一个句字， p 一个段落。也可以是一个特别的字符：”、 ‘、 )、 }、 ]。

假设你有一个字符串 (map (+) (“foo”)).而光标键在第一个 o 的位置。

> *   vi” → 会选择 foo.
> 
> *   va” → 会选择 “foo”.
> 
> *   vi) → 会选择 “foo”.
> 
> *   va) → 会选择(“foo”).
> 
> *   v2i) → 会选择 map (+) (“foo”)
> 
> *   v2a) → 会选择 (map (+) (“foo”))

![](http://static.oschina.net/uploads/img/201403/30213319_fpAj.png)

#### 块操作: `&lt;C-v&gt;`

块操作，典型的操作： `0 &lt;C-v&gt; &lt;C-d&gt; I-- [ESC]`

*   ^ → 到行头
*   `&lt;C-v&gt;` → 开始块操作
*   `&lt;C-d&gt;` → 向下移动 (你也可以使用hjkl来移动光标，或是使用%，或是别的)
*   I– [ESC] → I是插入，插入“–”，按ESC键来为每一行生效。

![](http://static.oschina.net/uploads/img/201403/30213319_eOK2.gif)

在Windows下的vim，你需要使用 `&lt;C-q&gt;` 而不是 `&lt;C-v&gt;` ，`&lt;C-v&gt;` 是拷贝剪贴板。

#### 自动提示： `&lt;C-n&gt; 和 &lt;C-p&gt;`

在 Insert 模式下，你可以输入一个词的开头，然后按 `&lt;C-p&gt;或是&lt;C-n&gt;`，自动补齐功能就出现了……

![](http://static.oschina.net/uploads/img/201403/30213320_pPoI.gif)

#### 宏录制： qa 操作序列 q, @a , @@

*   qa 把你的操作记录在寄存器 a。

*   于是 @a  会replay被录制的宏。

*   @@ 是一个快捷键用来replay最新录制的宏。
> **_示例_**
> 
> 在一个只有一行且这一行只有“1”的文本中，键入如下命令：
> 
> *   qaYp<c-a>q→</c-a>
> 
>         *   qa 开始录制
> 
>         *   Yp 复制行.
> 
>         *   `&lt;C-a&gt;` 增加1.
> 
>         *   q 停止录制.
> 
> *   @a  → 在1下面写下 2
> 
> *   @@ → 在2 正面写下3
> 
> *   现在做 100@@ 会创建新的100行，并把数据增加到 103.

![](http://static.oschina.net/uploads/img/201403/30213321_axRo.gif)

#### 可视化选择： v,V,`&lt;C-v&gt;`

前面，我们看到了 `&lt;C-v&gt;`的示例 （在Windows下应该是`&lt;C-q&gt;`），我们可以使用 v 和 V。一但被选好了，你可以做下面的事：

*   J → 把所有的行连接起来（变成一行）

*   &lt; 或 &gt; → 左右缩进

*   = → 自动给缩进

![](http://static.oschina.net/uploads/img/201403/30213324_dgd1.gif)

在所有被选择的行后加上点东西：

*   `&lt;C-v&gt;`

*   选中相关的行 (可使用 j 或 `&lt;C-d&gt;` 或是 /pattern 或是 % 等……)

*   $ 到行最后

*   A, 输入字符串，按 ESC。

![](http://static.oschina.net/uploads/img/201403/30213326_mQtz.gif)

#### 分屏: :split 和 vsplit.

下面是主要的命令，你可以使用VIM的帮助 :help split. 你可以参考本站以前的一篇文章VIM分屏。

> *   :split → 创建分屏 (:vsplit创建垂直分屏)
> 
> *   `&lt;C-w&gt;&lt;dir&gt;` : dir就是方向，可以是 hjkl 或是 ←↓↑→ 中的一个，其用来切换分屏。
> 
> *   `&lt;C-w&gt;_` (或 `&lt;C-w&gt;|`) : 最大化尺寸 (`&lt;C-w&gt;|` 垂直分屏)
> 
> *   `&lt;C-w&gt;+` (或 `&lt;C-w&gt;-`) : 增加尺寸

![](http://static.oschina.net/uploads/img/201403/30213328_ANuy.gif)