include $(ROOTDIR)/config.arch

BOGUS_TARGETS		:= FORCE makefile $(ROOTDIR)/config.arch

CC			:= $(CROSS_COMPILE)gcc
STRIP			:= $(CROSS_COMPILE)strip
CFLAGS			:= -Os

export CC CFLAGS

# DNS resolution hangs with IPv6 enabled
CFGOPTS			:= --host=$(MACHINE)-linux --disable-ipv6

ifeq ($(CONFIG_USER_WGET_WITH_SSL),y)
CFGOPTS			+= --with-ssl=$(ROOTDIR)/lib/openssl
endif

.PHONY: all
all::
	if [ ! -e Makefile ]; then \
		./configure $(CFGOPTS) || exit 1 ; \
		touch configure config.status ; \
	fi
	$(MAKE) -f Makefile

.PHONY: clean distclean
clean distclean::
	[ ! -f Makefile ] || $(MAKE) -f Makefile $@

.PHONY: romfs
romfs::
	$(STRIP) src/wget
	$(ROMFSINST) -e CONFIG_USER_WGET src/wget /bin/wget

# These targets must not be passed through to the original Makefile
.PHONY: $(BOGUS_TARGETS)
$(BOGUS_TARGETS)::

# Everything else (maybe including clean, distclean) does get passed through
%:: FORCE
	$(MAKE) -f Makefile $@
