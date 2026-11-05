KMODULE_NAME := ssv6051

KBUILD_TOP := $(shell pwd)
KVERSION := $(shell uname -r)
KDIR := /lib/modules/$(KVERSION)/build

# ============================================================
# Compiler flags (from platform-config.mak, with modifications)
# ============================================================

ccflags-y += -DCONFIG_SSV6200_CORE

# Chip variant
ccflags-y += -DCONFIG_SSV_CABRIO_E

# SDIO transport settings
ccflags-y += -DCONFIG_SSV_TX_LOWTHRESHOLD
ccflags-y += -DCONFIG_FW_ALIGNMENT_CHECK
ccflags-y += -DCONFIG_PLATFORM_SDIO_OUTPUT_TIMING=3
ccflags-y += -DCONFIG_PLATFORM_SDIO_BLOCK_SIZE=128

# *** CRITICAL: Keep SDIO clock at 25 MHz for stability on meson-gx-mmc ***
# Without this, the driver switches to 37.5 MHz after firmware upload,
# which causes sporadic EILSEQ (-84) CRC errors on X96 Mini.
ccflags-y += -DSDIO_USE_SLOW_CLOCK

# Rate control, security, features
ccflags-y += -DCONFIG_SSV_RSSI
ccflags-y += -DCONFIG_SSV_VENDOR_EXT_SUPPORT
ccflags-y += -DRATE_CONTROL_REALTIME_UPDATA
ccflags-y += -DCONFIG_SSV6200_HAS_RX_WORKQUEUE
ccflags-y += -DUSE_THREAD_TX
ccflags-y += -DENABLE_AGGREGATE_IN_TIME
ccflags-y += -DENABLE_INCREMENTAL_AGGREGATION
ccflags-y += -DUSE_GENERIC_DECI_TBL
ccflags-y += -DFW_WSID_WATCH_LIST
ccflags-y += -DSSV6200_ECO
ccflags-y += -DHAS_CRYPTO_LOCK
ccflags-y += -DENABLE_TX_Q_FLOW_CONTROL
ccflags-y += -DUSE_MAC80211_DECRYPT_BROADCAST

# Include paths
ccflags-y += -I$(KBUILD_TOP) -I$(KBUILD_TOP)/include

# ============================================================
# Module objects (from Makefile, matches in-tree exactly)
# ============================================================

obj-m += $(KMODULE_NAME).o

$(KMODULE_NAME)-objs := \
        ssv6051-generic-wlan.o \
        ssvdevice/ssvdevice.o \
        ssvdevice/ssv_cmd.o \
        hci/ssv_hci.o \
        smac/init.o \
        smac/dev.o \
        smac/ssv_rc.o \
        smac/ssv_ht_rc.o \
        smac/ap.o \
        smac/ampdu.o \
        smac/efuse.o \
        smac/ssv_pm.o \
        smac/sar.o \
        smac/ssv_cfgvendor.o \
        hwif/sdio/sdio.o

# ============================================================
# Build targets
# ============================================================

.PHONY: all clean install uninstall

all:
	$(MAKE) -C $(KDIR) M=$(KBUILD_TOP) modules

clean:
	$(MAKE) -C $(KDIR) M=$(KBUILD_TOP) clean
