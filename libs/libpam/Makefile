#
# Copyright (C) 2006-2014 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=libpam
PKG_VERSION:=1.2.0
PKG_RELEASE:=3

PKG_SOURCE:=Linux-PAM-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:=http://www.linux-pam.org/library/
PKG_HASH:=cd8beac5961e942e9c73b32a3cd1a3457755f8fb35d07c9ec64511e19e135ea4
PKG_INSTALL:=1
PKG_FIXUP:=autoreconf
PKG_MAINTAINER:=Nikos Mavrogiannopoulos <n.mavrogiannopoulos@gmail.com>

PKG_SOURCE_SUBDIR:=Linux-PAM-$(PKG_VERSION)
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(BUILD_VARIANT)/$(PKG_SOURCE_SUBDIR)

include $(INCLUDE_DIR)/package.mk

define Package/$(PKG_NAME)/Default
  SECTION:=libs
  CATEGORY:=Libraries
  SUBMENU:=Web Servers/Proxies
  TITLE:=the Linux-PAM libraries and modules.
  TITLE+=$(2)
  URL:=http://www.kernel.org/pub/linux/libs/pam
  VARIANT:=$(1)
  DEPENDS:=
  DEPENDS+=$(3)
  MAINTAINER:=Cezary Jackiewicz <cezary@eko.one.pl>
endef

Package/$(PKG_NAME)=$(call Package/$(PKG_NAME)/Default,default,,)
Package/$(PKG_NAME)-db=$(call Package/$(PKG_NAME)/Default,db,(db support),+libdb47)

define Package/$(PKG_NAME)/description
	The Linux-PAM Pluggable Authentication Modules.
endef

define Package/$(PKG_NAME)-db/description
	The Linux-PAM Pluggable Authentication Modules.(db support)
endef

TARGET_CFLAGS += $(FPIC)

ifeq ($(BUILD_VARIANT),db)
 define Build/Configure
	$(call Build/Configure/Default, \
		--enable-shared \
		--enable-static \
		--enable-pamlocking \
		--disable-prelude \
		--disable-lckpwdf \
		--disable-selinux \
		--disable-nls \
		--disable-rpath \
	)
 endef
else
 define Build/Configure
	$(call Build/Configure/Default, \
		--enable-shared \
		--enable-static \
		--enable-pamlocking \
		--disable-prelude \
		--disable-lckpwdf \
		--disable-selinux \
		--disable-nls \
		--disable-rpath \
		--disable-nis \
		--disable-regenerate-docu \
		--enable-db=no \
	)
 endef
endif

define Build/InstallDev
	$(INSTALL_DIR) $(1)/lib
	$(INSTALL_DIR) $(1)/usr/include
	$(CP) $(PKG_INSTALL_DIR)/lib/* $(1)/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/include/* $(1)/usr/include
endef

define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/lib $(1)/lib/security $(1)/lib/security/pam_filter
	$(INSTALL_DIR) $(1)/etc $(1)/etc/pam.d
	$(INSTALL_DIR) $(1)/usr/sbin
	$(CP) $(PKG_INSTALL_DIR)/lib/*.so* $(1)/lib/
	$(CP) $(PKG_INSTALL_DIR)/lib/security/*.so* $(1)/lib/security/
	$(CP) $(PKG_INSTALL_DIR)/lib/security/pam_filter/* $(1)/lib/security/pam_filter/
	$(CP) $(PKG_INSTALL_DIR)/etc/* $(1)/etc/
	$(CP) ./files/* $(1)/etc/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/* $(1)/usr/sbin/
endef

Package/$(PKG_NAME)-db/install=$(Package/$(PKG_NAME)/install)

$(eval $(call BuildPackage,$(PKG_NAME)))
$(eval $(call BuildPackage,$(PKG_NAME)-db))
