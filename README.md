whiper applications : ycsb, tpcc, echo, redis, ctree, hashmap, memcached, vacation, nfs, exim, sql <br/>
This is really beginning version. I will update gradually.<br/>
And on same kinds of machine, the results can be different.

# 1. whisper Download
git clone --recursive https://github.com/swapnilh/whisper.git

-> successful

# 2. build whisper applications
## ycsb, tpcc : completed.

It may incur error message.
error message : dynamic exception specifications are deprecated in C++11 [-Werror=deprecated]
Resolve it by removing '-Werror' option in Makefile.

## memcached : error

error message :

(LINK)     build/library/pmalloc/libpmalloc.so
(LINK)     build/bench/memcached/memcached-1.2.4-mtm/libpvar.so
/bin/ld: cannot find -lalps
collect2: error: ld returned 1 exit status
scons: *** [build/bench/memcached/memcached-1.2.4-mtm/libpvar.so] Error 1
scons: building terminated because of errors.

## vacation : error

error message : 

(COMPILE)  build/bench/stamp-kozy/vacation/pvar.c
(LINK)     build/bench/stamp-kozy/vacation/libpvar.so
/bin/ld: cannot find -lalps
collect2: error: ld returned 1 exit status
scons: *** [build/bench/stamp-kozy/vacation/libpvar.so] Error 1
scons: building terminated because of errors.

---> dynamic exception specifications are deprecated, header does not exist, or cannot find some options.

## nfs, exim, sql: error

error message : please visit github.com/snalli/PMFS-new

## echo : completed(too much warning)

## redis : completed(too much warning)

## ctree : completed

## hashmap : completed


# 3. Run whisper applications
## ycsb, tpcc : No such file or directory

## ctree

[whisper]$ ./script.py -r -z 'small' -w 'ctree'

\>>> ./run_ctree.sh --small

failed to enable tracing. err = -1
failed to enable tracing. err = -1
map_insert [1]
total-avg;ops-per-second;total-max;total-min;total-median;total-std-dev;latency-avg;latency-min;latency-max;latency-std-dev;threads;ops-per-thread;data-size;seed;repeats;type;seed;max-key;external-tx;alloc
removing file failed: Operation not permitted
TOTAL EPOCH COUNT : 0, RUNTIME : 409 us
exiting main.

## hashmap

[whisper]$ ./script.py -r -z 'small' -w 'hashmap'

\>>> ./run_hashmap.sh --small

failed to enable tracing. err = -1
failed to enable tracing. err = -1
map_insert [1]
total-avg;ops-per-second;total-max;total-min;total-median;total-std-dev;latency-avg;latency-min;latency-max;latency-std-dev;threads;ops-per-thread;data-size;seed;repeats;type;seed;max-key;external-tx;alloc
removing file failed: Operation not permitted
TOTAL EPOCH COUNT : 0, RUNTIME : 204 us
exiting main.

## memcached

[whisper]$ ./script.py -r -z 'small' -w 'memcached'

\>>> ./run_memcache.sh

memcached: no process found
./run_memcache.sh: line 18: ./build/bench/memcached/memcached-1.2.4-mtm/memcached: No such file or directory

\>>> starting memslap client


\>>> ./run_memslap.sh --small

/home/hk/whisper/mnemosyne-gcc/usermode/bench/memcached/memslap: error while loading shared libraries: libevent-2.0.so.5: cannot open shared object file: No such file or directory
/home/hk/whisper/mnemosyne-gcc/usermode/bench/memcached/memslap: error while loading shared libraries: libevent-2.0.so.5: cannot open shared object file: No such file or directory
/home/hk/whisper/mnemosyne-gcc/usermode/bench/memcached/memslap: error while loading shared libraries: libevent-2.0.so.5: cannot open shared object file: No such file or directory
/home/hk/whisper/mnemosyne-gcc/usermode/bench/memcached/memslap: error while loading shared libraries: libevent-2.0.so.5: cannot open shared object file: No such file or directory

\>>> kill -s SIGKILL `pgrep memcached`

sh: line 0: kill: SIGKILL: invalid signal specification
[whisper]$

## echo

[whisper]$ ./script.py -r -z 'small' -w 'echo'

\>>> ./run.sh --small

Unable to allocate memory pool
0.00user 0.00system 0:00.03elapsed 58%CPU (0avgtext+0avgdata 4496maxresident)k
0inputs+0outputs (0major+337minor)pagefaults 0swaps

## nfs, exim, sql : error

error message : please visit github.com/snalli/PMFS-new

## redis : executed well.

----------------------------------------------------------
[whisper]$ ./script.py -r -z 'small' -w 'redis'

\>>> ./run-redis-server.sh

failed to enable tracing. err = -1
41188:C 21 Jul 15:26:20.064 * Start init Persistent memory file /dev/shm/redis.pm size 2.00G
41188:C 21 Jul 15:26:20.064 # Cannot int persistent memory file /dev/shm/redis.pm size 2.00G

\>>> starting redis client


\>>> ./run-redis-cli.sh --small

63250 Gets/sec | Hits: 63250 (100.00%) | Misses: 0 (0.00%) <br/>
63250 Gets/sec | Hits: 63250 (100.00%) | Misses: 0 (0.00%) <br/> 
61000 Gets/sec | Hits: 61000 (100.00%) | Misses: 0 (0.00%) <br/>
....(repetition) <br/>
.... <br/>
61000 Gets/sec | Hits: 61000 (100.00%) | Misses: 0 (0.00%) <br/>
\>>> kill -s SIGKILL `pgrep redis`

sh: line 0: kill: SIGKILL: invalid signal specification
[whisper]$

----------------------------------------------------------
There's a message that 'Cannot int persistent memory' at the beginning of the exectuion, I regarded this as the problem which happens in non-PM situation. and I searched the operation of the remaining part in operation message. the opeartion of remaining part is written in run-redis-cli.sh file.<br/>

the remaining part is simulating a cache workload with an 80-20 distribution(--lru-test option). The detailed opeation is the function 'LRUTestMode' in redis-cli.c. That function has so many inner user-defined functions, so it is difficult to analyze. But simply it performs cycles of 1 second with 50% writes and 50% reads. I will keep analyzing it.

------------------------------------------------------------
<br/>If you have anything not understood, please ask to gwak0320@gmail.com.

------------------------------------------------------------
-- What I did last week 
- (Ongoing) Read paper on Whisper
- (Ongoing) Build and Execute applications in Whisper benchmark suite
- (Ongoing) Analyze the result of application(redis) which can be executed, and the cause of application(others) which cannot be executed with Dongui

-- What I will do for next two weeks
- The things which are ongoing on last week
- Execute applications in Whisper on 3DXpoint and analyze the result
