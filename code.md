计算机存储都是01二进制，这里的编码主要是为了展示，方便软件或者用户查看相应的数据，将0-1二进制文件变得更可读或者安全等。

### acsii编码

总共128个字符，由7位二进制表示。如下分为0-31和127，共33个`控制字符`和32-126，共95个`可显示字符`。
![image](https://wiki.huawei.com/vision-file-storage/api/file/download/upload-v2/2023/4/31/20230531T1123Z/13ebbc4a03da4d0fba282a387a576397/image.png)

### ISO/IEC 8859-1 编码

在ACSII编码基础上，在0xA0-0xFF的范围内增加96个字母和符号，提供给拉丁字母语言使用，参考如下
![image](https://wiki.huawei.com/vision-file-storage/api/file/download/upload-v2/2023/4/31/20230531T1150Z/7b5737a000794cb8984030109e5845cc/image.png)

### Unicode

Unicode是一种编码的统一标准，目标是覆盖所有的语言编码。acsii编码只能表达128个字符，ISO/IEC 8859-1只能表达256个字符，而像日文、韩文、中文等是有几万的常用字符，所以需要对原有的编码进行扩展，支持不同国家和语言的使用。

#### 编码方式

Unicode是国际组织**统一码联盟**制定的可以容纳世界上所有文字和符号的字符编码方案。Unicode用数字0-0x10FFFF来映射这些字符，最多可以容纳1114112个字符，或者说有1114112个码位（码位就是可以分配给字符的数字）。

#### 实现方式

UTF-8、UTF-16、UTF-32都是将数字转换到程序数据的编码方案。

### Hex 编码

16进制,4位 2^4，使用数字`0-9`和字母`a-f`，不区分大小写，共16个字符查看二进制内容。一个字节是8位，因此一个字节是两个hex字符

```
616263 -> abc
```

### Base64 编码

64进制， 6位 2^6，将字节数据中每6位用字母（`a-zA-Z`）、数字（`0-9`）、`+`、`/`总共64个字符等效表示。因此每3个字节（`3x8`）表示成4个Base64编码（`4x6`）。由于数据中的字节数不一定是3的整数倍，当字节数对3求模后，多一个字节时，表示成2个Base64加两个`=`（填充字符），多两个字符时，表示成3个Base64字符和一个等号。刚好整除，则不需要`=`。

```
hello world -> aGVsbG8gd29ybGQ=
```

![image](https://wiki.huawei.com/vision-file-storage/api/file/download/upload-v2/2023/4/31/20230531T1051Z/386d2752cac14aec8af618a009d87ff8/image.png)

### Urlencode 编码

设计用来给url进行编码的，对于 `a-z`,`A-Z`,`0-9`,` .`, `-`,`_`的字符保持不变，其他字符会编码为 `%xx`(16进制，hex编码)，

```
hello world => hello%20world
```

### Base64URL编码

参考base64编码和urlencode编码会发现，base64编码中的字符`+`、`/`、`=`（填充字符）在urlencode编码中需要转化成`%2B`、`%2F`、`%3D`，这样增加了字符长度，也不具备可读性，而且后续存储在数据库中还需要再进行解码，因此Base64编码的流程

#### 编码

1. 进行base64编码
2. 增加一些转换
   + 去掉末尾的填充字符`=`
   + 将`+`替换成`-`
   + 将`/`替换成`_`

#### 解码

1. 增加一些转换
   + 将`-`替换成`+`
   + 将`_`替换成`/`
   + 增加末尾的填充字符`=``
2. 进行base64解码

### 参考

+ [ASCII](https://zh.wikipedia.org/wiki/ASCII)
+ [# ISO/IEC 8859-1](https://zh.wikipedia.org/wiki/ISO/IEC_8859-1)
+ [Unicode](https://zh.wikipedia.org/wiki/Unicode)
+ [Unicode](https://bbs.huaweicloud.com/blogs/174981)
+ 
