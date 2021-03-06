ifeq ($(TARGET_NAME),)
TARGET_NAME=boot
endif
ifeq ($(ATOMTHREADS),)
ATOMTHREADS = $(shell pwd)/../../
endif
ifeq ($(TEST_NAME),)
TEST_NAME = kern1
endif



CC = arm-none-eabi-gcc
LN = arm-none-eabi-gcc
AS = arm-none-eabi-gcc

CFLAGS := $(CFLAGS) -Wall -g -c -mcpu=cortex-m3 -mthumb -mthumb-interwork -ffreestanding 
AFLAGS := $(AFLAGS) -Wall -g -c -mcpu=cortex-m3 -mthumb -mthumb-interwork -ffreestanding 
LFLAGS := $(LFLAGS) -Wall -mcpu=cortex-m3 -mthumb -Wl,-Map=system.map  -Tsystem.ld 

CDEFS := $(CDEFS) -DATOMTHREADS_TEST='"$(TEST_NAME)"' -DPLATFORM_QEMU_LM3S_HACK
ADEFS := $(ADEFS) -D__thumb2__ -DARM_RDI_MONITOR -DPLATFORM_QEMU_LM3S_HACK

LLIBS := $(LLIBS) 


SRCS := $(SRCS) \
		modules.c \
		main.c \
		$(ATOMTHREADS)/tests/$(TEST_NAME).c \


ASMS := $(ASMS) \
		startup.S \

INCLUDES := $(INCLUDES) \
			-I$(ATOMTHREADS)

include $(ATOMTHREADS)/ports/cortex_m/Makefile

OBJS = $(SRCS:.c=.o) $(ASMS:.S=.o)

include 	../rules.mk


run_test: clean all
	echo "START TEST $(TEST_NAME)"
	qemu-system-arm -M lm3s6965evb -kernel boot.elf  -semihosting >> atomthreads_test.out

all_tests:
	echo "Starting atomthreads test suite" > atomthreads_test.out
	make run_test "TEST_NAME=mutex1"
	make run_test "TEST_NAME=mutex2"
	make run_test "TEST_NAME=mutex3"
	make run_test "TEST_NAME=mutex5"
	make run_test "TEST_NAME=mutex6"
	make run_test "TEST_NAME=mutex7"
	make run_test "TEST_NAME=mutex8"
	make run_test "TEST_NAME=mutex9"
	make run_test "TEST_NAME=kern1"
	make run_test "TEST_NAME=kern2"
	make run_test "TEST_NAME=kern3"
	make run_test "TEST_NAME=kern4"
	make run_test "TEST_NAME=timer1"
	make run_test "TEST_NAME=timer2"
	make run_test "TEST_NAME=timer3"
	make run_test "TEST_NAME=timer5"
	make run_test "TEST_NAME=timer6"
	make run_test "TEST_NAME=timer7"
	make run_test "TEST_NAME=queue1"
	make run_test "TEST_NAME=queue2"
	make run_test "TEST_NAME=queue3"
	make run_test "TEST_NAME=queue4"
	make run_test "TEST_NAME=queue5"
	make run_test "TEST_NAME=queue6"
	make run_test "TEST_NAME=queue7"
	make run_test "TEST_NAME=queue8"
	make run_test "TEST_NAME=queue9"
	make run_test "TEST_NAME=sem1"
	make run_test "TEST_NAME=sem2"
	make run_test "TEST_NAME=sem3"
	make run_test "TEST_NAME=sem5"
	make run_test "TEST_NAME=sem6"
	make run_test "TEST_NAME=sem7"
	make run_test "TEST_NAME=sem8"
	make run_test "TEST_NAME=sem9"

fail_tests:
		make run_test "TEST_NAME=mutex4"
		make run_test "TEST_NAME=timer4"
		make run_test "TEST_NAME=sem4"


run_last:
	qemu-system-arm -M lm3s6965evb -kernel boot.elf  -semihosting

