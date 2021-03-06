# Makefile for the OpenRTOS part of OpenOSEK

#output EXE
TARGET = OpenAPP

#Base Directory
BD = ../../..

OUT = $(BD)/OpenAPP/out

#common compilers
AS = gcc
CC = gcc
LD = gcc
AR = ar
RM = rm
OILCC = python $(BD)/OpenTOOL/osek/oilcc.py
OILCC-I = -I$(BD)/OpenCom/config

#common flags
ASFLGS = 
INCDIR = 	-I$(BD)/OpenRTOS/inc \
			-I$(BD)/OpenDriver/inc \
			-I$(BD)/OpenCom/inc \
			-I$(BD)/OpenCom/config \
			-I$(BD)/OpenAPP/inc	\
			-I$(BD)/OpenAPP/out	\
			-I$(BD)/OpenRTOS/src	\
			-I$(BD)/OpenRTOS/src/portable/posix

MACROS = 
CCFLGS = -c -g $(MACROS) -O2 -Wall $(INCDIR)
LDFLGS = -g -O2 -Wall $(INCDIR) -lpthread
ARFLGS = rv
RMFLGS = -f	


#common rules
%.o:%.c
	@echo compile $<  to $@
	@$(CC) $(CCFLGS) -o $@ $<

.PHONY:all clean gen

#common objectives		
dir-y =	$(BD)/OpenAPP/src	\
		$(BD)/OpenRTOS/src \
		$(BD)/OpenRTOS/src/portable/posix \
		$(BD)/OpenDriver/src/posix \
		$(BD)/OpenCom/src \
		$(BD)/OpenCom/config \
		$(BD)/OpenAPP/out
src-y = $(foreach x,$(dir-y),$(wildcard $(addprefix $(x)/*,.c)))
obj-y = $(subst .c,.o,$(src-y))	

all:$(obj-y)
	@$(LD) $(BD)/OpenAPP/src/start/main.c -o $(OUT)/$(TARGET) $(obj-y) $(LDFLGS)
	@echo ">>>>>>>>>>>>>>>>>  BUILD $(TARGET).EXE DONE   <<<<<<<<<<<<<<<<<<<<<<"	

gen:
	@($(OILCC) $(OILCC-I) -S $(OUT)/OpenAPP.csv -o $(OUT) $(BD)/OpenAPP/config/OpenAPP.oil)
	@echo ">>>>>>>>>>  OILCC BUILD DONE <<<<<<<<<<<<<<<< "
	
run:
#	@(sh python $(BD)/OpenTOOL/osek/CanBusServer.py --server 8000 &)
#	@(sh python $(BD)/OpenTOOL/osek/uds.py --port 8999 &)
	@($(OUT)/$(TARGET) 16 &) 
	@($(OUT)/$(TARGET) 32 &)
	@($(OUT)/$(TARGET) 48 &)
	@($(OUT)/$(TARGET) 64 &)
	@($(OUT)/$(TARGET) 81 &)
	
clean:
	@echo remove $(obj-y) $(OUT)/*
	@$(RM) $(RMFLGS) $(obj-y) $(OUT)/*
	@echo ">>>>>>>>>>>>>>>>>  CLEAN $(TARGET) DONE   <<<<<<<<<<<<<<<<<<<<<<"
