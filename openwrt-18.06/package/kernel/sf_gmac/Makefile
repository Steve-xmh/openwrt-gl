#
# Copyright (C) 2008-2010 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=sf_gmac
PKG_RELEASE:=1

include $(INCLUDE_DIR)/kernel.mk
include $(INCLUDE_DIR)/package.mk

define KernelPackage/sf_gmac/config
	if PACKAGE_kmod-sf_gmac

		config SFAX8_RGMII_GMAC
			bool "select RGMII mode gmac"
			default "y"

		config SFAX8_RMII_GMAC
			bool "select RMII mode gmac"
			default "n"

		config SFAX8_FPGA_GMAC
			bool "select FPGA mode gmac"
			default "n"

		config SFAX8_HNAT_DRIVER
			bool "hnat driver support"
			depends on PACKAGE_kmod-sf_hnat
			default "n"

		config SFAX8_ESWITCH_DRIVER
			bool "eswitch driver support"
			depends on PACKAGE_kmod-sf_eswitch
			default "n"

		config SFAX8_HNAT_TEST_TOOL
			bool "set test tool"
			depends on SFAX8_HNAT_DRIVER
			default "n"

		config SFAX8_IFG_ENABLE
			bool "gmac/eswitch IFG setting enable"
			default "n"
	endif
endef

EXTRA_KCONFIG:= \
	CONFIG_NET_VENDOR_SIFLOWER=y \
	CONFIG_SFAX8_GMAC=m \


ifdef CONFIG_TARGET_siflower_sf16a18_fullmask
NOSTDINC_FLAGS += -DCONFIG_SF16A18_FULL_MASK
endif

ifdef CONFIG_TARGET_siflower_sf19a28_mpw1
NOSTDINC_FLAGS += -DCONFIG_SF19A28_MPW1
endif

ifdef CONFIG_TARGET_siflower_sf19a28_fullmask
NOSTDINC_FLAGS += -DCONFIG_SF19A28_FULLMASK
endif

ifdef CONFIG_SFAX8_RGMII_GMAC
NOSTDINC_FLAGS += -DCONFIG_SFAX8_RGMII_GMAC
endif

ifdef CONFIG_SFAX8_HNAT_DRIVER
EXTRA_KCONFIG += CONFIG_SFAX8_HNAT_DRIVER=y
NOSTDINC_FLAGS += -DCONFIG_SFAX8_HNAT_DRIVER
ifdef CONFIG_SFAX8_HNAT_TEST_TOOL
EXTRA_KCONFIG += CONFIG_SFAX8_HNAT_TEST_TOOL=y
NOSTDINC_FLAGS += -DCONFIG_SFAX8_HNAT_TEST_TOOL
endif
endif

ifdef CONFIG_SFAX8_FACTORY_READ
NOSTDINC_FLAGS += -DCONFIG_SFAX8_FACTORY_READ
endif



ifdef CONFIG_SFAX8_ESWITCH_DRIVER
EXTRA_KCONFIG += CONFIG_SFAX8_ESWITCH_DRIVER=y
NOSTDINC_FLAGS += -DCONFIG_SFAX8_ESWITCH_DRIVER
endif

ifdef CONFIG_SFAX8_GENL
EXTRA_KCONFIG += CONFIG_SFAX8_GENL=y
NOSTDINC_FLAGS += -DCONFIG_SFAX8_GENL
endif

ifdef CONFIG_SFAX8_IFG_ENABLE
EXTRA_KCONFIG += CONFIG_SFAX8_IFG_ENABLE=y
NOSTDINC_FLAGS += -DCONFIG_SFAX8_IFG_ENABLE
endif

define KernelPackage/sf_gmac
  SUBMENU:=Network Devices
  TITLE:=Siflower SFA18 gmac support
  DEPENDS:=+kmod-sfax8-factory-read
  FILES:=$(PKG_BUILD_DIR)/sgmac.ko
  AUTOLOAD:=$(call AutoLoad,60,sgmac,1)
  KCONFIG:=
endef


ifdef CONFIG_SFAX8_RMII_GMAC
NOSTDINC_FLAGS += -DCONFIG_SFAX8_RMII_GMAC
endif

ifdef CONFIG_SFAX8_FPGA_GMAC
NOSTDINC_FLAGS += -DCONFIG_SFAX8_FPGA_GMAC
endif

SFAX8_KO_NAME="a28_sgmac.ko"

ifneq ($(findstring EVB-V5,$(PROFILE)),)
SFAX8_KO_NAME="evb_v5_sgmac.ko"
endif

define Build/Prepare
	mkdir -p $(PKG_BUILD_DIR)
	$(CP) ./Makefile $(PKG_BUILD_DIR)/
endef

define Build/Compile
	$(CP) ./$(SFAX8_KO_NAME) $(PKG_BUILD_DIR)/sgmac.ko
endef

$(eval $(call KernelPackage,sf_gmac))
