==============================
编程中各种语言对应的字符编解码
==============================

概念
----

字符串: 一个字符串是一个字符序列.
       
编解码: 字符的具体表述取决于所用的编码,把字符转换成字节序列的过程是编码;把字节序列转换成字符的过程叫解码.

bytes <--> str 

编码转换: 先解码成原生的字节序列,重新进行编码的过程.

字符串是文本序列, 字节序列是二进制序列.

处理文本文件最佳实战
--------------------

Unicode三明治: 

   意思是,要尽早把输入的字节序列解码成字符串,这种三明治中的"肉片"是程序的业务逻辑,在这里只能处理字符串对象.
   在其它处理过程中,一定不能编码或解码.
   对输出来说,则要尽量把字符串编码成字节序列.

C语言字符编解码
---------------

.. code-block:: c

    // API 
    #include <iconv.h>

    iconv_t iconv_open(const char\* tocode, const char\* fromcode);

    size_t iconv(iconv_t cd, char\*\* inbuf, size_t\* inbytesleft, char\*\* outbuf, size_t\* outbytesleft);

    int iconv_close(iconv_t cd);

    // Work Flow
    int code_convert(char\* from_charset, char\* to_charset, char\* inbuf, int inlen, char\* outbuf, int outlen)
    {
      iconv_t cd;
      int rc;
      char\*\* pin = &inbuf;
      char\*\* pout = &outbuf;

      cd = iconv_open(to_charset, from_charset);
      if(cd==0) return -1;
      memset(outbuf, 0, outlen);
      if(iconv(cd, pin, &inlen, pout, &outlen)==-1) return -1;
      iconv_close(cd);
      return 0;
    }

Python3字符编解码
-----------------

.. code-block:: python 

    str_demo = "caffe"
    buff = str_demo.encode('utf8')
    buff.decode('utf8')