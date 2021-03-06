#
## Copyright (C) 2018 CZ.NIC z.s.p.o. (http://www.nic.cz/)
#
## This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
# #
#
include $(TOPDIR)/rules.mk

PKG_NAME:=sentinel-nikola
PKG_VERSION:=1
PKG_RELEASE:=1
PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://gitlab.labs.nic.cz/turris/nikola.git
PKG_SOURCE:=$(PKG_NAME).tar.gz
PKG_SOURCE_VERSION:=be7a13ecdc53073c9c9cf4da2fffdc22ac1e0ca9
PKG_SOURCE:=$(PKG_NAME)-$(PKG_SOURCE_VERSION).tar.gz
PKG_SOURCE_SUBDIR:=$(PKG_NAME)
PKG_MAINTAINER:=CZ.NIC <packaging@turris.cz>
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)

include $(INCLUDE_DIR)/package.mk
$(call include_mk, python3-package.mk)

define Package/$(PKG_NAME)
	TITLE:=$(PKG_NAME)
	DEPENDS:=+python3-light +python3-setuptools +logrotate +syslog-ng3 +firewall +python3-msgpack +python3-zmq +sentinel-proxy
	CONFLICTS:=nikola
endef

define Package/$(PKG_NAME)/description
	Collects firewall logs and sends it to sentinel.
endef

define Build/Compile
	$(call Build/Compile/Py3Mod,, install --prefix="/usr" --root="$(PKG_INSTALL_DIR)")
endef

define Package/$(PKG_NAME)/conffiles
/etc/config/nikola
endef

define Build/Configure
	$(SED) 's/^__version__ =.*/__version__ = "$(PKG_VERSION)"/' $(PKG_BUILD_DIR)/nikola/__init__.py
endef

define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)
	$(CP) $(PKG_INSTALL_DIR)/* $(1)/
	$(INSTALL_DIR) $(1)/etc/config/
	$(INSTALL_CONF) $(PKG_BUILD_DIR)/conf/uci/nikola $(1)/etc/config/nikola
	$(INSTALL_DIR) $(1)/etc/logrotate.d/
	$(INSTALL_CONF) $(PKG_BUILD_DIR)/conf/logrotate.d/iptables $(1)/etc/logrotate.d/iptables
	$(INSTALL_DIR) $(1)/usr/share/nikola/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/conf/cron/nikola.sh $(1)/usr/share/nikola/bin/nikola.sh
	$(INSTALL_DIR) $(1)/etc/cron.d
	$(INSTALL_CONF) $(PKG_BUILD_DIR)/conf/cron/nikola $(1)/etc/cron.d/nikola
	$(INSTALL_DIR) $(1)/etc/syslog-ng.d/
	$(INSTALL_CONF) $(PKG_BUILD_DIR)/conf/syslog/syslog-ng.conf $(1)/etc/syslog-ng.d/nikola.conf
endef

define Package/$(PKG_NAME)/postinst
#!/bin/sh
# check if we are on real system
if [ -z "$${IPKG_INSTROOT}" ]; then
	echo "Reloading syslog-ng3"
	/etc/init.d/syslog-ng reload
fi
exit 0
endef

$(eval $(call BuildPackage,$(PKG_NAME)))
