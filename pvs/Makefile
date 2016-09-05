CURRENT_PATH=$(shell pwd)
BASE_PATH=$(CURRENT_PATH)/../..
include $(BASE_PATH)/build/linux/header.mk

BIN_PATH=$(BASE_PATH)/$(BINTARGET)
LIB_PATH=$(BASE_PATH)/$(LIBTARGET)
TRDLIB_PATH=$(BASE_PATH)/$(TRDLIB)

ifeq ($(RELEASE),1)
BIN_NAME=libpvs.so
else
BIN_NAME=libpvsd.so
endif
CFLAGS+=-DPVS_EXPORTS

INCLUDE=-I$(VION_BASE_INC) -I$(BASE_PATH)/code/include -I$(BASE_PATH)/code/include/linux -I$(BASE_PATH)/code/utils -I$(CURRENT_PATH)

#��ԭ�������Ͻ�debug��release�����ֿ�,debug�汾ִ��ʱ�����û������������Ӹ�Ŀ¼�µ�lib_d
ifeq ($(RELEASE),1)	
LIB=-L$(USR_LIB) \
	-L$(BIN_PATH) \
	-L$(LIB_PATH) \
	-L$(VION_BASE_LIB) \
	-lSCImage \
	-lSCCommon \
	-lSCOS \
	-lSCAppCommon -lasound -lini \
	-lz \
    -lSCXml  
#    
else
LIB=-L$(USR_LIB) \
	-L$(LIB_PATH) \
	-L$(BIN_PATH) \
	-lSCImaged \
	-lSCCommond \
	-lSCOSd \
	-lzd \
    -lSCXmld\
    
	#libjpeg.a 
endif

ifeq ($(PLATFORM),TK1)
	ifeq ($(RELEASE),1)	
	LIB+=-lxml2 -liconv  \
	-lSCAppCommon -lasound -lini
	else
	LIB+=-lxml2 -liconv  \
	-lSCAppCommond -lasound -lini 
	endif
else
	ifeq ($(RELEASE),1)	
	LIB+=-lxml2 -liconv -ljpeg
	else
	LIB+=-lxml2d -liconvd
	endif
endif

LIB+=-pthread -ldl -lrt


.PHONY:all pvs prepare clean

all:prepare pvs

pvs:$(OBJS)
	$(PP) $(LDFLAGS) -o $(BIN_PATH)/$(BIN_NAME) $^ $(LIB)

$(CXXOBJTARGET)
#�����Ż�ѡ��-march����ݴ������ܹ����б����Ż������ض��Ĵ��루����ֲ�Բ���������Ʋ��ɼ�á�
ifeq ($(PLATFORM),TK1)
	$(PP) $(CFLAGS) $< -o $@ $(INCLUDE)
else
	$(PP) $(CFLAGS) -march=i686 $< -o $@ $(INCLUDE)
endif

prepare:
	@if [ ! -e $(INTDIR) ]; then mkdir -p $(INTDIR); fi
	@if [ ! -e $(BIN_PATH) ]; then mkdir -p $(BIN_PATH); fi

clean:
	rm -f $(BIN_PATH)/$(BIN_NAME) $(OBJS)