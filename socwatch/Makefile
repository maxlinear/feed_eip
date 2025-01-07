# Copyright (C) 2020 Intel Corporation
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=socwatch
PKG_VERSION:=v2020.2
PKG_BUILD_DIR:= ${BUILD_DIR}/${PKG_NAME}-${PKG_VERSION}

STRIP=/bin/true
RSTRIP=/bin/true

include $(INCLUDE_DIR)/package.mk

define KernelPackage/$(PKG_NAME)
	SUBMENU:=MaxLinear
	TITLE:=Socwatch
	FILES:=$(PKG_BUILD_DIR)/drivers/socwatch2_11.ko
	AUTOLOAD:=$(call AutoLoad,80,socwatch2_11)
	DEPENDS:=+libstdcpp libc +@KERNEL_FTRACE
endef

define KernelPackage/$(PKG_NAME)/description
	Use Intel® SoC Watch to perform energy analysis on a Linux*, Windows*,
	or Android* system running on Intel® architecture. Study power
	consumption in the system and identify behaviors that waste energy.
	Intel SoC Watch generates a summary text report or you can import
	results into Intel® VTune™ Profiler.
endef

define KernelPackage/${PKG_NAME}/install
	$(INSTALL_DIR) $(1)/opt/intel/${PKG_NAME}; \
	$(INSTALL_DIR) $(1)/opt/intel/${PKG_NAME}/configs; \
	$(INSTALL_DIR) $(1)/opt/intel/${PKG_NAME}/plugins; \
	$(INSTALL_DIR) $(1)/opt/intel/${PKG_NAME}/output_configs; \
	$(INSTALL_DIR) $(1)/etc/profile.d; \
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/socwatch $(1)/opt/intel/${PKG_NAME}; \
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/configs/SOCWatchConfig.txt \
		$(1)/opt/intel/${PKG_NAME}/configs/; \
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/plugins/libSWCore.so \
		$(1)/opt/intel/${PKG_NAME}/plugins/;
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/output_configs/cpu-c-p-state_config.txt \
		$(1)/opt/intel/${PKG_NAME}/output_configs/;
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/output_configs/epu_dstate_config.txt \
		$(1)/opt/intel/${PKG_NAME}/output_configs/;
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/output_configs/lgm_temperature_config.txt\
		$(1)/opt/intel/${PKG_NAME}/output_configs/;
	$(INSTALL_BIN) ./files/socwatch.sh $(1)/etc/profile.d/
endef

define Build/Prepare
	$(call Build/Prepare/Default)
	chmod u+x $(PKG_BUILD_DIR)/build_drivers.sh
	chmod u+x $(PKG_BUILD_DIR)/socwatch_driver/build_linux_driver.sh
endef

define Build/Configure
endef

define Build/Compile
	(cd $(PKG_BUILD_DIR); \
		./build_drivers.sh -n -l  -k ${LINUX_DIR} \
			-c $(TARGET_CC) ; \
	)
endef

$(eval $(call KernelPackage,$(PKG_NAME)))
