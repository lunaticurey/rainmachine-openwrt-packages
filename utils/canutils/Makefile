#
# Copyright (C) 2013 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=canutils
PKG_VERSION:=2014-02-27
PKG_RELEASE=$(PKG_SOURCE_VERSION)

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=git://gitorious.org/linux-can/can-utils.git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=67a2bdcd336e6becfa5784742e18c88dbeddc973
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz

include $(INCLUDE_DIR)/package.mk

FILES:=	canbusload can-calc-bit-timing candump \
	cangen cangw canplayer cansniffer cansend


define Package/canutils-log-conversion
  TITLE:=canutils log conversion
  SECTION:=utils
  CATEGORY:=Utilities
  DEPENDS:=canutils
endef

define PartGen
define Package/canutils-$(1)
  TITLE:=$(1) package from canutils
  SECTION:=utils
  CATEGORY:=Utilities
  DEPENDS:=canutils
endef
endef

define Package/canutils
  SECTION:=utils
  CATEGORY:=Utilities
  URL=https://gitorious.org/linux-can
  TITLE:=CAN userspace utilities and tools
  MENU:=1
endef

define Build/Configure
	( cd "$(PKG_BUILD_DIR)"; ./autogen.sh )
	$(call Build/Configure/Default)
endef

$(foreach file,$(FILES),$(eval $(call PartGen,$(file))))

define Package/canutils/install
	true
endef

define Package/canutils-log-conversion/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/asc2log $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/log2asc $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/log2long $(1)/usr/bin/
endef

define PartInstall
define Package/canutils-$(1)/install
	$(INSTALL_DIR) $$(1)/usr/bin
	$(INSTALL_BIN) \
		$(PKG_BUILD_DIR)/$(1) \
		$$(1)/usr/bin/
endef
endef

define Package/canutils-log-conversion/description
asc2log - convert ASC logfile to compact CAN frame logfile.
log2asc - convert compact CAN frame logfile to ASC logfile.
log2long - convert compact CAN frame representation into user readable.
endef

define Package/canutils-canbusload/description
canbusload - display the load percentage of can buses.
endef

define Package/canutils-can-calc-bit-timing/description
can-calc-bit-timing - calculate hw bittiming for supported can chips.
endef

define Package/canutils-candump/description
candump - dumps can frames to terminal, logfile or another can device,
with optional filtering.
endef

define Package/canutils-cangen/description
cangen - CAN frames generator for testing purposes.
endef

define Package/canutils-cangw/description
cangw - manage PF_CAN netlink gateway.
endef

define Package/canutils-canplayer/description
canplayer - replay a compact CAN frame logfile to CAN devices.
endef

define Package/canutils-cansend/description
cansend - simple command line tool to send CAN-frames via CAN_RAW sockets.
endef

$(foreach file,$(FILES),$(eval $(call PartInstall,$(file))))

$(eval $(call BuildPackage,canutils))
$(eval $(call BuildPackage,canutils-log-conversion))
$(foreach file,$(FILES),$(eval $(call BuildPackage,canutils-$(file))))
