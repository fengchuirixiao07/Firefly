---
title: basic crypto
published: 2026-02-17
description: ctf中密码学的入门知识点
image: ./images/834c8cb76c58741f7d5ff9e6664cc448.jpg
tags: [ctf,crypto]
category: crypto
draft: false
---

# 密码学笔记：RSA 与常见攻击手法

## 1. RSA 加密

### 1.1 数学基础

**同余：**
$$a \equiv b \pmod{m}$$

**逆元：**
若存在 $x$ 使得 $a \times x \equiv 1 \pmod{m}$，则称 $x$ 为 $a$ 模 $m$ 的逆元，记作 $x \equiv a^{-1} \pmod{m}$。

### 1.2 RSA 加密原理

- $n$：模数
- $e$：公钥指数
- $c$：密文
- $m$：明文

加密公式：
$$c \equiv m^e \pmod{n}$$

### 1.3 RSA 解密原理

**1. 密钥生成：**

选择两个大质数 $p$ 和 $q$，计算：
$$n = p \times q$$
$$\varphi(n) = (p-1) \times (q-1)$$

加密关系：$m^e \equiv c \pmod{n}$，解密关系：$c^d \equiv m \pmod{n}$

由此可得：$m^{e \times d} \equiv m \pmod{n}$

**2. 欧拉定理：**
$$m^{\varphi(n)} \equiv 1 \pmod{n}$$

**3. 推导私钥 $d$：**

由 $m^{k \cdot \varphi(n) + 1} \equiv m \pmod{n}$ 可得：
$$e \times d = k \cdot \varphi(n) + 1 \Rightarrow e \times d \equiv 1 \pmod{\varphi(n)}$$

因此：
$$d = e^{-1} \pmod{\varphi(n)} \quad \text{or} \quad d = \text{pow}(e, -1, \varphi(n))$$

**4. 最终解密：**
$$m = c^d \pmod{n}$$

---

## 2. 共享素数攻击 (Common Modulus)

**原理：** 如果两台机器的 $n$ 共享一个大素数 $p$（即 $\gcd(n_1, n_2) = p$），则可以分解模数。

**步骤：**

1. 求公因子：$p = \gcd(n_1, n_2)$
2. 分解模数：
   - $q_1 = n_1 // p \Rightarrow \varphi(n_1) = (p-1)(q_1-1)$
   - $q_2 = n_2 // p \Rightarrow \varphi(n_2) = (p-1)(q_2-1)$
3. 求解私钥：
   - $d_1 = e^{-1} \pmod{\varphi(n_1)}$
   - $d_2 = e^{-1} \pmod{\varphi(n_2)}$
4. 解密：$m_1 = c^{d_1} \pmod{n_1}$

---

## 3. 中国剩余定理 (CRT)

用于求解同余方程组：
$$
\begin{cases}
x \equiv a_1 \pmod{m_1} \\
x \equiv a_2 \pmod{m_2} \\
\vdots \\
x \equiv a_k \pmod{m_k}
\end{cases}
$$

**解法：**

1. 计算总模数：$M = m_1 \times m_2 \times \dots \times m_k$
2. 计算分量：$M_i = M / m_i$
3. 求逆元：$x_i = M_i^{-1} \pmod{m_i}$
4. 计算结果：
$$x \equiv \sum_{i=1}^{k} (a_i \cdot M_i \cdot x_i) \pmod{M}$$

**爆破开方 (Python)：**
```python
import gmpy2
from Crypto.Util.number import *

# 计算第 k 次方根
m = gmpy2.iroot(x, k)[0]
```

---

## 4. 小明文攻击 (Small Message Attack)

**适用条件：** 当 $e$ 较小，且明文 $m$ 很小，导致 $m^e < n$ 时。
此时取模运算无效：$c = m^e \pmod{n} \Rightarrow c = m^e$。

**解法：** 直接对 $c$ 开 $e$ 次方。

```python
import gmpy2
from Crypto.Util.number import *

m = gmpy2.iroot(c, e)[0]
```

---

## 5. 费马小定理与临近素数问题

### 5.1 费马小定理

- **形式一：** 若 $p$ 为质数，且 $\gcd(a, p) = 1$，则 $a^{p-1} \equiv 1 \pmod{p}$
- **形式二：** 若 $p$ 为质数，对任意整数 $a$，有 $a^p \equiv a \pmod{p}$

### 5.2 临近素数问题 (Fermat's Factorization)

若 $p$ 和 $q$ 距离很近，可利用平方差公式分解 $N$。

**原理：**

设 $p = a - b$，$q = a + b$。

则 $N = p \times q = (a-b)(a+b) = a^2 - b^2$

即 $b^2 = a^2 - N$

**方法一：平方差遍历法**

```python
from Crypto.Util.number import *
import gmpy2

n = 122719648746679660211272134136414102389555796575857405114496972248651220892565781331814993584484991300852578490929023084395318478514528533234617759712503439058334479192297581245539902950267201362675602085964421659147977335779128546965068649265419736053467523009673037723382969371523663674759921589944204926693
e = 65537
c = 109215817118156917306151535199288935588358410885541150319309172366532983941498151858496142368333375769194040807735053625645757204569614999883828047720427480384683375435683833780686557341909400842874816853528007258975117265789241663068590445878241153205106444357554372566670436865722966668420239234530554168928

a = gmpy2.iroot(n, 2)[0]
while True:
    b_2 = a*a - n
    if gmpy2.is_square(b_2):
        b = gmpy2.iroot(b_2, 2)[0]
        p = a - b
        q = a + b
        if p * q == n:
            break
    a += 1

# assert pow(666666, x, p) == 1
x = p - 1
phi = (p - 1) * (q - 1)
d = pow(e, -1, phi)
m_ = pow(c, d, n)
m = m_ ^ x
print(long_to_bytes(m))
```

**方法二：相邻素数法**

```python
from Crypto.Util.number import *
import gmpy2

e = 65537
n = 169522900072954416356051647146585827691225327527086797334523482640452305793443986277933900273961829438217255938808371865341750200444086653241610669340348513884285892043530862971785487294831341653909852543469963032532560079879299447677636753647721541724969084825510405349373420839032990681851700075554428485967
c = 105943762023156641770119141175498496686312095002592803768522760959533958364969985856505466722378959991757667341747887520146437729810252085791886309974903778546814812093444837674447485802109225767800488527376777153844313243366001288246744190001997192598159277512188417272938455513900277907186067996704043274199

t = gmpy2.iroot(n, 2)
p = gmpy2.next_prime(t[0])
x = (p-1)*1024
q = n // p
phi = (p - 1) * (q - 1)
d = inverse(e, phi)
m_ = pow(c, d, n)
m = m_ ^ x
print(long_to_bytes(m))
```

**适用范围差异：**

| 方法 | 适用条件 | 搜索范围 | 成功率 |
|:---|:---|:---|:---|
| 平方差遍历法 | p 和 q 相近 | 从 √n 开始线性搜索 | 较高（只要 p, q 相近） |
| 相邻素数法 | p 和 q 相近 | 只检查一个特定的素数 | 较低（依赖特定假设） |

---

## 6. dp 泄露攻击

**原理：** 若泄露 $dp = d \pmod{(p-1)}$，则可反推 $p$。

由 $e \cdot d \equiv 1 \pmod{\varphi(n)}$ 推导出 $e \cdot dp \equiv 1 \pmod{(p-1)}$

即 $e \cdot dp = 1 + k(p-1)$

变形得：$(e \cdot dp - 1) // k = p - 1$

**攻击脚本：**

```python
from Crypto.Util.number import *
import gmpy2

n = 110231451148882079381796143358970452100202953702391108796134950841737642949460527878714265898036116331356438846901198470479054762675790266666921561175879745335346704648242558094026330525194100460497557690574823790674495407503937159099381516207615786485815588440939371996099127648410831094531405905724333332751
dp = 3086447084488829312768217706085402222803155373133262724515307236287352098952292947424429554074367555883852997440538764377662477589192987750154075762783925
c = 59325046548488308883386075244531371583402390744927996480498220618691766045737849650329706821216622090853171635701444247741920578127703036446381752396125610456124290112692914728856924559989383692987222821742728733347723840032917282464481629726528696226995176072605314263644914703785378425284460609365608120126
e = 65537

for k in range(1, e):
    if (dp * e - 1) % k == 0:
        p = (dp * e - 1) // k + 1
        if n % p == 0:
            q = n // p
            phi_n = (p - 1) * (q - 1)
            d = pow(e, -1, phi_n)
            m = pow(c, d, n)
            print(long_to_bytes(m))
            break
```

**补充定理：**

若 $n = p \times q$（$p, q$ 都是素数），则 $m = c^d \pmod{p}$ 或 $m = c^d \pmod{q}$。

$\varphi(n) = (p-1)(q-1)$，也可寻找与 $e$ 互质的部分满足逆元使用条件。

---

## 7. 线性同余生成器 (LCG)

**递推公式：** $X_{n+1} = (a \times X_n + b) \pmod{m}$

**参数说明：**
- $X_n$：当前状态（种子）
- $a$：乘数
- $b$：增量
- $m$：模数

### 7.1 求 Seed ($X_0$)

由 $X_{n+1} = (a X_n + b) \pmod{m}$ 反推：
$$X_n \equiv (X_{n+1} - b) \times a^{-1} \pmod{m}$$

```python
from Crypto.Util.number import *
import gmpy2

flag = b'NSSCTF{******}'
a = 113439939100914101419354202285461590291215238896870692949311811932229780896397
b = 72690056717043801599061138120661051737492950240498432137862769084012701248181
m = 72097313349570386649549374079845053721904511050364850556329251464748004927777
c = 9772191239287471628073298955242262680551177666345371468122081567252276480156

class LCG:
    def __init__(self, seed, a, b, m):
        self.seed = seed
        self.a = a
        self.b = b
        self.m = m

    def generate(self):
        self.seed = (self.a * self.seed + self.b) % self.m
        return self.seed

a_inv = pow(a, -1, m)
for _ in range(2**16):
    c = (c - b) * a_inv % m
    flag = long_to_bytes(c)
    if b'NSSCTF{' in flag:
        print(flag)
        break
```

### 7.2 求参数 $a$

利用连续三个输出值：
$$a \equiv (X_{n+1} - X_n) \times (X_n - X_{n-1})^{-1} \pmod{m}$$

```python
from Crypto.Util.number import *
import gmpy2

flag = b'NSSCTF{******}'
a = 83968440254358975953360088805517488739689448515913931281582194839594954862517
m = 77161425490597512806099499399561161959645895427463118872087051902811605680317

c1 = 43959768681328408257423567932475057408934775157371406900460140947365416240650
c2 = 8052043336238864355872102889254781281466728072798160448260752595038552944808

class LCG:
    def __init__(self, seed, a, b, m):
        self.seed = seed
        self.a = a
        self.b = b
        self.m = m

    def generate(self):
        self.seed = (self.a * self.seed + self.b) % self.m
        return self.seed

lcg = LCG(bytes_to_long(flag), getPrime(256), getPrime(256), getPrime(256))
a_inv = pow(a, -1, m)
b = (c2 - a * c1) % m
c = c1
for _ in range(2**16):
    c = (c - b) * a_inv % m
    flag = long_to_bytes(c)
    if b'NSSCTF{' in flag:
        print(flag)
        break
```

### 7.3 求参数 $b$

$$b \equiv (X_{n+1} - a \times X_n) \pmod{m}$$

```python
from Crypto.Util.number import *
import gmpy2

flag = b'NSSCTF{******}'
m = 96343920769213509183566159649645883498232615147408833719260458991750774595569

c1 = 10252710164251491500439276567353270040858009893278574805365710282130751735178
c2 = 45921408119394697679791444870712342819994277665465694974769614615154688489325
c3 = 27580830484789044454303424960338587428190874764114011948712258959481449527087

class LCG:
    def __init__(self, seed, a, b, m):
        self.seed = seed
        self.a = a
        self.b = b
        self.m = m

    def generate(self):
        self.seed = (self.a * self.seed + self.b) % self.m
        return self.seed

lcg = LCG(bytes_to_long(flag), getPrime(256), getPrime(256), getPrime(256))
a = ((c3 - c2) * pow(c2 - c1, -1, m)) % m
b = (c2 - a * c1) % m
a_inv = pow(a, -1, m)
c = c1
for _ in range(2**16):
    c = (c - b) * a_inv % m
    flag = long_to_bytes(c)
    if b'NSSCTF{' in flag:
        print(flag)
        break
```

### 7.4 求模数 $m$ (Marsaglia's GCD Trick)

利用连续四个输出值，构造 $t_i = X_{i+1} - X_i$：
$$m = \gcd(t_3 t_1 - t_2^2, \; t_4 t_2 - t_3^2)$$

```python
from Crypto.Util.number import *
import gmpy2

flag = b'NSSCTF{******}'

c1 = 47513456973995038401745402734715062697203139056061145149400619356555247755807
c2 = 57250853157569177664354712595458385047274531304709190064872568447414717938749
c3 = 30083421760501477670128918578491346192479634327952674530130693136467154794135
c4 = 38739029019071698539301566649413274114468266283936163804522278316663267625091
c5 = 42506270962409723585330663340839465445484970240895653869393419413017237427900

t1 = c2 - c1
t2 = c3 - c2
t3 = c4 - c3
t4 = c5 - c4

m_temp = gmpy2.gcd(t3 * t1 - t2**2, t4 * t2 - t3**2)
g = gmpy2.gcd(c2 - c1, m_temp)
m = m_temp // g

a = ((c3 - c2) * pow(c2 - c1, -1, m)) % m
b = (c2 - a * c1) % m
a_inv = pow(a, -1, m)
c = c1
for _ in range(2**16):
    c = (c - b) * a_inv % m
    flag = long_to_bytes(c)
    if b'NSSCTF{' in flag:
        print(flag)
        break
```

---

## 8. ECC (椭圆曲线密码)

### 8.1 基础概念

**曲线方程：** $y^2 = x^3 + ax + b$

**有限域方程：** $y^2 \equiv x^3 + ax + b \pmod{p}$

**密钥生成：**
- **基点 $G$**：曲线上的公开生成元 $(G_x, G_y)$
- **私钥 $d$**：随机整数，$d \in [1, n-1]$（$n$ 为 $G$ 的阶）
- **公钥 $Q$**：$Q = d \times G$（标量乘法/倍点运算）

**离散对数问题：** 已知 $Q$ 和 $G$，求 $d$ 在计算上是困难的

### 8.2 ECDSA 签名

1. 哈希消息（SHA-256）→ 哈希值 $H(m)$
2. 选择随机数 $k$（即阶数）
3. 计算 $r$：$r = (k \cdot G)_x \pmod{n}$
4. 计算 $s$：$s = k^{-1}(H(m) + r \cdot d) \pmod{n}$

### 8.3 ECC 加密与解密 (ECIES-like)

- **私钥** $k$，**公钥** $K = k \cdot G$
- **加密：** 选随机数 $r$
  - $C_1 = M + r \cdot K$
  - $C_2 = r \cdot G$
- **解密：** $M = C_1 - k \cdot C_2$

**解密脚本示例：**

```python
from Crypto.Util.number import long_to_bytes

q = 1139075593950729137191297
a = 930515656721155210883162
b = 631258792856205568553568

G = (641322496020493855620384, 437819621961768591577606)
K = (781988559490437792081406, 76709224526706154630278)
C_1 = (55568609433135042994738, 626496338010773913984218)
C_2 = (508425841918584868754821, 816040882076938893064041)

def inverse(x, q):
    return pow(x, q-2, q)

def ecc_add(P, Q, q, a):
    if P is None:
        return Q
    if Q is None:
        return P
    x1, y1 = P
    x2, y2 = Q
    if x1 != x2:
        lam = ((y2 - y1) * inverse(x2 - x1, q)) % q
    else:
        lam = ((3 * x1 * x1 + a) * inverse(2 * y1, q)) % q
    x3 = (lam*lam - x1 - x2) % q
    y3 = (lam * (x1 - x3) - y1) % q
    return (x3, y3)

def scalar_mul(k, P, q, a):
    R = None
    for bit in bin(k)[2:]:
        R = ecc_add(R, R, q, a)
        if bit == '1':
            R = ecc_add(R, P, q, a)
    return R

# 爆破 r
print("Searching for r...")
r_found = None
for r in range(65536):
    S = scalar_mul(r, G, q, a)
    if S == C_2:
        r_found = r
        print(f"Found r = {r}")
        break

if r_found is None:
    print("r not found")
    exit(1)

# 计算 M = C_1 - r*K
T = scalar_mul(r_found, K, q, a)
M = ecc_add(C_1, (T[0], -T[1] % q), q, a)
print(f"M = {M}")

m1, m2 = M
msg = long_to_bytes(m1) + long_to_bytes(m2)
print(f"Decrypted msg: {msg}")

flag = b'flag{' + msg + b'}'
print(flag)
```
