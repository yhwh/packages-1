#
# Copyright (C) 2010-2014 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=minidlna
PKG_VERSION:=1.1.6+git-8a996b
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL=http://git.code.sf.net/p/minidlna/git
PKG_SOURCE_SUBDIR=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=8a996b4b624ef45538a5de10730b8e94c55e7768
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.xz
PKG_MIRROR_HASH:=fddbb7c96b40e335a251b919ff5c674519f266a59b8a43011761ba952c0df1b6

PKG_LICENSE:=GPL-2.0 BSD-3-Clause
PKG_LICENSE_FILES:=COPYING LICENCE.miniupnpd

PKG_MAINTAINER:=Knyazkov Dmitry <medavedik@gmail.com>

PKG_FIXUP:=autoreconf

PKG_BUILD_PARALLEL:=0
PKG_BUILD_DEPENDS:=util-linux

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/kernel.mk
include $(INCLUDE_DIR)/nls.mk

define Package/minidlna
  SECTION:=multimedia
  CATEGORY:=Multimedia
  TITLE:=UPnP A/V & DLNA Media Server
  URL:=http://minidlna.sourceforge.net/
  DEPENDS:= +libpthread +libexif +libjpeg +libsqlite3 +libffmpeg-full \
  	+libid3tag +libflac +libvorbis +libuuid \
  	$(ICONV_DEPENDS) $(INTL_DEPENDS)
endef

define Package/minidlna/description
  MiniDLNA (aka ReadyDLNA) is server software with the aim of
  being fully compliant with DLNA/UPnP-AV clients.
endef

define Package/minidlna/conffiles
/etc/config/minidlna
endef

TARGET_CPPFLAGS += \
	-I$(ICONV_PREFIX)/include \
	-I$(INTL_PREFIX)/include \
	-D_GNU_SOURCE -D_FILE_OFFSET_BITS=64 \

TARGET_LDFLAGS += \
	-L$(ICONV_PREFIX)/lib \
	-L$(INTL_PREFIX)/lib \
	-Wl,-rpath-link=$(STAGING_DIR)/usr/lib \

MAKE_FLAGS +=\
	CFLAGS="$(TARGET_CFLAGS) $(TARGET_CPPFLAGS)" \
	LDFLAGS="$(TARGET_LDFLAGS)" \
	ICONV_LIBS="-liconv $(if $(INTL_FULL),-lintl)" \

CONFIGURE_ARGS +=\
	--disable-rpath \
	--with-libiconv-prefix="$(ICONV_PREFIX)" \
	--with-libintl-prefix="$(INTL_PREFIX)" \
	--with-os-name="OpenWrt Linux" \
	--with-os-version="$(LINUX_VERSION)" \
	--with-os-url="http://openwrt.org/" \
	--with-db-path="/var/run/minidlna" \
	--with-log-path="/var/log"

define Package/minidlna/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/minidlnad $(1)/usr/bin/minidlna
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/minidlna.init $(1)/etc/init.d/minidlna
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_CONF) ./files/minidlna.config $(1)/etc/config/minidlna
endef

$(eval $(call BuildPackage,minidlna))
