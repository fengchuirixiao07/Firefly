---
title: basic crypto
published: 2026-02-17
description: ctf中密码学的入门知识点
image: C:\Users\pc\Desktop\照片\834c8cb76c58741f7d5ff9e6664cc448.jpg
tags: [ctf,crypto]
category: crypto
draft: false
---

<p><strong>1.RSA加密</strong>  </p>
<blockquote>
<p><br><strong>1.同余：</strong></p>
<blockquote>
<p><br> &emsp;&emsp; $a \equiv b \pmod{m}$</p>
<blockquote>
<p><br>1.1 逆元：
<br> &emsp;&emsp; $a \times x \equiv 1 \pmod{m}$
&emsp;&emsp;    $x \equiv a^{-1} \pmod{m}$&emsp;&emsp;则称x为a模m的逆元</p>
</blockquote>
</blockquote>
<p><br><strong>2.RSA加密原理：</strong></p>
<blockquote>
<p><br> n为模数&emsp;&emsp;e为公钥指数&emsp;&emsp;c为密文&emsp;&emsp;m为明文
<br>​c ≡ m^e (mod n)</p>
</blockquote>
<p><br><strong>3.RSA解密原理:</strong></p>
<blockquote>
<p><br>3.1 选择两个大质数 p和 q，并计算模数 n和欧拉函数 φ(n):
<br>n = p × q
<br>ψ(n) = (p-1) × (q-1)&emsp;&emsp;m^e ≡ C (mod n)&emsp;&emsp;c^d ≡ m (mod n)
<br>m^(e×d) ≡ m (mod n)
<br>3.2 其是依靠欧拉公式：
<br>&emsp;&emsp;m^φ(n) ≡ 1 (mod n)
<br>3.3 其后续可以变为：
<br>&emsp;&emsp;m^(k × φ(n) + 1) ≡ m (mod n)
<br>&emsp;&emsp;e×d = k×ψ(n)+1 ⇒ e×d ≡ 1 (mod ψ(n))
<br>&emsp;&emsp;d = e^(-1) mod ψ(n)&emsp;&emsp;<strong>or</strong>&emsp;&emsp;d = pow(e, -1, ψ(n))
<br>&emsp;&emsp;m = c^d mod(n)
<br>
<br></p>
</blockquote>
</blockquote>
<p><strong>2.共享素数攻击</strong></p>
<blockquote>
<p><br>通过gcd（n1,n2) = 共享素数p</p>
<blockquote>
<p><br>q1 = n1 // p&emsp;&rArr;φ (n1) = (p-1)(q1-1)&emsp;&rArr;d1=e^(-1)mod/φ(n1)
<br>q2 = n2 // p&emsp;&rArr;φ (n2) = (p-1)(q2-1)&emsp;&rArr;d2=e^(-1)mod/φ(n2)
<br>m1 = c^(d1)mod n1
<br>
<br></p>
</blockquote>
</blockquote>
<p><strong>3.中国剩余定理</strong></p>
<blockquote>
<p><br>$$
\begin{cases}
x \equiv a<em>{1} \pmod{m</em>{1}} \
x \equiv a<em>{2} \pmod{m</em>{2}} \
\vdots \
x \equiv a<em>{k} \pmod{m</em>{k}}
\end{cases}</p>
<blockquote>
<p>模M = m<sub>1</sub> <em> m<sub>2</sub> </em> m<sub>3</sub> <em> …… </em> m<sub>k</sub>
<br>M<sub>i</sub> = M /m<sub>i</sub>&emsp;&emsp;&emsp;逆元x = pow（M<sub>i</sub>,-1,m<sub>i</sub>)</p>
<blockquote>
<p><br>x ≡ (a<sub>1</sub> <em> M<sub>1</sub> </em> x<sub>1</sub> + a<sub>2</sub> <em> M<sub>2</sub> </em> x<sub>2</sub>+……+a<sub>k</sub> <em> M<sub>k</sub> </em> x<sub>k</sub>)mod(M)</p>
<pre><code class="lang-python"><span class="hljs-keyword">import</span> Crypto.Util.number from *
<span class="hljs-keyword">import</span> gmpy2
<span class="hljs-title">me</span> = gmpy2.iroot(x,k)[<span class="hljs-number">0</span>]
</code></pre>
<p><strong>4.小明文攻击</strong>
<br>只是适用于c 与 n的数量级差距很大时及m^e mod n = c，m^e =c
直接开方运算
<br>$m^e$ &lt; n
<br> c =&gt;m^emod(n)
<br> c = &gt;m^e</p>
<pre><code class="lang-python"><span class="hljs-keyword">import</span> Crypto.Util.number from *
<span class="hljs-keyword">import</span> gmpy2
m = gmpy2.iroot(<span class="hljs-built_in">c</span>,e)[<span class="hljs-number">0</span>]
</code></pre>
<p><strong>5.费马小定理</strong>
<br>形式一：如果 p是一个质数，且整数 a不是 p的倍数（即 a与 p互质，gcd(a,p)=1)，那么有：
<br>&emsp;&emsp;a^(p - 1)  ≡ 1 mod(p)
<br>形式二：如果 p是一个质数，对于任意整数 a，有：
<br>&emsp;&emsp;a^p ≡  p mod(p)
<br>临近素数问题
<br><em>1.平方差遍历法</em>
<br>如果素数 p和 q临近，那么它们的算术平均值 a与它们之间的差值 b都会很小。我们可以将模数 N=p×q表示为一个平方差公式：
设 p=a−b，q=a+b。 其中 a 和 b 的定义为：a =(p + q)//2&emsp;&emsp; b = (q - p）//2
<br>&emsp;&emsp;N=p×q=(a−b)(a+b)=a^2 - b^2
<br>b^2 = a^2 -N
```python
from Crypto.Util.number import <em>
from sympy import </em>
import gmpy2
n = 122719648746679660211272134136414102389555796575857405114496972248651220892565781331814993584484991300852578490929023084395318478514528533234617759712503439058334479192297581245539902950267201362675602085964421659147977335779128546965068649265419736053467523009673037723382969371523663674759921589944204926693
e = 65537
c = 109215817118156917306151535199288935588358410885541150319309172366532983941498151858496142368333375769194040807735053625645757204569614999883828047720427480384683375435683833780686557341909400842874816853528007258975117265789241663068590445878241153205106444357554372566670436865722966668420239234530554168928
a = gmpy2.iroot(n,2)[0]
while 1:
    b_2 = a<em>a - n
    if gmpy2.is_square(b_2):
        b = gmpy2.iroot(b_2,2)[0]
        p = a - b
        q = a + b
        if p </em> q == n:<br>            break
    a += 1
&#39;&#39;&#39;assert pow(666666, x, p) == 1&#39;&#39;&#39;
x = p - 1
phi = (p - 1) * (q - 1)
d = pow(e,-1,phi)
m<em> = pow(c,d,n)
m = m</em> ^ x</p>
</blockquote>
</blockquote>
</blockquote>
<p>print(long_to_bytes(m))</p>
<pre><code>&gt;&gt;&gt;&lt;br&gt;*<span class="hljs-number">2</span>.相邻素数法*
```python
from Crypto.Util.number <span class="hljs-built_in">import</span> *
<span class="hljs-built_in">import</span> gmpy2
<span class="hljs-attr">e</span> = <span class="hljs-number">65537</span>
<span class="hljs-attr">n</span> = <span class="hljs-number">169522900072954416356051647146585827691225327527086797334523482640452305793443986277933900273961829438217255938808371865341750200444086653241610669340348513884285892043530862971785487294831341653909852543469963032532560079879299447677636753647721541724969084825510405349373420839032990681851700075554428485967</span>
<span class="hljs-attr">c</span> = <span class="hljs-number">105943762023156641770119141175498496686312095002592803768522760959533958364969985856505466722378959991757667341747887520146437729810252085791886309974903778546814812093444837674447485802109225767800488527376777153844313243366001288246744190001997192598159277512188417272938455513900277907186067996704043274199</span>
<span class="hljs-attr">t=gmpy2.iroot(n,2)</span> 
<span class="hljs-attr">p=gmpy2.next_prime(t[0])</span> 
<span class="hljs-attr">x=(p-1)*1024</span>
<span class="hljs-attr">q=n</span> // p 
<span class="hljs-attr">phi</span> = (p - <span class="hljs-number">1</span>) * (q - <span class="hljs-number">1</span>) 
<span class="hljs-attr">d=inverse(e,phi)</span> 
<span class="hljs-attr">m_</span> = pow (c,d,n) 
<span class="hljs-attr">m</span> = m_ ^ x 
print(long_to_bytes(m))
</code></pre><blockquote>
<blockquote>
<p><br><strong>适用范围差异</strong></p>
<blockquote>
<p><br>1.平方差遍历法 适用条件 p和q相近&emsp;&emsp;&emsp;&emsp;适用条件 p和q相近&emsp;&emsp;&emsp;&emsp;搜索范围 从√n开始线性搜索 &emsp;&emsp;&emsp;&emsp;成功率 较高（只要p,q相近）
<br>2.直接找相邻素数法 &emsp;&emsp;&emsp;&emsp;适用条件 p和q相近&emsp;&emsp;&emsp;&emsp;搜索范围 只检查一个特定的素数&emsp;&emsp;&emsp;&emsp;较低（依赖特定假设）</p>
</blockquote>
</blockquote>
</blockquote>
<p><strong>6.dp泄露</strong></p>
<blockquote>
<p>其形式：dp = d mod (p-1)</p>
<blockquote>
<p><br>&emsp;&emsp;e×d = k×ψ(n)+1 ⇒ e×d ≡ 1 (mod ψ(n))
<br>&emsp;&emsp;e×dp ≡ 1 (mod (p-1))
<br>&emsp;&emsp;e×dp ≡ 1 + k * (p-1)
<br>&emsp;&emsp;(e×dp-1)//k = p-1</p>
<pre><code class="lang-python">from Crypto.Util.number <span class="hljs-built_in">import</span> *
<span class="hljs-built_in">import</span> gmpy2
<span class="hljs-attr">n</span> = <span class="hljs-number">110231451148882079381796143358970452100202953702391108796134950841737642949460527878714265898036116331356438846901198470479054762675790266666921561175879745335346704648242558094026330525194100460497557690574823790674495407503937159099381516207615786485815588440939371996099127648410831094531405905724333332751</span>
<span class="hljs-attr">dp</span> = <span class="hljs-number">3086447084488829312768217706085402222803155373133262724515307236287352098952292947424429554074367555883852997440538764377662477589192987750154075762783925</span>
<span class="hljs-attr">c</span> = <span class="hljs-number">59325046548488308883386075244531371583402390744927996480498220618691766045737849650329706821216622090853171635701444247741920578127703036446381752396125610456124290112692914728856924559989383692987222821742728733347723840032917282464481629726528696226995176072605314263644914703785378425284460609365608120126</span>
<span class="hljs-attr">e</span> = <span class="hljs-number">65537</span>
for i <span class="hljs-keyword">in</span> range(<span class="hljs-number">1</span>,e):
    <span class="hljs-keyword">if</span> (dp*e-<span class="hljs-number">1</span>)%<span class="hljs-attr">i</span> == <span class="hljs-number">0</span>:
        <span class="hljs-keyword">if</span> (n%((dp*e-<span class="hljs-number">1</span>)//i+<span class="hljs-number">1</span>)) == <span class="hljs-number">0</span>:
            <span class="hljs-attr">p</span> = (dp*e-<span class="hljs-number">1</span>)//i+<span class="hljs-number">1</span>
            <span class="hljs-attr">q</span> = n // p
            <span class="hljs-attr">phi_n</span> = (p-<span class="hljs-number">1</span>)*(q-<span class="hljs-number">1</span>)
            <span class="hljs-attr">d</span> = pow(e,-<span class="hljs-number">1</span>,phi_n)
            <span class="hljs-attr">m</span> = pow(c,d,n)
print(long_to_bytes(m))
</code></pre>
<p><em>补充定理</em>
<br>定理：比如m = c ^d mod n 
<br>n = p<em>q(p,q 都是素数)那么 m = c^d mod p &emsp;&emsp;</em>or<em>&emsp;&emsp; m = c^d mod q 
<br>phi =（p -1）</em>(q -1),也可以以此类推，找其与e互质的部分满足逆元使用条件</p>
</blockquote>
</blockquote>
<p><strong>7.线性同余生成器（lcg）</strong></p>
<blockquote>
<p><br>递推公式：X <sub>n+1</sub>=(a×X <sub>n</sub>+b)modm</p>
<blockquote>
<p><br>参数说明：X <sub>n</sub>：当前状态（种子） &emsp;&emsp;a：乘数 &emsp;&emsp;b：增量 &emsp;&emsp;m：模数
<br></p>
<blockquote>
<p><br>求初始Seed：
<br> &emsp;&emsp;x<sub>n</sub>≡(x<sub>n+1</sub> - b) <em> a^(-1) mod(m)
```python
from Crypto.Util.number import </em>
import gmpy2
flag = b&#39;NSSCTF{<strong>**</strong>}&#39;
a = 113439939100914101419354202285461590291215238896870692949311811932229780896397
b = 72690056717043801599061138120661051737492950240498432137862769084012701248181
m = 72097313349570386649549374079845053721904511050364850556329251464748004927777
c = 9772191239287471628073298955242262680551177666345371468122081567252276480156
class LCG:
    def <strong>init</strong>(self, seed, a, b, m):
        self.seed = seed<br>        self.a = a 
        self.b = b 
        self.m = m </p>
</blockquote>
</blockquote>
</blockquote>
<pre><code><span class="hljs-function"><span class="hljs-keyword">def</span> <span class="hljs-title">generate</span><span class="hljs-params">(<span class="hljs-keyword">self</span>)</span></span>:
 <span class="hljs-keyword">self</span>.seed = (<span class="hljs-keyword">self</span>.a * <span class="hljs-keyword">self</span>.seed + <span class="hljs-keyword">self</span>.b) % <span class="hljs-keyword">self</span>.m
 <span class="hljs-keyword">return</span> <span class="hljs-keyword">self</span>.seed
</code></pre><p>a<em> = pow(a,-1,m)
for i in range(2 <em>* 16):
    c = (c - b) </em> a</em> % m
    flag = (long_to_bytes(c))
    if b&#39;NSSCTF{&#39; in flag:<br>        print(flag)
        break</p>
<pre><code>&gt;&gt;&gt;&lt;<span class="hljs-keyword">br&gt;求a：
</span>&lt;<span class="hljs-keyword">br&gt;&amp;emsp;&amp;emsp;x&lt;sub&gt;n+1&lt;/sub&gt;=(a </span>* x&lt;<span class="hljs-keyword">sub&gt;n&lt;/sub&gt; </span>+  <span class="hljs-keyword">b)mod(m)
</span>&lt;<span class="hljs-keyword">br&gt;&amp;emsp;&amp;emsp;x&lt;sub&gt;n&lt;/sub&gt;=(a </span>* x&lt;<span class="hljs-keyword">sub&gt;n-1&lt;/sub&gt; </span>+  <span class="hljs-keyword">b)mod(m)
</span>&lt;<span class="hljs-keyword">br&gt;&amp;emsp;&amp;emsp;x&lt;sub&gt;n+1&lt;/sub&gt; </span>- x&lt;<span class="hljs-keyword">sub&gt;n&lt;/sub&gt; </span>= (a * (x&lt;<span class="hljs-keyword">sub&gt;n&lt;/sub&gt; </span>- x&lt;<span class="hljs-keyword">sub&gt;n-1&lt;/sub&gt; </span>))mod(m)
&lt;<span class="hljs-keyword">br&gt;&amp;emsp;&amp;emsp;a </span>≡ (x&lt;<span class="hljs-keyword">sub&gt;n+1&lt;/sub&gt; </span>- x&lt;<span class="hljs-keyword">sub&gt;n&lt;/sub&gt;) </span>* (x&lt;<span class="hljs-keyword">sub&gt;n&lt;/sub&gt; </span>- x&lt;<span class="hljs-keyword">sub&gt;n-1&lt;/sub&gt; </span>)^(-<span class="hljs-number">1</span>) mod (m)
```python
<span class="hljs-symbol">from</span> Crypto.Util.number <span class="hljs-meta">import</span> *
<span class="hljs-symbol">import</span> gmpy2
<span class="hljs-symbol">flag</span> = <span class="hljs-keyword">b'NSSCTF{******}'
</span>a = <span class="hljs-number">83968440254358975953360088805517488739689448515913931281582194839594954862517</span>
m = <span class="hljs-number">77161425490597512806099499399561161959645895427463118872087051902811605680317</span>

<span class="hljs-symbol">c1</span> = <span class="hljs-number">43959768681328408257423567932475057408934775157371406900460140947365416240650</span>

<span class="hljs-symbol">c2</span> = <span class="hljs-number">8052043336238864355872102889254781281466728072798160448260752595038552944808</span>
<span class="hljs-symbol">class</span> LCG:
    def __init__(<span class="hljs-keyword">self, </span>seed, a, <span class="hljs-keyword">b, </span>m):
     <span class="hljs-keyword">self.seed </span>= seed 
     <span class="hljs-keyword">self.a </span>= a 
     <span class="hljs-keyword">self.b </span>= <span class="hljs-keyword">b </span>
     <span class="hljs-keyword">self.m </span>= m 
    def generate(<span class="hljs-keyword">self):
</span>        <span class="hljs-keyword">self.seed </span>= (<span class="hljs-keyword">self.a </span>* <span class="hljs-keyword">self.seed </span>+ <span class="hljs-keyword">self.b) </span>% <span class="hljs-keyword">self.m
</span>        return <span class="hljs-keyword">self.seed
</span>
<span class="hljs-symbol">lcg</span> = LCG(<span class="hljs-keyword">bytes_to_long(flag), </span>getPrime(<span class="hljs-number">256</span>), getPrime(<span class="hljs-number">256</span>), getPrime(<span class="hljs-number">256</span>))
<span class="hljs-symbol">a_</span> = pow(a,-<span class="hljs-number">1</span>,m)
<span class="hljs-keyword">b </span> = (<span class="hljs-built_in">c2</span> - a * <span class="hljs-built_in">c1</span>) % m
c = <span class="hljs-built_in">c1</span>
<span class="hljs-symbol">for</span> i in range(<span class="hljs-number">2</span> ** <span class="hljs-number">16</span>):
    c = (c - <span class="hljs-keyword">b) </span>* a_ % m
    flag = (long_to_bytes(c))
    <span class="hljs-meta">if</span> <span class="hljs-keyword">b'NSSCTF{'in </span>flag:
       print(flag)
       <span class="hljs-keyword">break</span>
</code></pre><blockquote>
<blockquote>
<blockquote>
<p><br>求b：
<br>&emsp;&emsp;b ≡ (x<sub>n+1</sub> - a <em> x<sub>n</sub>) mod(m)
```python
from Crypto.Util.number import </em></p>
</blockquote>
</blockquote>
</blockquote>
<p>flag = b&#39;NSSCTF{<strong>**</strong>}&#39;
m = 96343920769213509183566159649645883498232615147408833719260458991750774595569</p>
<p>c1 = 10252710164251491500439276567353270040858009893278574805365710282130751735178</p>
<p>c2 = 45921408119394697679791444870712342819994277665465694974769614615154688489325</p>
<p>c3 = 27580830484789044454303424960338587428190874764114011948712258959481449527087</p>
<p>class LCG:
     def <strong>init</strong>(self, seed, a, b, m):
         self.seed = seed
         self.a = a 
         self.b = b 
         self.m = m</p>
<pre><code> <span class="hljs-function"><span class="hljs-keyword">def</span> <span class="hljs-title">generate</span><span class="hljs-params">(<span class="hljs-keyword">self</span>)</span></span>:
    <span class="hljs-keyword">self</span>.seed = (<span class="hljs-keyword">self</span>.a * <span class="hljs-keyword">self</span>.seed + <span class="hljs-keyword">self</span>.b) % <span class="hljs-keyword">self</span>.m
    <span class="hljs-keyword">return</span> <span class="hljs-keyword">self</span>.seed
</code></pre><p>lcg = LCG(bytes_to<em>long(flag), getPrime(256), getPrime(256), getPrime(256))
a = ((c3- c2) <em> (pow(c2 - c1,-1,m))) % m
b = (c2 - a </em> c1) % m
c =c1
a</em> = pow(a,-1,m)
for i in range(2 <em>* 16):
    c = (c -b) </em> a_ % m
    flag = (long_to_bytes(c))
    if b&#39;NSSCTF{&#39; in flag:
        print(flag)
        break</p>
<pre><code>&gt;&gt;&gt;&lt;<span class="hljs-keyword">br&gt;求m：
</span>&lt;<span class="hljs-keyword">br&gt;&amp;emsp;&amp;emsp;t&lt;sub&gt;n&lt;/sub&gt; </span>= x&lt;<span class="hljs-keyword">sub&gt;n+1&lt;/sub&gt; </span>- x&lt;<span class="hljs-keyword">sub&gt;n&lt;/sub&gt;
</span>&lt;<span class="hljs-keyword">br&gt;&amp;emsp;&amp;emsp;=[(a*x&lt;sub&gt;n&lt;/sub&gt;+b)-(a*x&lt;sub&gt;n-1&lt;/sub&gt;+b)+b)]mod(m)
</span>&lt;<span class="hljs-keyword">br&gt;&amp;emsp;&amp;emsp;=a*t&lt;sub&gt;n-1&lt;/sub&gt;+b)+b)mod(m)
</span>&lt;<span class="hljs-keyword">br&gt;&amp;emsp;&amp;emsp;t&lt;sub&gt;n+1&lt;/sub&gt;*t&lt;sub&gt;n-1&lt;/sub&gt;=a*t&lt;sub&gt;n&lt;/sub&gt;*t&lt;sub&gt;n-1&lt;/sub&gt;=t&lt;sub&gt;n&lt;/sub&gt;^2
</span>&lt;<span class="hljs-keyword">br&gt;&amp;emsp;&amp;emsp;t&lt;sub&gt;n+1&lt;/sub&gt;与t&lt;sub&gt;n-1&lt;/sub&gt;的最大公约数为m
</span>```python
<span class="hljs-symbol">from</span> Crypto.Util.number <span class="hljs-meta">import</span> *
<span class="hljs-symbol">import</span> gmpy2
<span class="hljs-symbol">flag</span> = <span class="hljs-keyword">b'NSSCTF{******}'
</span><span class="hljs-symbol">c1</span> = <span class="hljs-number">47513456973995038401745402734715062697203139056061145149400619356555247755807</span>

<span class="hljs-symbol">c2</span> = <span class="hljs-number">57250853157569177664354712595458385047274531304709190064872568447414717938749</span>

<span class="hljs-symbol">c3</span> = <span class="hljs-number">30083421760501477670128918578491346192479634327952674530130693136467154794135</span>

<span class="hljs-symbol">c4</span> = <span class="hljs-number">38739029019071698539301566649413274114468266283936163804522278316663267625091</span>

<span class="hljs-symbol">c5</span> = <span class="hljs-number">42506270962409723585330663340839465445484970240895653869393419413017237427900</span>

<span class="hljs-symbol">t1</span> = <span class="hljs-built_in">c2</span> - <span class="hljs-built_in">c1</span>
<span class="hljs-symbol">t2</span> = <span class="hljs-built_in">c3</span> - <span class="hljs-built_in">c2</span>
<span class="hljs-symbol">t3</span> = <span class="hljs-built_in">c4</span> - <span class="hljs-built_in">c3</span>
<span class="hljs-symbol">t4</span> = <span class="hljs-built_in">c5</span> - <span class="hljs-built_in">c4</span>
<span class="hljs-symbol">class</span> LCG:
    def __init__(<span class="hljs-keyword">self, </span>seed, a, <span class="hljs-keyword">b, </span>m):
     <span class="hljs-keyword">self.seed </span>= seed 
     <span class="hljs-keyword">self.a </span>= a 
     <span class="hljs-keyword">self.b </span>= <span class="hljs-keyword">b </span>
     <span class="hljs-keyword">self.m </span>= m 

<span class="hljs-symbol">def</span> generate(<span class="hljs-keyword">self):
</span> <span class="hljs-keyword">self.seed </span>= (<span class="hljs-keyword">self.a </span>* <span class="hljs-keyword">self.seed </span>+ <span class="hljs-keyword">self.b) </span>% <span class="hljs-keyword">self.m
</span> return <span class="hljs-keyword">self.seed
</span>
<span class="hljs-symbol">lcg</span> = LCG(<span class="hljs-keyword">bytes_to_long(flag), </span>getPrime(<span class="hljs-number">256</span>), getPrime(<span class="hljs-number">256</span>), getPrime(<span class="hljs-number">256</span>))
m = <span class="hljs-number">0</span>
<span class="hljs-symbol">m1</span> = gmpy2.gcd(t3 * t1 - t2 ** <span class="hljs-number">2</span>,m)
<span class="hljs-symbol">m2</span> = gmpy2.gcd(t4 * t2 - t3 ** <span class="hljs-number">2</span>,m1)
g = gmpy2.gcd(<span class="hljs-built_in">c2</span> - <span class="hljs-built_in">c1</span>,m2)
m = m2 // g
a = (<span class="hljs-built_in">c3</span> - <span class="hljs-built_in">c2</span>) * (pow(<span class="hljs-built_in">c2</span> - <span class="hljs-built_in">c1</span>,-<span class="hljs-number">1</span>,m)) % m
<span class="hljs-keyword">b </span>= (<span class="hljs-built_in">c2</span> - a * <span class="hljs-built_in">c1</span>) % m

<span class="hljs-symbol">a_</span> = pow(a,-<span class="hljs-number">1</span>,m)
c = <span class="hljs-built_in">c1</span>
<span class="hljs-symbol">for</span> i in range(<span class="hljs-number">2</span> ** <span class="hljs-number">16</span>):
  c = (c - <span class="hljs-keyword">b) </span>* a_ % m
  flag = (long_to_bytes(c))
  <span class="hljs-meta">if</span> <span class="hljs-keyword">b'NSSCTF{' </span>in flag:
    print(flag)
    <span class="hljs-keyword">break</span>
</code></pre><p><strong>8.ecc.ECDSA</strong></p>
<blockquote>
<p><br>椭圆曲线方程：y^2=x^3+ax+b
<br>密钥生成</p>
<blockquote>
<p><br>基点 G：曲线上的一个公开点 (G <sum>x</sum>,G<sum>y</sum>)
<br>私钥：一个随机整数 d（通常为 256 位大整数）
<br>公钥：Q=d×G（点 G 进行 d 次倍点运算的结果）
<br>数学表示
<br>私钥：d（标量）
<br>公钥：Q=dG（曲线上的点）
<br>离散对数问题：已知 Q 和 G，求 d 在计算上是困难的
<br>有限域上的椭圆曲线
<br>实际应用中在有限域 GF(p)上定义：y^2≡x^3+ax+b(modp)</p>
<blockquote>
<p><br><em>数字签名法</em>
<br>私钥d，必须小于圆锥曲线的阶数（n），d∈[1，n-1]</p>
<blockquote>
<p><br>1.哈希消息（SHA-256）→哈希值H(m)
<br>2.生成签名
<br>选择随机数k，即位阶数
<br>计算r ；  r = (k <em> G<sum>x</sum>)modn   G为基点
<br>计算s ：  s = k^(-1)(H(m)+ r </em> d)mod m</p>
<p><br><em>解密过程</em></p>
<blockquote>
<p><br>椭圆曲线Ep(a,b)（p为模数），基点（生成元）G(x,y)，G点的阶数n，私钥k，公钥K(x,y)，随机整数r，明文为一点m(x,y)，密文为两点c1(x,y)c2(x,y)（其中基点G，明文m，公钥K，密文c1、c2都是椭圆曲线E上的点）</p>
<p><br>选择私钥k（k<n）
<br>得到公钥K = k<em>G
<br>选择随机整数r（r<n）
<br>加密：
<br>c1 = m+ r </em> K
<br>c2 = r <em> G
<br>解密：
<br>m = c1-k </em> c2= (c1-r * K)
```python
from Crypto.Util.number import long_to_bytes</p>
</blockquote>
</blockquote>
</blockquote>
</blockquote>
</blockquote>
<p>q = 1139075593950729137191297
a = 930515656721155210883162
b = 631258792856205568553568</p>
<p>G = (641322496020493855620384, 437819621961768591577606)
K = (781988559490437792081406, 76709224526706154630278)
C_1 = (55568609433135042994738, 626496338010773913984218)
C_2 = (508425841918584868754821, 816040882076938893064041)</p>
<p>def inverse(x, q):
    return pow(x, q-2, q)</p>
<p>def add(P, Q, q, a):
    if P is None:
        return Q
    if Q is None:
        return P
    x1, y1 = P
    x2, y2 = Q
    if x1 != x2 and y1 != y2:
        t = ((y2 - y1) <em> inverse(x2 - x1, q)) % q
    else:
        t = ((3 </em> x1 <em> x1 + a) </em> inverse(2 <em> y1, q)) % q
    x3 = t</em>t - x1 - x2
    y3 = t*(x1 - x3) - y1
    return (x3 % q, y3 % q)</p>
<p>def negate(P, q):
    if P is None:
        return None
    x, y = P
    return (x, (-y) % q)</p>
<p>def mul(t, P, q, a):
    if t == 0:
        return None
    t_bin = bin(t)[2:]
    R = None
    for bit in t_bin:
        R = add(R, R, q, a)
        if bit == &#39;1&#39;:
            R = add(R, P, q, a)
    return R</p>
<h1 id="find-r-by-brute-force">Find r by brute force</h1>
<p>print(&quot;Searching for r...&quot;)
r_found = None
for r in range(0, 65536):
    S = mul(r, G, q, a)
    if S == C_2:
        r_found = r
        print(f&quot;Found r = {r}&quot;)
        break</p>
<p>if r_found is None:
    print(&quot;r not found&quot;)
    exit(1)</p>
<h1 id="calculate-m-c_1-r-k">Calculate M = C_1 - r * K</h1>
<p>T = mul(r_found, K, q, a)
minus_T = negate(T, q)
M = add(C_1, minus_T, q, a)
print(f&quot;M = {M}&quot;)</p>
<p>m1, m2 = M
bytes1 = long_to_bytes(m1)
bytes2 = long_to_bytes(m2)
msg = bytes1 + bytes2
print(f&quot;Decrypted msg: {msg}&quot;)</p>
<p>flag = b&#39;flag{&#39; + msg + b&#39;}&#39;
print(flag)
```</p>
