CC			:=	$(PREFIX)gcc
CXX			:=	$(PREFIX)g++
AR			:=	$(PREFIX)ar
OBJCOPY		:=	$(PREFIX)objcopy

UNAME		:=	$(shell uname -s)

CFLAGS		:=	$(DEBUGFLAGS) -O2
CFLAGS		+=	$(INCLUDES)
CXXFLAGS	:=	$(CFLAGS) -fno-rtti -fno-exceptions
LDFLAGS		=	$(DEBUGFLAGS)

ifeq ($(strip $(PS3DEV)),)
  ifeq ($(strip $(DEVKITPS3)),)
    export PS3DEV := /usr/local/ps3dev
  else
    export PS3DEV := $(DEVKITPS3)
  endif
endif

ifneq (,$(findstring MINGW,$(UNAME)))
	PLATFORM	:= win32
	POSTFIX		:= .exe
	CFLAGS		+= -mno-cygwin
	LDFLAGS		+= -mno-cygwin -s -lgmp -lcrypto -lz
	OS			:= win32
endif

ifneq (,$(findstring CYGWIN,$(UNAME)))
	LDFLAGS		+= -s -lgmp -lcrypto -lz
	POSTFIX		:= .exe
	OS			:= win32
endif

ifneq (,$(findstring Darwin,$(UNAME)))
	OSXCFLAGS	:= -mmacosx-version-min=10.4 -arch i386 -arch ppc
	OSXCXXFLAGS	:=	$(OSXCFLAGS)
	CXXFLAGS	+=	-fvisibility=hidden
	LDFLAGS		+= -mmacosx-version-min=10.4 -arch i386 -arch ppc -Wl,-syslibroot,$(SDK)
endif

ifneq (,$(findstring Linux,$(UNAME)))
	LDFLAGS += -lgmp -lcrypto -lz
	OS		:= Linux
endif

TARGETS	:=	make_self$(POSTFIX) make_self_npdrm$(POSTFIX) package_finalize$(POSTFIX)

all: $(TARGETS)

make_self$(POSTFIX): make_self.c
	@$(CC) $< $(CFLAGS) $(LDFLAGS) -o $@

make_self_npdrm$(POSTFIX): make_self.c
	@$(CC) $< $(CFLAGS) $(LDFLAGS) -DNPDRM -o $@

package_finalize$(POSTFIX): package_finalize.c
	@$(CC) $< $(CFLAGS) $(LDFLAGS) -o $@

install: $(TARGETS)
	@echo Installing $(TARGETS)
	@[ -d $(PS3DEV)/bin ] || mkdir -p $(PS3DEV)/bin
	@install -m 755 $(TARGETS) $(PS3DEV)/bin

clean:
	@echo clean ...
	@rm -f $(TARGETS)

.PHONY: install clean
