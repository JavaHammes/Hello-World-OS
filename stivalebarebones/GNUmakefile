override KERNEL := myos.elf

ifeq ($(origin CC), default)
CC := cc
endif

ifeq ($(origin LD), default)
LD := ld
endif

CFLAGS ?= -O2 -g -Wall -Wextra -pipe

LDFLAGS ?=

override INTERNALCFLAGS :=	\
	-I.			\
	-std=gnu11		\
	-ffreestanding		\
	-fno-stack-protector	\
	-fno-pic		\
	-mabi=sysv		\
	-mno-80387		\
	-mno-mmx		\
	-mno-3dnow		\
	-mno-sse		\
	-mno-sse2		\
	-mno-red-zone		\
	-mcmodel=kernel		\
	-MMD

override INTERNALLDFLAGS :=	\
	-Tlinker.ld		\
	-nostdlib		\
	-zmax-page-size=0x1000	\
	-static

override CFILES := $(shell find ./ -type f -name '*.c')
override OBJ := $(CFILES:.c=.o)
override HEADER_DEPS := $(CFILES:.c=.d)

.PHONY: all
all: $(KERNEL)

$(KERNEL): $(OBJ)
	$(LD) $(OBJ) $(LDFLAGS) $(INTERNALLDFLAGS) -o $@

-include $(HEADER_DEPS)+
%.o: %.c
	$(CC) $(CLAGS) $(INTERNALCFLAGS) -c $< -o $@

.PHONY: clean
clean:
	rm -rf $(KERNEL) $(OBJ) $(HEADER_DEPS)
