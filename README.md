whiper applications : ycsb,tpcc,echo,redis,ctree,hashmap,memcached,vacation,nfs,exim,sql

# 1. whisper Download
===========================================================================
git clone --recursive https://github.com/swapnilh/whisper.git

-> successful

# 2. build whisper applications
===========================================================================
## ycsb : error

error message : 

>>> make

make  all-recursive
make[1]: 디렉터리 '/home/hk/whisper/nstore' 들어감
Making all in src
make[2]: 디렉터리 '/home/hk/whisper/nstore/src' 들어감
g++ -DHAVE_CONFIG_H -I. -I..  -I./common -Wno-pointer-arith   -Wall -Wextra -Werror  -ggdb -O3 -D_ENABLE_FTRACE -MT libpm.o -MD -MP -MF .deps/libpm.Tpo -c -o libpm.o libpm.cpp
libpm.cpp:9:31: error: dynamic exception specifications are deprecated in C++11 [-Werror=deprecated]
 void* operator new(size_t sz) throw (std::bad_alloc) {
                               ^~~~~
libpm.cpp:37:38: error: dynamic exception specifications are deprecated in C++11 [-Werror=deprecated]
 void *operator new[](std::size_t sz) throw (std::bad_alloc) {
                                      ^~~~~
cc1plus: all warnings being treated as errors
make[2]: *** [Makefile:466: libpm.o] 오류 1
make[2]: 디렉터리 '/home/hk/whisper/nstore/src' 나감
make[1]: *** [Makefile:413: all-recursive] 오류 1
make[1]: 디렉터리 '/home/hk/whisper/nstore' 나감
make: *** [Makefile:345: all] 오류 2

## tpcc :

