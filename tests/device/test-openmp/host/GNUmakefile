MYDIR := $(abspath $(dir $(lastword $(MAKEFILE_LIST))))

ifneq (__clang__,$(shell echo __clang__ | $(CC) -x c -E - | tail -n 1))

.PHONY: test
test:
	@echo "SKIP: clang doesn't support OpenMP"

else

include $(MYDIR)/../common.mk

CFLAGS += -I../jni

define add-test-rule
.PHONY: test
test: test-$(1)

.PHONY: test-$(1)
test-$(1):
	@+$(MAKE) -f $(MYDIR)/test.mk SRCFILES=$(1).c CFLAGS="$(CFLAGS)" LDFLAGS="$(LDFLAGS)" TARGETNAME=test-$(1)

.PHONY: clean-$(1)
clean-$(1):
	@+$(MAKE) -f $(MYDIR)/test.mk clean SRCFILES=$(1).c

.PHONY: clean
clean: clean-$(1)
endef

$(foreach __t,$(CTESTS),\
    $(eval $(call add-test-rule,$(__t)))\
)

endif
