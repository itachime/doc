## URI编码

URI:统一资源标识。典型URL的格式如下所示

	foo://example.com:8042/over/there?name=ferret#nose
	\___/ \______________/ \________/\_________/ \__/
	  |         |              |         |        |
	scheme   authority       path      query   fragment

Url编码的原则就是使用安全的字符（没有特殊用途或者特殊意义的可打印字符）去表示那些不安全的字符。  
RFC3986 文档规定，Url中只允许包含英文字母`（a-zA-Z）`、数字`（0-9）`、`-_.~`4个特殊字符以及所有保留字符。Url编码默认使用的字符集是 US-ASCII,US-ASCII 字符集中没有对应的可打印字符：Url中只允许使用可打印字符。US-ASCII码中的10-7F字节全都表示控制字符，这些字符都不能直接出现在Url中。同时，对于80-FF字节（ISO-8859-1），由于已经超出了US-ACII定义的字节范围，因此也不可以放在Url中。

RFC3986中指定了以下字符为保留字符：! * ' ( ) ; : @ & = + $ , / ? # [ ],因此对于Url而言，只有普通英文字符和数字，特殊字符$-_.+!*'()还有保留字符，才能出现在未经编码的Url之中。其他字符均需要经过编码之后才能出现在Url中。

Url编码通常也被称为百分号编码（percent-encoding），使用 % 百分号加上两位的字符——0123456789ABCDEF——代表一个字节的十六进制形式。例如a在US-ASCII码中对应的字节是0x61，那么Url编码之后得到的就是%61， 对于非ASCII字符，需要使用ASCII字符集的超集进行编码得到相应的字节，然后对每个字节执行百分号编码。对于Unicode字符，RFC文档建议使用utf-8对其进行编码得到相应的字节，然后对每个字节执行百分号编码。如"中文"使用UTF-8字符集得到的字节为0xE4 0xB8 0xAD 0xE6 0x96 0x87，经过Url编码之后得到"%E4%B8%AD%E6%96%87"。如果某个字节对应着ASCII字符集中的某个非保留字符，则此字节无需使用百分号表示。例如"Url编码"，使用UTF-8编码得到的字节是0x55 0x72 0x6C 0xE7 0xBC 0x96 0xE7 0xA0 0x81，由于前三个字节对应着ASCII中的非保留字符"Url"，因此这三个字节可以用非保留字符"Url"表示。最终的Url编码可以简化成"Url%E7%BC%96%E7%A0%81" ，

Javascript中提供了3对函数用来对Url编码以得到合法的Url，它们分别是escape / unescape, encodeURI / decodeURI和encodeURIComponent / decodeURIComponent

### 安全字符不同：

escape（69个）：*/@+-._0-9a-zA-Z
encodeURI（82个）：!#$&'()*+,/:;=?@-._~0-9a-zA-Z
encodeURIComponent（71个）：!'()*-._~0-9a-zA-Z

对Unicode字符的编码方式不同：
这三个函数对于ASCII字符的编码方式相同，均是使用百分号+两位十六进制字符来表示。但是对于Unicode字符，escape的编码方式是%uxxxx，其中的xxxx是用来表示unicode字符的4位十六进制字符。这种方式已经被W3C废弃了。但是在ECMA-262标准中仍然保留着escape的这种编码语法。encodeURI和encodeURIComponent则使用UTF-8对非ASCII字符进行编码，然后再进行百分号编码。这是RFC推荐的。因此建议尽可能的使用这两个函数替代escape进行编码。

适用场合不同：
encodeURI被用作对一个完整的URI进行编码，而encodeURIComponent被用作对URI的一个组件进行编码。从上面提到的安全字符范围表格来看，encodeURIComponent编码的字符范围要比encodeURI的大。我们上面提到过，保留字符一般是用来分隔URI组件，这些字符在URI中本来就有特殊用途，因此这些保留字符不会被encodeURI编码，否则意义就变了。组件内部有自己的数据表示格式，但是这些数据内部不能包含有分隔组件的保留字符，否则就会导致整个URI中组件的分隔混乱。因此对于单个组件使用encodeURIComponent，需要编码的字符就更多了。

表单提交中的URI编码
当Html的表单被提交时，每个表单域都会被Url编码之后才在被发送。由于历史的原因，表单使用的Url编码实现并不符合最新的标准,对于非ASCII字符，使用的编码字符集取决于当前文档使用的字符集。一般在head指定