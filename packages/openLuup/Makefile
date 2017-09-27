include $(TOPDIR)/rules.mk

PKG_NAME:=openLuup
PKG_VERSION:=17.3.9
PKG_REV:=b108e6d746684f79ec24baf0245ac09f5a889b38
PKG_RELEASE:=1

PKG_SOURCE:=v$(PKG_VERSION).tar.gz
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_URL:=https://github.com/akbooer/openLuup.git
PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=$(PKG_REV)
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk

define Package/openLuup
	SECTION:=hvac
	CATEGORY:=Home automation
	TITLE:=openLuup
	PKGARCH:=all
	MAINTAINER:=sam_
	DEPENDS:=+luafilesystem +luasec +dkjson
endef

define Package/openLuup/description
	openLuup - Run MiOS (Vera) plugins on generic Unix systems
endef

define Build/Configure
endef

define Build/Compile
	mkdir -p $(PKG_BUILD_DIR)/files
	mkdir -p $(PKG_BUILD_DIR)/icons
	sed -re '1,/local index_html = \[\[/ d' -e '/\]\]/,$$$$ d' \
	< $(PKG_BUILD_DIR)/openLuup/virtualfilesystem.lua \
	> $(PKG_BUILD_DIR)/index.html
endef

define Package/openLuup/install
	$(INSTALL_DIR) $(1)/usr/lib/$(PKG_NAME)

	$(CP) $(PKG_BUILD_DIR)/$(PKG_NAME) $(1)/usr/lib/$(PKG_NAME)/
	$(CP) $(PKG_BUILD_DIR)/files $(1)/usr/lib/$(PKG_NAME)/
	$(CP) $(PKG_BUILD_DIR)/icons $(1)/usr/lib/$(PKG_NAME)/
	$(CP) $(PKG_BUILD_DIR)/index.html $(1)/usr/lib/$(PKG_NAME)/

	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) files/$(PKG_NAME).init $(1)/etc/init.d/$(PKG_NAME)
endef

define Package/openLuup/postinst
#!/bin/sh
echo "Installing UI..."
cd $(1)/usr/lib/$(PKG_NAME) && lua $(PKG_NAME)/init.lua altui || true
endef

$(eval $(call BuildPackage,openLuup))
