zlib���ṩ����ѹ���ĺ�ʽ�⣬zlib����ҳ��:http://www.zlib.net/

���غ�zlib������zlib-1.2.8\contrib\vstudio�ҵ���Ӧ�İ汾�����б�����dll��zlib.lib
����� zlib.lib ��, ��͵õ��˵���һ����̬������Ҫ�������ļ���(zlib.lib, zlib.h, zconf.h). ��ε��þ�̬�ⲻ����˵�˰�.
�ؼ��ĺ�����
��1��int compress (Bytef *dest,   uLongf *destLen, const Bytef *source, uLong sourceLen);

��Դ����ѹ����Ŀ�Ļ���, ����ô��, һ�������㶨
(2) int compress2 (Bytef *dest,   uLongf *destLen,const Bytef *source, uLong sourceLen,int level);

���ܺ���һ������һ��,��һ����������ָ��ѹ��������ѹ������֮��Ĺ�ϵ(0-9)���ҿ϶���������Ļ�����̫������,����һ������ͺ���: Ҫ��õ��ߵ�ѹ���Ⱦ�Ҫ�໨ʱ��

(3) uLong compressBound (uLong sourceLen);

������Ҫ�Ļ���������. ��������ѹ��֮ǰ����֪����Ĳ���Ϊ sourcelen ������ѹ�����ж��, �ɵ��������������һ��,������������ܵõ���ȷ�Ľ��,���������Ա�֤ʵ��������ȿ϶�С������������ĳ���

(4) int uncompress (Bytef *dest,   uLongf *destLen,const Bytef *source, uLong sourceLen);

��ѹ��(�����־�֪����:)

(5) deflateInit() + deflate() + deflateEnd()

3���������ʹ�����ѹ������,�����÷��� example.c �� test_deflate()����. ��ʵ compress() �����ڲ���������3������ʵ�ֵ�(���� zlib �� compress.c �ļ�)

(6) inflateInit() + inflate() + inflateEnd()

��(5)����,��ɽ�ѹ������.

(7) gz��ͷ�ĺ���. ��������*.gz���ļ�,���ļ�stdio���÷�ʽ����. ��֪����ô�õĻ���example.c �� test_gzio() ����,��easy.

(8) ���������ð汾�Ⱥ����Ͳ�˵��.

�ܽ�: �ڱ����ʱ����Ҫ����Ĺ������һ��Ԥ����꣺��������->C++->Ԥ�����->Ԥ����������(��һ��)��
����ZLIB_WINAPI����꣮��һ�����Ҫ��������Ĺ��̻���벻ͨ����

���úù��̺���Ϳ���ʹ���ˣ�����Ҫ�õ���compress��uncompress����������������÷���λ����
ȥ�������������ǿ�API�ĵ���ʹ�úܼ򵥣�
�ٸ����ӣ�

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