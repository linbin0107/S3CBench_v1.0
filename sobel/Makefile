#===========================================================================
#
# File Name    : Makefile
# Description  : Makefile for Sobel filter
# Release Date : 23/07/13
# Author       : PolYU
#
# Note         : Edit SYSTEMC_HOME path
# Revision History
#---------------------------------------------------------------------------
# Date     Author      Version     Description
#---------------------------------------------------------------------------
# 23/07/13 PolYU        1.0
#===========================================================================

TARGET	= sobel.exe

SRCS	=  \
	main.cpp \
	tb_sobel.cpp \
	sobel.cpp


HDRS	=  \
	tb_sobel.h \
	define.h \
	sobel.h

ifneq (, $(wildcard /bin/uname))
CPU_ARCH = $(shell /bin/uname -m)
endif

ifeq ($(CPU_ARCH),x86_64)
CWB_LIB   = $(CYBER_PATH)/lib64
ARCH_FLAG = 
else
CWB_LIB   = $(CYBER_PATH)/lib
ARCH_FLAG = -m32
endif

ifdef CYBER_SYSTEMC_TARGET_ARCH
CWB_SC_ARCH = $(CYBER_SYSTEMC_TARGET_ARCH)
else
ifeq (, $(findstring Windows,$(OS)))
ifeq ($(CPU_ARCH),x86_64)
CWB_SC_ARCH = linux64
else
CWB_SC_ARCH = linux
endif
else
CWB_SC_ARCH = mingw
endif
endif

ifdef CYBER_SYSTEMC_HOME
CWB_SC_HOME = $(CYBER_SYSTEMC_HOME)
else
CWB_SC_HOME = $(CYBER_PATH)/osci
endif

CWB_SC_INCL = -I"$(CWB_SC_HOME)/include"
CWB_SC_LIB  = "$(CWB_SC_HOME)/lib-$(CWB_SC_ARCH)/libsystemc.a"

EMPTY	=
SPACE	= $(EMPTY) $(EMPTY)
ifeq (, $(wildcard  $(subst $(SPACE),\$(SPACE),$(CYBER_PATH))/gcc/bin))
CC	= g++
RM	= rm
else
CC	= "$(CYBER_PATH)/gcc/bin/g++"
RM	= "$(CYBER_PATH)/gcc/bin/rm"
endif
COVER   = -fprofile-arcs -ftest-coverage 
LINKER	= $(CC)
INCL	= -I"."
CFLAGS	= $(ARCH_FLAG) -O1
LDFLAGS	= $(ARCH_FLAG)
LIBS	= -lm $(CWB_SC_LIB)  -Wl,-rpath="$(CWB_LIB)" $(COVER)
OBJS    = $(notdir $(addsuffix .o,$(basename $(SRCS))))
debug: CFLAGS += -g -DDEBUG
wave: CFLAGS += -DWAVE_DUMP
cwm: CFLAGS += -DCWM
cwom: CFLAGS += -DCWOM
swm: CFLAGS += -DSWM

$(TARGET) : $(OBJS)
	$(LINKER) -o "$@" $(LDFLAGS) $(OBJS) $(LIBS) 

debug: $(OBJS)
	$(LINKER) -o $(TARGET) $(LDFLAGS) $(OBJS) $(LIBS) $(DEBUG)

wave:  $(OBJS) 
	$(LINKER) -o $(TARGET) $(LDFLAGS) $(OBJS) $(LIBS) $(WAVEFLAG)

cwm:  $(OBJS) 
	$(LINKER) -o $(TARGET) $(LDFLAGS) $(OBJS) $(LIBS) $(CWMFLAG)

cwom:  $(OBJS) 
	$(LINKER) -o $(TARGET) $(LDFLAGS) $(OBJS) $(LIBS) $(CWOMFLAG)

swm:  $(OBJS) 
	$(LINKER) -o $(TARGET) $(LDFLAGS) $(OBJS) $(LIBS) $(SWMFLAG)

main.o: main.cpp
	$(CC) $(CFLAGS) $(CWB_SC_INCL) $(INCL) -c $< -o $@
tb_sobel.o: tb_sobel.cpp
	$(CC) $(CFLAGS) $(CWB_SC_INCL) $(INCL) -c $< -o $@
sobel.o: sobel.cpp
	$(CC) $(CFLAGS) $(CWB_SC_INCL) $(INCL) -c $< -o $@ -fprofile-arcs -ftest-coverage

clean :
	@$(RM) -f $(OBJS) $(TARGET)

depend :
	makedepend -- $(INCL) $(CFLAGS) -- -Y -fMakefile.GNU $(SRCS)

