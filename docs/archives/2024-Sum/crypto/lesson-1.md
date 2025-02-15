# 课前准备

预先了解一下CTF中密码方向及古典密码分类

# 基础知识

## CTF密码方向分类

CTF中的密码学主要包括**「编码** **Encoding** **」** , **「古典密码学** **Classic cryptography** **」** 和 **「现代密码学** **Modern cryptography** **」三部分，**虽然不再符合现代 严谨的密码学 的定义，但在广义的密码学上 **「编码** **Encoding** **」** 依旧被承认为密码学分支。

由于密码学的发展 以及 CTF 比赛的不断革新，编码和古典密码已经很少出现在密码学方向的题目中了，即使出现也会跟随现代密码学一同使用，更常见的反而是在「 **杂项** **MISC** 」中作为签到题或者一般简单题目考察。

## 相关工具

1，gmpy2

```
pip install gmpy2
```

2，pycryptodome

```
pip install pycryptodome
```

3.pwntools

```
pip install pwntools
```

4，sagemath

[doc.sagemath.org](https://doc.sagemath.org/html/en/installation/index.html)

ps:([提问的智慧](https://how-to-ask.natro92.fun/))

## 古典密码之移位密码

移位式密码，它们字母本身不变，但它们在消息中顺序是依照一个定义明确的项目改变。许多移位式密码是基于[几何](https://zh.wikipedia.org/wiki/幾何)而设计的，可分为一个简单的加密（也易被破解），可以将字母向右移1位。例如，明文"Hello my name is Alice."将变成"olleH ym eman si ecilA."。

### 栅栏密码

最经典的移位密码就是栅栏密码

①把将要传递的信息中的字母交替排成上下两行。

②再将下面一行字母排在上面一行的后边，从而形成一段密码。

③例如：明文：THE LONGEST DAY MUST HAVE AN END加密：

1、把将要传递的信息中的字母交替排成上下两行。T E O G S D Y U T A E N N             H L N E T A M S H V A E D

2、 密文：将下面一行字母排在上面一行的后边。TEOGSDYUTAENN   HLNETAMSHVAED

解密：先将密文分为两行T E O G S D Y U T A E N N      H L N E T A M S H V A E D再按上下上下的顺序组合成一句话明文：THE LONGEST DAY MUST HAVE AN END

## 古典密码之替换密码

替换密码是字母（或是字母群）作有系统的代换，直到消息被替换成其它难以解读的字。

替换式密码亦有许多不同类型。如果每一个字母为一单元（或称元素）进行加密操作，就可以称之为 **「简易替换密码** simple substitution cipher」 或 「单表加密 monoalphabetic cipher 」另又称为 **单字母替换加密** ; 以数个字母为一单元则称为 **「多表加密** polyalphabetic cipher」 或 「表格式加密 polygraphic」。

### 单表替换

以Atbash码为例，其密文表是明文表的逆转：

明文表：ABCDEFGHIJKLMNOPQRSTUVWXYZ 

密文表：ZYXWVUTSRQPONMLKJIHGFEDCBA

明文：the quick brown fox jumps over the lazy dog

密文：gsv jfrxp yildm ulc qfnkh levi gsv ozab wlt

#### 凯撒密码

**凯撒密码** **Caesar****[¶](https://hello-ctf.com/HC_Crypto/Classicalcipher/)**

**凯撒密码**（或称恺撒加密、恺撒变换、变换加密、位移加密）是一种 **单表代换** 替换加密，明文中的所有字母都在字母表上向后（或向前）按照一个固定数目进行偏移后被替换成密文。例，当 偏移量 **Key = 3** 的时候，所有的字母 A 将被替换成 D，B 变成 E，以此类推。

![fbe9efd7c749aacdb8fb01178e813032.png](https://ice.frostsky.com/2024/08/03/fbe9efd7c749aacdb8fb01178e813032.png)

### 多表替换

#### 维吉尼亚密码

**维吉尼亚密码** **Vigenère Cipher****[¶](https://hello-ctf.com/HC_Crypto/Classicalcipher/)**

**维吉尼亚密码** 是在单一恺撒密码的基础上扩展出 **多表代换** 替换加密，根据密钥 (当密钥长度小于明文长度时可以循环使用) 来决定用哪一行的密表来进行替换，以此来对抗字频统计。![d0a75f79081100c81faec20b82a3c7e7.png](https://ice.frostsky.com/2024/08/03/d0a75f79081100c81faec20b82a3c7e7.png)

在一个凯撒密码中，字母表中的每一字母都会作一定的偏移，例如偏移量为 3 时，A 就转换为了 D、B 转换为了 E……而维吉尼亚密码则是由一些偏移量不同的凯撒密码组成。

为了生成密码，需要使用表格法。这一表格包括了 26 行字母表，每一行都由前一行向左偏移一位得到。具体使用哪一行字母表进行编译是基于密钥进行的，在过程中会不断地变换。

举个例子，假设明文为：ATTACKATDAWN

选择某一关键词并重复而得到密钥，如关键词为 LEMON 时，密钥为：

LEMONLEMONLE

对于明文的第一个字母 A，对应密钥的第一个字母 L，于是使用表格中 L 行字母表进行加密，得到密文第一个字母 L。类似地，明文第二个字母为 T，在表格中使用对应的 E 行进行加密，得到密文第二个字母 X。以此类推，可以得到：

明文：ATTACKATDAWN

密钥：LEMONLEMONLE

密文：LXFOPVEFRNHR

## 古典密码之编码

编码不同于密码，他的关键不是隐藏信息，而是处理信息，是信息的另一种表示形式，用于解决一些特殊字符、不可见字符的传输问题。

将原始信息变成编码信息的过程为 **「编码** **Encoding** **」**，反之，将编码信息转化回原始信息，转化的过程称之为 **「解码** **Dncoding** **」**。

编码很少单独作为题目出现在 CTF 中，除开 T0 级别的签到题目，多数情况都贯穿 CTF 的各个方向，包括但不限于 杂项 和 密码学。

### 工具

Cyberchef

[CyberChef](https://github.com/gchq/CyberChef)[ ](https://github.com/gchq/CyberChef)是 CTF 中最常用的编码解码工具之一，内含多种编码和解码方式。

![bb3f13bd3d61f8ef1f07f144eccdd225.png](https://ice.frostsky.com/2024/08/03/bb3f13bd3d61f8ef1f07f144eccdd225.png)

### 常见编码

#### **ASCII** **编码**

一种使用指定的 7 位或 8 位二进制数组合来表示 128 或 256 种可能的字符的编码方式。

![5fc69be40aaa54c73c73ea2613a730bc.png](https://ice.frostsky.com/2024/08/04/5fc69be40aaa54c73c73ea2613a730bc.png)

#### **Hex**

「Hex Hexadecimal 」 意为 "十六进制"，虽然十六进制经常用于与编码相关的上下文中，但它并不是一种编码，只是一种数值表示法。

#### **Base** **家族**

Base 家族的编码方式提供了一种方法，将原始的二进制数据转换成一个更“友好”的、由特定字符集组成的字符串格式，比如 base64、base32、base16 可以分别编码转化 8 位字节为 6 位、5 位、4 位。16,32,64 分别表示用多少个字符来编码

### **Base** **家族**

以 Base64 为例：

编码过程：

1.将输入数据分割成长度为 3 的字节组。

2.将这三个字节转换为 4 个 6 位的组。这样做是通过将这三个字节（总共 24 位）重组为 4 个 6 位的数值。

3.使用这 4 个 6 位的数值作为索引，从 BASE64 字符集中选择对应的字符。

4.如果输入数据的字节长度不是 3 的倍数，就在输出的 BASE64 字符串后面加上一个或两个 '=' 字符作为填充。

源文本：Go

G -> 01000111

o -> 01101111

得到8位binary：01000111 01101111

分组为6位binary：010001 110110 1111

补0：010001 110110 111100

对应十进制：17 54 60 (为数组下标)

ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/

-----------------\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-↑------------------------------------\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-↑-----\-\-\-\-↑---

010001 -> R

110110 -> 2

111100 -> 8

查表得：R28

补齐得：R28=

![e1ab086aaaf264f104a916ceab95d20b.png](https://ice.frostsky.com/2024/08/04/e1ab086aaaf264f104a916ceab95d20b.png)

### URL 编码

**URL 编码**,也被称为百分号编码，是一种编码机制，用于将不安全或特殊的字符转换为`%`后跟其 ASCII 的十六进制表示，以确保 URL 的安全传输。

通常的 url 编码只会处理符号和不可见字符，比如 Squdgy fez, blank jimp crwth vox

会被编码为 Squdgy%20fez%2C%20blank%20jimp%20crwth%20vox (普通类型)

但在 CTF 中我们可能会将其编码为

%53%71%75%64%67%79%20%66%65%7a%2c%20%62%6c%61%6e%6b%20%6a%69%6d%70%20%63%72%77%74%68%20%76%6f%78 (复杂类型)

### Unicode 编码

默认模式

\u0054\u0065\u0073\u0074\u0040\u0031\u0032\u0033

宽字符

\u{54}\u{65}\u{73}\u{74}\u{40}\u{31}\u{32}\u{33}

编码模式

U+0054U+0065U+0073U+0074U+0040U+0031U+0032U+0033

CSS实体 十六进制

\0054\0065\0073\0074\0040\0031\0032\0033

### **莫尔斯电码**

「 **摩尔斯电码** **Morse code** 」也被称作摩斯密码，当然虽然有密码一词，但其并不属于加解密算法，而是一种经典的编码方式。

A .-  N -.  . .-.-.- + .-.-.  1 .----

B -... O ---  , --..-- _ ..--.-  2 ..---

C -.-. P .--. : ---... $ ...-..- 3 ...--

D -..  Q --.- " .-..-. & .-...  4 ....-

E .   R .-.  ' .----. / -..-.  5 .....

F ..-. S ...  ! -.-.--       6 -....

G --.  T -   ? ..--..       7 --...

H .... U ..-  @ .--.-.       8 ---..

I ..  V ...- - -....-       9 ----.

J .--- W .--  ; -.-.-.       0 -----

K -.-  X -..- ( -.--.      

L .-.. Y -.-- ) -.--.-     

M --  Z --.. = -...-

## 有趣の古典密码

### 宝可梦图腾

![1e44cc6c6b4ae135154ba651004e9902.png](https://ice.frostsky.com/2024/08/04/1e44cc6c6b4ae135154ba651004e9902.png)

### 精灵语

![f61c35559fdc6ca59768c6457c6b65f2.png](https://ice.frostsky.com/2024/08/04/f61c35559fdc6ca59768c6457c6b65f2.png)

### 盲文

![e146ed5c45247d7b480bef7e05786f18.png](https://ice.frostsky.com/2024/08/03/e146ed5c45247d7b480bef7e05786f18.png)

# 课后作业

课后的三道习题都在http://ctf.miaoaixuan.cn/

平台的Crypto讲课作业提交处，做完三道题目后左上角提交wp(PDF格式)即可(不会的可以上网搜索，但不要照抄wp)，也可以自己找点题来练