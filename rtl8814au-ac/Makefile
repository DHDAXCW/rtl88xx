include $(TOPDIR)/rules.mk

PKG_NAME:=rtl8814au-ac
PKG_RELEASE:=1

PKG_LICENSE:=GPLv2
PKG_LICENSE_FILES:=

PKG_SOURCE_URL:=https://github.com/aircrack-ng/rtl8814au.git
PKG_SOURCE_PROTO:=git
PKG_SOURCE_DATE:=2022-05-31
PKG_SOURCE_VERSION:=bdf80b5a932d5267cd1aff66fee8ac244cd38777
PKG_MIRROR_HASH:=skip

# PKG_MAINTAINER:=
PKG_BUILD_PARALLEL:=1

STAMP_CONFIGURED_DEPENDS := $(STAGING_DIR)/usr/include/mac80211-backport/backport/autoconf.h

include $(INCLUDE_DIR)/kernel.mk
include $(INCLUDE_DIR)/package.mk

define KernelPackage/rtl8814au-ac
  SUBMENU:=Wireless Drivers
  TITLE:=Realtek rtl8814au driver
  DEPENDS:=+kmod-cfg80211 +kmod-usb-core +@DRIVER_11N_SUPPORT +@DRIVER_11AC_SUPPORT
  FILES:=\
	$(PKG_BUILD_DIR)/rtl8814au.ko
  AUTOLOAD:=$(call AutoProbe,rtl8814au)
endef

NOSTDINC_FLAGS = \
	-I$(PKG_BUILD_DIR) \
	-I$(PKG_BUILD_DIR)/include \
	-I$(STAGING_DIR)/usr/include/mac80211-backport \
	-I$(STAGING_DIR)/usr/include/mac80211-backport/uapi \
	-I$(STAGING_DIR)/usr/include/mac80211 \
	-I$(STAGING_DIR)/usr/include/mac80211/uapi \
	-include backport/autoconf.h \
	-include backport/backport.h

NOSTDINC_FLAGS += -DCONFIG_IOCTL_CFG80211 -DRTW_USE_CFG80211_STA_EVENT \
	-D_LINUX_BYTEORDER_SWAB_H -DBUILD_OPENWRT -DRTW_SINGLE_WIPHY
ifeq ($(CONFIG_BIG_ENDIAN), y)
NOSTDINC_FLAGS += -DCONFIG_BIG_ENDIAN
endif
ifeq ($(CONFIG_LITTLE_ENDIAN), y)
NOSTDINC_FLAGS += -DCONFIG_LITTLE_ENDIAN
endif

PKG_MAKE_FLAGS += USER_MODULE_NAME=rtl8814au
PKG_MAKE_FLAGS += USER_DRV_NAME=rtl8814au
KERNEL_MAKE_FLAGS += CONFIG_RTL8814AU=m

define Build/Compile
	+$(MAKE) $(PKG_JOBS) -C "$(LINUX_DIR)" \
		$(KERNEL_MAKE_FLAGS) \
		$(PKG_MAKE_FLAGS) \
		M="$(PKG_BUILD_DIR)" \
		NOSTDINC_FLAGS="$(NOSTDINC_FLAGS)" \
		modules
endef

$(eval $(call KernelPackage,rtl8814au-ac))
