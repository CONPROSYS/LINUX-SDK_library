-include version
ifndef VERSION
VERSION = 1.0.4
endif

#set architecture
ifeq "${ARCH}" ""
 ifeq "${CPS_SDK_TARGET_NAME}" ""
  # without CONPROSYS Linux SDK
  ARCH=arm
  CROSS_COMPILE=arm-linux-gnueabi-
 else
  # CONPROSYS Linux SDK
  ARCH=$(shell arch)
 endif
endif

ifeq "${CROSS_COMPILE}" "arm-linux-gnueabihf-"
	HF_OPT=hf
endif

CC=${CROSS_COMPILE}gcc
LD=${CROSS_COMPILE}ld
TARGET=libCpsCnt.so
OBJ=libcpscnt.o
SRC=libcpscnt.c
CFLAGS= -g -Wall -DCONPROSYS_MAKEFILE_VERSION=${VERSION}
INCLUDE= -I$(CPS_SDK_ROOTDIR)/driver/cps-drivers/include -I$(CPS_SDK_ROOTDIR)/lib/cps-drivers/include
TARGET_ROOTFS   := ${CPS_SDK_INSTALL_FULLDIR}/${CPS_SDK_ROOTFS}

all:$(OBJ) ${TARGET}

$(OBJ):	$(SRC) ../include/libcpscnt.h
	${CC} ${INCLUDE} ${LD_FLAGS} $(SRC) -c -fPIC -o $(OBJ) -lm

$(TARGET): $(OBJ)
	${CC} ${INCLUDE} ${LD_FLAGS}  -shared -O2 -Wl,-soname,$(TARGET) -o $(TARGET) $(OBJ) -lm

install:
	cp -p $(TARGET) $(TARGET_ROOTFS)/usr/local/lib/$(TARGET).$(VERSION)
	@if [ ! -f $(TARGET_ROOTFS)/usr/local/lib/$(TARGET) ]; then \
	( cd $(TARGET_ROOTFS)/usr/local/lib/ && ln -s $(TARGET).$(VERSION) $(TARGET) ) \
	fi

release_copy:
	cp -p ./$(TARGET) $(CPS_SDK_ROOTDIR)/release/$(ARCH)$(HF_OPT)/$(TARGET).$(VERSION)

clean:
	rm -f $(OBJ) $(TARGET)
