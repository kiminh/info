"base64" --- Base16, Base32, Base64, Base85 数据编码
****************************************************

**源代码：** Lib/base64.py

======================================================================

此模块提供了将二进制数据编码为可打印的 ASCII 字符以及将这些编码解码回
二进制数据的函数。它为 **RFC 3548** 指定的 Base16, Base32 和 Base64 编
码以及已被广泛接受的 Ascii85 和 Base85 编码提供了编码和解码函数。

**RFC 3548** 编码的目的是使得二进制数据可以作为电子邮件的内容正确地发
送，用作 URL 的一部分，或者作为 HTTP POST 请求的一部分。其中的编码算法
和 **uuencode** 程序是不同的。

此模块提供了两个接口。新的接口提供了从 *类字节对象* 到 ASCII 字节
"bytes" 的编码，以及将 ASCII 的 *类字节对象* 或字符串解码到 "bytes" 的
操作。此模块支持定义在 **RFC 3548** 中的所有 base-64 字母表 （普通的、
URL 安全的和文件系统安全的）。

旧的接口不提供从字符串的解码操作，但提供了操作 *文件对象* 的编码和解码
函数。旧接口只支持标准的 Base64 字母表，并且按照 **RFC 2045** 的规范每
76 个字符增加一个换行符。注意：如果你需要支持 **RFC 2045**，那么使用
"email" 模块可能更加合适。

在 3.3 版更改: 新的接口提供的解码函数现在已经支持只包含 ASCII 的
Unicode 字符串。

在 3.4 版更改: 所有 *类字节对象* 现在已经被所有编码和解码函数接受。添
加了对 Ascii85/Base85 的支持。

新的接口提供：

base64.b64encode(s, altchars=None)

   对 *bytes-like object* *s* 进行 Base64 编码，并返回编码后的 "bytes"
   。

   可选项 *altchars* 必须是一个长 2 字节的 *bytes-like object*，它指定
   了用于替换 "+" 和 "/" 的字符。这允许应用程序生成 URL 或文件系统安全
   的 Base64 字符串。默认值是 "None"，使用标准 Base64 字母表。

base64.b64decode(s, altchars=None, validate=False)

   解码 Base64 编码过的 *bytes-like object* 或 ASCII 字符串 *s* 并返回
   解码过的 "bytes"。

   可选项 *altchars* 必须是一个长 2 字节的 *bytes-like object*，它指定
   了用于替换 "+" 和 "/" 的字符。

   如果 *s* 被不正确地填写，一个 "binascii.Error" 错误将被抛出。

   如果 *validate* 值为 "False" （默认情况），则在填充检查前，将丢弃既
   不在标准 base-64 字母表之中也不在备用字母表中的字符。如果
   *validate* 为 "True"，这些非 base64 字符将导致 "binascii.Error"。

base64.standard_b64encode(s)

   编码 *bytes-like object* *s*，使用标准 Base64 字母表并返回编码过的
   "bytes"。

base64.standard_b64decode(s)

   解码 *bytes-like object* 或 ASCII 字符串 *s*，使用标准 Base64 字母
   表并返回编码过的 "bytes"。

base64.urlsafe_b64encode(s)

   编码 *bytes-like object* *s*，使用 URL 与文件系统安全的字母表，使用
   "-" 以及 "_" 代替标准 Base64 字母表中的 "+" 和 "/"。返回编码过的
   "bytes"。结果中可能包含 "="。

base64.urlsafe_b64decode(s)

   解码 *bytes-like object* 或 ASCII 字符串 *s*，使用 URL 与文件系统安
   全的字母表，使用 "-" 以及 "_" 代替标准 Base64 字母表中的 "+" 和 "/"
   。返回解码过的 "bytes"

base64.b32encode(s)

   用 Base32 编码 *bytes-like object* *s* 并返回编码过的 "bytes"

base64.b32decode(s, casefold=False, map01=None)

   解码 Base32 编码过的 *bytes-like object* 或 ASCII 字符串 *s* 并返回
   解码过的 "bytes"。

   可选的 *casefold* 是一个指定小写字幕是否可接受为输入的标志。为了安
   全考虑，默认值为 "False"。

   **RFC 3548** 允许将字母 0(zero) 映射为字母 O(oh)，并可以选择是否将
   字母 1(one) 映射为 I(eye) 或 L(el)。可选参数 *map01* 不是 "None" 时
   ，它的值指定 1 的映射目标 (I 或 l)，当 *map01* 非 "None" 时， 0 总
   是被映射为 O。为了安全考虑，默认值被设为 "None"，如果是这样， 0 和
   1 不允许被作为输入。

   如果 *s* 被错误地填写或输入中存在字母表之外的字符，将抛出
   "binascii.Error"。

base64.b16encode(s)

   用 Base16 编码 *bytes-like object* *s* 并返回编码过的 "bytes"

base64.b16decode(s, casefold=False)

   解码 Base16 编码过的 *bytes-like object* 或 ASCII 字符串 *s* 并返回
   解码过的 "bytes"。

   可选的 *casefold* 是一个指定小写字幕是否可接受为输入的标志。为了安
   全考虑，默认值为 "False"。

   如果 *s* 被错误地填写或输入中存在字母表之外的字符，将抛出
   "binascii.Error"。

base64.a85encode(b, *, foldspaces=False, wrapcol=0, pad=False, adobe=False)

   用 Ascii85 编码 *bytes-like object* *s* 并返回编码过的 "bytes"

   *foldspaces* 是一个可选的标志，使用特殊的短序列 'y' 代替 'btoa' 提
   供的 4 个连续空格 (ASCII 0x20)。这个特性不被 "标准" Ascii85 编码支
   持。

   *wrapcol* 控制了输出是否包含换行符 ("b'\n'"). 如果该值非零, 则每一
   行只有该值所限制的字符长度.

   *pad* 控制在编码之前输入是否填充为4的倍数。请注意，"btoa" 实现总是
   填充。

   *adobe* 控制编码后的字节序列是否要加上 "<~" 和 "~>"，这是 Adobe 实
   现所使用的。

   3.4 新版功能.

base64.a85decode(b, *, foldspaces=False, adobe=False, ignorechars=b' \t\n\r\v')

   解码 Ascii85 编码过的 *bytes-like object* 或 ASCII 字符串 *s* 并返
   回解码过的 "bytes"。

   *foldspaces* 旗标指明是否应接受 'y' 短序列作为 4 个连续空格 (ASCII
   0x20) 的快捷方式。 此特性不被 "标准" Ascii85 编码格式所支持。

   *adobe* 控制输入序列是否为 Adobe Ascii85 格式 (即附加 <~ 和 ~>)。

   *ignorechars* 应当是一个 *bytes-like object* 或 ASCII 字符串，其中
   包含要从输入中忽略的字符。 这应当只包含空白字符，并且默认包含 ASCII
   中所有的空白字符。

   3.4 新版功能.

base64.b85encode(b, pad=False)

   用 base85（如 git 风格的二进制 diff 数据所用格式）编码 *bytes-like
   object* *b* 并返回编码后的 "bytes"。

   如果 *pad* 为真值，输入将以 "b'\0'" 填充以使其编码前长度为 4 字节的
   倍数。

   3.4 新版功能.

base64.b85decode(b)

   解码 base85 编码过的 *bytes-like object* 或 ASCII 字符串 *b* 并返回
   解码过的 "bytes"。 如有必要，填充会被隐式地移除。

   3.4 新版功能.

旧式接口:

base64.decode(input, output)

   解码二进制 *input* 文件的内容并将结果二进制数据写入 *output* 文件。
   *input* 和 *output* 必须为 *文件对象*. *input* 将被读取直至
   "input.readline()" 返回空字节串对象。

base64.decodebytes(s)

   解码 *bytes-like object* *s*，该对象必须包含一行或多行 base64 编码
   的数据，并返回已解码的 "bytes"。

   3.1 新版功能.

base64.decodestring(s)

   已弃用的 "decodebytes()" 的别名。

   3.1 版后已移除.

base64.encode(input, output)

   编码二进制 *input* 文件的内容并将经 base64 编码的数据写入 *output*
   文件。 *input* 和 *output* 必须为 *文件对象*。 *input* 将被读取直到
   "input.read()" 返回空字节串对象。 "encode()" 会在每输出 76 个字节之
   后插入一个换行符 ("b'\n'")，并会确保输出总是以换行符来结束，如
   **RFC 2045** (MIME) 所规定的那样。

base64.encodebytes(s)

   编码 *bytes-like object* *s*，其中可以包含任意二进制数据，并返回包
   含经 base64 编码数据的 "bytes"，每输出 76 个字节之后将带一个换行符
   ("b'\n'")，并会确保在末尾也有一个换行符，如 **RFC 2045** (MIME) 所
   规定的那样。

   3.1 新版功能.

base64.encodestring(s)

   一个被弃用的 "encodebytes()" 的别名。

   3.1 版后已移除.

此模块的一个使用示例:

>>> import base64
>>> encoded = base64.b64encode(b'data to be encoded')
>>> encoded
b'ZGF0YSB0byBiZSBlbmNvZGVk'
>>> data = base64.b64decode(encoded)
>>> data
b'data to be encoded'

参见:

  模块 "binascii"
     支持模块，包含ASCII到二进制和二进制到ASCII转换。

  **RFC 1521** - MIME (Multipurpose Internet Mail Extensions) 第一部分
  ：规定并描述因特网消息体的格式的机制。
     第 5.2 节，“Base64 内容转换编码格式” 提供了 base64 编码格式的定义
     。
