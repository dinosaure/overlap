ifneq (, $(shell command -v opam))
  ifeq ($(origin PKG_CONFIG_PATH), undefined)
    PKG_CONFIG_PATH := $(shell opam var prefix)/lib/pkgconfig
  endif
endif

ifeq ($(OS), Windows_NT)
  ifneq (, $(shell command -v cygpath))
    PKG_CONFIG_PATH := $(shell cygpath --unix --path "$(PKG_CONFIG_PATH)")
  endif
endif

EXISTS := $(shell PKG_CONFIG_PATH=$(PKG_CONFIG_PATH) pkg-config --exists ocaml-freestanding; echo $$?)

.PHONY: all clean
all: liboverlap_freestanding_stubs.a

ifeq ($(EXISTS), 1)
liboverlap_freestanding_stubs.a:
	touch $@
else
CC ?= cc
CFLAGS := $(shell PKG_CONFIG_PATH=$(PKG_CONFIG_PATH) pkg-config --cflags ocaml-freestanding)

OBJS=overlap.o

liboverlap_freestanding_stubs.a: $(OBJS)
	$(AR) r $@ $^
endif

clean:
	$(RM) $(OBJS) liboverlap_freestanding_stubs.a
