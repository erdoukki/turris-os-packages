#
## Copyright (C) 2019 CZ.NIC z.s.p.o. (http://www.nic.cz/)
#
## This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
# #
#
include $(TOPDIR)/rules.mk

PKG_NAME:=czmq
PKG_VERSION:=4.1.1
PKG_RELEASE:=2

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://codeload.github.com/zeromq/czmq/tar.gz/v$(PKG_VERSION)?
PKG_HASH:=b7623433547aa4b6e79722796c27ebc7c0470fea4204e920fd05e717c648f889

PKG_INSTALL:=1
PKG_FIXUP:=autoreconf

PKG_LICENSE:=MPLv2
PKG_LICENSE_FILES:=LICENSE
PKG_MAINTAINER:=CZ.NIC <packaging@turris.cz>

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/autotools.mk

define Package/czmq
	SECTION:=libs
	CATEGORY:=Libraries
	TITLE:=CZMQ
	DEPENDS:=+libzmq +libuuid +libpcre +liblz4
endef

define Package/czmq/description
	High-level C binding for ØMQ
endef

TARGET_CFLAGS += --std=c99
CONFIGURE_ARGS += --without-docs

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/* $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/usr/include
	$(CP) $(PKG_INSTALL_DIR)/usr/include/* $(1)/usr/include/
	$(INSTALL_DIR) $(1)/usr/lib/pkgconfig
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/pkgconfig/* $(1)/usr/lib/pkgconfig/
endef

define Package/czmq/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/zmakecert $(1)/usr/bin/zmakecert
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libczmq.so* $(1)/usr/lib/
endef

$(eval $(call BuildPackage,czmq))
