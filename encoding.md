#### Little endian和Big endian
如严Unicode码是U+4E25，需要用两个字节存储,一个字节是4E，另一个字节是25，存储的时候
4E在前，25在后，这就是Big endian方式.25在前，4E灾后，这就是Little endian方式.

#### BOM
BOM(Byte order mark): 
[wikipedia](https://en.wikipedia.org/wiki/Byte_order_mark)

wikipedia中的UTF8的BOM: EF BB BF是用来标识文件是UTF8的，FE FF和FF FE分别表示UTF16的大小端存储，可是微软在UTF8中加入了U+FEFF的BOM来用于编码的区分(UTF8与ASCII等编码).

带BOM的UTF-8(主要是微软的习惯)： 
微软在UTF8编码的文件开头添加U+FEFF标记，用于将UTF-8和
ASCII等编码明确区分开来.

#### ASCII
> ![ascii表](./ASCII.gif)

定义: 
定义了128个字符的编码规则，范围为0x00-0x7F这些字符组成的集合就叫做ASCII字符集.

控制字符: 
0-31号和127号(DEL)一共33个控制字符.

#### ISO-8859-1(Latin-1)
> [ISO-8859-1](https://cs.stanford.edu/people/miles/iso8859.html)

定义: 
单字节编码，向下兼容ASCII，编码范围是0x00-0xFF，0x00-0x7F之间
完全与ASCII一致，0x80-0x9F之间是控制字符，0xA0-0xFF之间是文字符号.

特性: 
因为ISO-8859-1编码范围使用了单字节内的所有空间，在支持ISO-8859-1的系统中传输和存储其他任何编码的字节流都不会被抛弃.换言之，把其他任何编码的字节流当作ISO-8859-1编码看待都没有问题.
(MySQL数据库默认编码是Latin1、框架的默认编码就是利用了这个特性).

#### unicode

> [unicode](https://www.compart.com/en/unicode)

code point: unicode的码点，用于表示抽象字符的数值.在unicode中，码点以"U+1234"的形式表示,
如字符"A"被分配的码位是U+0041([字符A](https://www.compart.com/en/unicode/U+0041)).
code unit: 使用某种unicode编码方式(utf8、utf16、gbk)里编码一个code point需要的最少字节数.

定义:
万国码，这是一种所有符号的编码.范围0x000000-0x10FFFF.将所有的对应的十六进制分成17个平面，
最常用的放在0x0000-0xFFFF，占用两个字节，叫做基本平面(BMP即Basic Multilingual Plane).

BMP的码点范围是U+0000-U+FFFF，但是其中U+D800-U+DFFF之间（包含边界）的代码点没有定义字符，
将U+D800-U+DFFF拆成两部分：0xD800-0xDBFF（高代理项），0xDC00-0xDFFF（低代理项.这两个各有1024个数，
所以有1024x1024=1048576种组合结果，而SP(U+10000~U+10FFFF)中的代码点个数正好是0x10FFFF-0x10000+1=1048576个


#### GB2312和GBK 
定义: 在GB2312加入更多的汉字，它的编码是和GB2312兼容，也就是说用GB2312编码的汉字可以用GBK来解码，
并且不会有乱码.

windows cmd使用chcp可以查看控制台编码,936为gbk，65001为utf8.

#### utf8(unicode transformation format)

定义: 变长字符编码，是互联网上使用最广的一种Unicode的实现方式，被定义为将码点编码为1-4个字节.

unicode编码和utf-8编码的转换: 
Unicode符号范围        |        UTF-8编码方式
(十六进制)             |              （二进制）
----------------------+---------------------------------------------
0000 0000-0000 007F   | 0xxxxxxx
0000 0080-0000 07FF   | 110xxxxx 10xxxxxx
0000 0800-0000 FFFF   | 1110xxxx 10xxxxxx 10xxxxxx
0001 0000-0010 FFFF   | 11110xxx 10xxxxxx 10xxxxxx 10xxxxxx

如严的unicode编码是U+4E25(二进制为100111000100101)，则UTF-8编码为
11100100 10111000 10100101


#### utf8mb4(most bytes 4)
只是mysql特有的概念，在MYSQL里实现的utf8最长使用3个字节，也就是支持BPM，不支持一些emoji和不常用的汉字，需要四个字节才能编码出来.


#### UTF-16
定义: UTF-16将字符编码成2个或4个字节.

具体编码规则如下: 
对于 Unicode 码小于 0x10000 的字符， 使用 2 个字节存储，并且是直接存储 Unicode 码，不用进行编码转换

对于 Unicode 码在 0x10000 和 0x10FFFF 之间的字符，使用 4 个字节存储，这 4 个字节分成前后两部分，每个部分各两个字节，其中，前面两个字节的前 6 位二进制固定为 110110，后面两个字节的前 6 位二进制固定为 110111, 前后部分各剩余 10 位二进制表示符号的 Unicode 码 减去 0x10000 的结果

大于 0x10FFFF 的 Unicode 码无法用 UTF-16 编码



UTF-16，没有指定后缀，即不知道其是大小端，所以其开始的两个字节表示该字节数组是大端还是小端.即
FE FF表示大端，FF FE表示小端.

UTF-16BE，其后缀是 BE 即 big-endian，大端的意思.大端就是将高位的字节放在低地址表示.

UTF-16LE，其后缀是 LE 即 little-endian，小端的意思.小端就是将高位的字节放在高地址表示.


ps:  java中char类型占用两个字节，理论上最多能表示65535个字符(BMP范围的字符).字符串默认编码是utf-16.如果是BMP外的字符，则需要通过代理顶对的方式，使用两个
连续的char类型单元来表示.

