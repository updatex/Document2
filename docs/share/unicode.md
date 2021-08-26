Xin, Gen 2018-10-09
### 常见字符编码

Country/District | Encode | Start Year
--- | --- | ---
AU | ASCII | 1967
EU | ISO 8859-1 | 1998
RUS | KOI-8 | 
CN | GB 18030 or GB 2312 or BIG-5 |  


### unicode
* 解决字符编码方案局限
* 初始版本定义2个字节
* 开始推广发展并不好，比如英文字符浪费空间等
* 网络中，UTF (UCS Transfer Format) 出现
    * UTF-8 使用1-4个字节编码，ASCII用1个字节

unicode编码范围 |UTF-8
---|----|---
0000 0000-0000 007F | 0xxxxxxx
0000 0080-0000 07FF | 110xxxxx 10xxxxxx
0000 0800-0000 FFFF | 1110xxxx 10xxxxxx 10xxxxxx
0001 0000-0010 FFFF | 11110xxx 10xxxxxx 10xxxxxx 10xxxxxx

* 分成17个代码级别（平面）
    * 第一个是基本多语言级别， 范围 [U+0000, U+FFFF]
    * 其余16个代码级别, 称为辅助字符，范围 [U+010000, U+10FFFF]

### JAVA
1. UTF-16
    * 代码单元 16bit
    * 基本多语言级别中，2048个坑没有被占用 [U+D800, U+DFFF]
    * 如果字符属于基本语言级别，直接2个字节。
    * 如果字符属于辅助字符，则第一个字节 范围[U+D800 ,U+DBFF]，第二个字节 范围[U+DC00, U+DFFF]
    * 如此可以轻易的判断字符是否是基本语言级别，判断是否是辅助字符，判断是否是第一个或者第二个字符

2. char 数据类型
    * 采用utf-16编码表示的unicode码点的代码单元
    * 大多数用一个代码单元(码元），辅助需要一对代码单元
```
String greeting = "Hello";

// length 代码单元 数量
int n = greeting.length();
// 返回码点数量，实际数量，大部分case应该没有影响，但是特殊字符的时候，特别需要注意
int cpcount = greeting.codePointCount(0, greeting.length);

// 返回某个代码单元
char first = greeting.charAt(0);
// 返回某个码点
int index = greeting.offsetByCodePoints(0,i)
int cp = greeting.codePointAt(index)


```
3. 区分码点和代码单元
    * 码点 对应 unicode 一个字符
    * 代码单元，即实际存储码点的基本单元，可能2个代码单元表示一个码点

### HADOOP Text type
* using standard utf-8 encoding
* to serialize, deseriaze and compare texts at byte level.
* string traversal without converting byte array to string
* Text is a replacement for the UTF8 class, which was deprecated because it didn’t support strings whose encoding was over 32,767 bytes, and because it used Java’s modified UTF-8

Function| Text | String
--- | --- | ---
charAt | int类型，unicode scalar value |  char,返回代码单元
getLength| 返回bytes数量 | 返回代码单元数量

## 汉字编码演化过程

### GB2312
* 二个字节表示，高位字节 [0xA1,0xF7]，低位字节[0xA1, 0xFE], 组合数大约7000+的样子
* GB2312包括ASCII码，这里叫全角，如果用ASCII则是半角


### GBK
* 后期发现编码不够用了，突破了低位字节必须大约127的限制，规定第一个字节大于127，则是汉字
* 增加了 2万多个字符

### GB18030
* 增加了少数民族字符 几千个

### BIG-5 
* 湾湾常用

### 为什么中国人用GBK比较多
* 因为utf-8 编码中文体积较大，GBK 2个字节就可以表示一个汉字

### 记事本
* 联通乱码
* UTF-8编码保存，会插入3个不可见字符(0xEF BB BF，即bom)


### Teradata UNICODE
https://community.teradata.com/t5/Database/UTF-8-or-UTF-16/m-p/61680

```
Hi Nishant,

UNICODE in Teradata is always stored as UTF-16, i.e. two bytes per character.

In 13.10 there's COMPRESS USING TransUnicodeToUTF8 DECOMPRESS USING TransUTF8ToUnicode to change internal storage to UTF-8 which mightreduce storage when most chars are latin.
```

### hive spark 
* default use utf-8 to store unicode


### Reference
1. [网页编码就是那点事](https://link.zhihu.com/?target=http%3A//www.qianxingzhem.com/post-1499.html)
2. JAVA核心技术 1
3. https://www.cnblogs.com/huiAlex/p/8182586.html
4. https://community.teradata.com/t5/Database/UTF-8-or-UTF-16/m-p/61680
