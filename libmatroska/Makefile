#
# Copyright (C) 2011-2017 Entware
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=libmatroska
PKG_VERSION:=1.4.7
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:=http://dl.matroska.org/downloads/libmatroska
PKG_MD5SUM:=45b583b5642d8e0a0c69f86dbf004eb7

PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/libmatroska
	SECTION:=libs
	CATEGORY:=Libraries
	DEPENDS:=+libebml
	TITLE:=MKV container format library
	URL:=http://matroska.org/
	MAINTAINER:=Entware team, http://entware.net
endef

define Package/libmatroska/description
 libmatroska - extensible open standard Audio/Video container format library
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/opt/include
	$(CP) $(PKG_INSTALL_DIR)/opt/include/matroska $(1)/opt/include
	$(INSTALL_DIR) $(1)/opt/lib
	$(CP) $(PKG_INSTALL_DIR)/opt/lib/libmatroska.{a,la,so*} $(1)/opt/lib/
	$(INSTALL_DIR) $(1)/opt/lib/pkgconfig
	$(CP) $(PKG_INSTALL_DIR)/opt/lib/pkgconfig/libmatroska.pc $(1)/opt/lib/pkgconfig/
endef

define Package/libmatroska/install
	$(INSTALL_DIR) $(1)/opt/lib
	$(CP) $(PKG_INSTALL_DIR)/opt/lib/libmatroska.so* $(1)/opt/lib
endef

$(eval $(call BuildPackage,libmatroska))
