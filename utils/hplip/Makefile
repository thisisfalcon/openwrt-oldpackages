#
# Copyright (C) 2006-2011 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=hplip
PKG_VERSION:=3.14.10
PKG_RELEASE:=5

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=@SF/hplip
PKG_MD5SUM:=7af44ee1c4312b00d2269c1b25008f73

PKG_BUILD_DEPENDS := python
PKG_FIXUP:=autoreconf

include $(INCLUDE_DIR)/package.mk

define Package/hplip
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=HP Linux Imaging and Printing
  URL:=http://sourceforge.net/projects/hplip/
  DEPENDS+=+libjpeg +libtiff +libusb-compat +cups +sane-libs
endef

define Package/hplip/description
	HPLIP is an HP developed solution for printing, scanning, and faxing with HP inkjet and laser based printers in Linux.
endef

CONFIGURE_ARGS += \
	--disable-gui-build \
	--disable-network-build \
	--disable-fax-build \
	--disable-pp-build \
	--disable-doc-build \
	--disable-foomatic-xml-install \
	--disable-dbus-build

TARGET_CFLAGS += \
    -I$(STAGING_DIR)/usr/include/libusb-1.0

define Build/Configure
	$(call Build/Configure/Default,\
		$(CONFIGURE_ARGS),\
		ac_cv_lib_cups_cupsDoFileRequest=yes \
		LIBS="-lz -ljpeg -lusb -lcups -lpng -ltiff" \
	)
endef

define Package/hplip/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_BUILD_DIR)/.libs/libhpip.so* $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/.libs/libhpmud.so* $(1)/usr/lib/

	$(INSTALL_DIR) $(1)/usr/lib/cups/backend
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/.libs/hp $(1)/usr/lib/cups/backend

	$(INSTALL_DIR) $(1)/usr/lib/sane
	$(CP) $(PKG_BUILD_DIR)/.libs/libsane-hpaio.so* $(1)/usr/lib/sane

	$(INSTALL_DIR) $(1)/etc/hp
	$(CP) $(PKG_BUILD_DIR)/hplip.conf $(1)/etc/hp/hplip.conf

	$(INSTALL_DIR) $(1)/usr/share/hplip/data/models/
	$(CP) $(PKG_BUILD_DIR)/data/models/models.dat $(1)/usr/share/hplip/data/models/

	$(INSTALL_DIR) $(1)/etc/hotplug.d/usb/
	$(INSTALL_BIN) ./files/20-hplip $(1)/etc/hotplug.d/usb/

	$(INSTALL_DIR) $(1)/etc/sane.d/dll.d/
	$(INSTALL_DATA) ./files/hplib.conf $(1)/etc/sane.d/dll.d/hplib
endef

define Package/hplip/conffiles
/etc/hp/hplip.conf
endef

$(eval $(call BuildPackage,hplip))
