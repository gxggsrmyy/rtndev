#
# Copyright (C) 2011-2017 Entware
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=duktape
PKG_VERSION:=2.0.2
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.xz
PKG_SOURCE_URL:=http://duktape.org
PKG_HASH:=863204c27a18adc86795616b64b3bc3975a32b38af0f68f46d7a4adedf38089e

include $(INCLUDE_DIR)/package.mk

define Package/duktape
	SECTION:=lang
	CATEGORY:=Languages
	SUBMENU:=JavaScript
	DEPENDS:=+libreadline +libncurses
	TITLE:=Duktape JavaScript engine
	URL:=$(PKG_SOURCE_URL)
	MAINTAINER:=Entware team, http://entware.net
endef

define Package/duktape/description
 Duktape is an embeddable Javascript engine, with a focus on portability and
 compact footprint.
endef

define Build/Prepare
	$(call Build/Prepare/Default)
	$(CP) $(PKG_BUILD_DIR)/Makefile.cmdline $(PKG_BUILD_DIR)/Makefile
endef

#define Build/InstallDev
#	$(INSTALL_DIR) $(1)/opt/include/duktape
#	$(CP) $(PKG_BUILD_DIR)/src/duktape.{h,c} $(1)/opt/include/duktape
#endef

define Package/duktape/install
	$(INSTALL_DIR) $(1)/opt/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/duk $(1)/opt/bin
	$(LN) duk $(1)/opt/bin/js
endef

$(eval $(call BuildPackage,duktape))
