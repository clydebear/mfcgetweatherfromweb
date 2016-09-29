zlib是提供数据压缩的函式库，zlib的主页是:http://www.zlib.net/

下载好zlib，进入zlib-1.2.8\contrib\vstudio找到对应的版本，运行编译获得dll和zlib.lib
编译好 zlib.lib 后, 你就得到了调用一个静态库所需要的所有文件了(zlib.lib, zlib.h, zconf.h). 如何调用静态库不用我说了吧.
关键的函数：
（1）int compress (Bytef *dest,   uLongf *destLen, const Bytef *source, uLong sourceLen);

把源缓冲压缩成目的缓冲, 就那么简单, 一个函数搞定
(2) int compress2 (Bytef *dest,   uLongf *destLen,const Bytef *source, uLong sourceLen,int level);

功能和上一个函数一样,都一个参数可以指定压缩质量和压缩数度之间的关系(0-9)不敢肯定这个参数的话不用太在意它,明白一个道理就好了: 要想得到高的压缩比就要多花时间

(3) uLong compressBound (uLong sourceLen);

计算需要的缓冲区长度. 假设你在压缩之前就想知道你的产度为 sourcelen 的数据压缩后有多大, 可调用这个函数计算一下,这个函数并不能得到精确的结果,但是它可以保证实际输出长度肯定小于它计算出来的长度

(4) int uncompress (Bytef *dest,   uLongf *destLen,const Bytef *source, uLong sourceLen);

解压缩(看名字就知道了:)

(5) deflateInit() + deflate() + deflateEnd()

3个函数结合使用完成压缩功能,具体用法看 example.c 的 test_deflate()函数. 其实 compress() 函数内部就是用这3个函数实现的(工程 zlib 的 compress.c 文件)

(6) inflateInit() + inflate() + inflateEnd()

和(5)类似,完成解压缩功能.

(7) gz开头的函数. 用来操作*.gz的文件,和文件stdio调用方式类似. 想知道怎么用的话看example.c 的 test_gzio() 函数,很easy.

(8) 其他诸如获得版本等函数就不说了.

总结: 在编译的时候，需要在你的工程里，加一个预定义宏：工程属性->C++->预定义宏->预处理器定义(第一个)．
增加ZLIB_WINAPI这个宏．这一点很重要，否则你的工程会编译不通过．

配置好工程后，你就可以使用了．我主要用到了compress和uncompress两个函数．具体的用法各位可以
去网络搜索或者是看API文档，使用很简单．
举个例子：

#include <iostream>
#include <stdlib.h>
#include "zlib.h"

using namespace std;

#define MaxBufferSize 1024*10
void main()
{
     int i;
 
      FILE* File_src;
      FILE* File_tmp;
      FILE* File_dest;
 
      unsigned long   len_src;
      unsigned long len_tmp;
      unsigned long len_dest;
 
      unsigned char *buffer_src  = new unsigned char[MaxBufferSize];
      unsigned char *buffer_tmp  = new unsigned char[MaxBufferSize];
      unsigned char *buffer_dest = new unsigned char[MaxBufferSize];
 
      File_src = fopen("src.txt","r");
      len_src = fread(buffer_src,sizeof(char),MaxBufferSize-1,File_src);
 
     for(i = 0 ; i < len_src ; ++i)
    {
          cout<<buffer_src[i];
     }
      cout<<endl;
      compress(buffer_tmp,&len_tmp,buffer_src,len_src);
 
      File_tmp = fopen("tmp.txt","w");
      fwrite(buffer_tmp,sizeof(char),len_tmp,File_tmp);
 
     for(i = 0 ; i < len_tmp ; ++i)
    {
          cout<<buffer_tmp[i];
     }
      cout<<endl;
 
      uncompress(buffer_dest,&len_dest,buffer_tmp,len_tmp);
 
      File_tmp = fopen("tmp.txt","r");
      File_dest = fopen("dest.txt","w");
      fwrite(buffer_dest,sizeof(char),len_dest,File_dest);
 
     for(i = 0 ; i < len_dest ; ++i)
    {
          cout<<buffer_dest[i];
     }
      cout<<endl;
 
}