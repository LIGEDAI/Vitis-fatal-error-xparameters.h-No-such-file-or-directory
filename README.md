# Vitis-fatal-error-xparameters.h-No-such-file-or-directory
此报错的原因为官方软件BUG
解决方法：
1.IP核封装完成后，在IP核所在路径下（X:\XX\你自己项目的文件夹\ip_repo\你的IP核的名字\drivers\你的IP核的名字\src），找到Makefile文件。
2.打开原始的Makefile文件之后，文件里的语句如下：
1  COMPILER= 
2  ARCHIVER= 
3  CP=cp 
4  COMPILER_FLAGS= 
5  EXTRA_COMPILER_FLAGS= 
6  LIB=libxil.a 
7   
8  RELEASEDIR=../../../lib 
9  INCLUDEDIR=../../../include 
10 INCLUDES=-I./. -I${INCLUDEDIR} 
11  
12 INCLUDEFILES=*.h 
13 LIBSOURCES=*.c 
14 OUTS = *.o 
15  
16 libs: 
17  echo "Compiling breath_led_ip..." 
18  $(COMPILER) $(COMPILER_FLAGS) $(EXTRA_COMPILER_FLAGS) $(INCLUDES) $(LIBSOURCES) 
19  $(ARCHIVER) -r ${RELEASEDIR}/${LIB} ${OUTS} 
20  make clean 
21  
22 include: 
23  ${CP} $(INCLUDEFILES) $(INCLUDEDIR) 
24  
25 clean: 
26  rm -rf ${OUTS}
3.修改方法如下：
先在上述语句的第15行添加两行语句： 
OBJECTS = $(addsuffix .o, $(basename $(wildcard *.c))) 
ASSEMBLY_OBJECTS = $(addsuffix .o, $(basename $(wildcard *.S))) 
然后再将上述语句的第19行和第26行的${OUTS}替换成${OBJECTS} ${ASSEMBLY_OBJECTS}。
