#
# Copyright (C) 2014 Arduino LLC
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NPM_NAME:=socket.io
PKG_NAME:=node-$(PKG_NPM_NAME)-legacy
PKG_VERSION:=0.9.17
PKG_RELEASE:=2

PKG_SOURCE:=$(PKG_NPM_NAME)-$(PKG_VERSION).tgz
PKG_SOURCE_URL:=http://registry.npmjs.org/$(PKG_NPM_NAME)/-/
PKG_MD5SUM:=67b3dd56732a4b317c680d094ef48efd

PKG_BUILD_DEPENDS:=node
PKG_NODE_VERSION:=0.10.33

include $(INCLUDE_DIR)/package.mk

define Package/node-socket.io-legacy
  DEPENDS:=+node +node-socket.io-client-legacy
  SUBMENU:=Node.js
  SECTION:=lang
  CATEGORY:=Languages
  TITLE:=Real-time apps made cross-browser & easy with a WebSocket-like API
  URL:=https://www.npmjs.org/package/socket.io
endef

define Package/node-socket.io-legacy/description
 Real-time apps made cross-browser & easy with a WebSocket-like API
endef

define Build/Prepare
	/bin/tar xzf $(DL_DIR)/$(PKG_SOURCE) -C $(PKG_BUILD_DIR) --strip-components 1
	$(Build/Patch)
endef

define Build/Compile
	CC="$(TOOLCHAIN_DIR)/bin/mips-openwrt-linux-gcc" \
	CXX="$(TOOLCHAIN_DIR)/bin/mips-openwrt-linux-g++" \
	AR="$(TOOLCHAIN_DIR)/bin/mips-openwrt-linux-ar" \
	RANLIB="$(TOOLCHAIN_DIR)/bin/mips-openwrt-linux-ranlib" \
	LINK="$(TOOLCHAIN_DIR)/bin/mips-openwrt-linux-g++" \
	LD="$(TOOLCHAIN_DIR)/bin/mips-openwrt-linux-g++" \
	PATH="/usr/bin/:$(PATH)" \
	GYPFLAGS="-Dv8_use_mips_abi_hardfloat=false -Dv8_can_use_fpu_instructions=false" \
	LDFLAGS="-L$(TOOLCHAIN_DIR)/lib/ -Wl,-rpath-link $(TOOLCHAIN_DIR)/lib/" \
	npm_config_arch=mips \
	npm_config_nodedir=$(BUILD_DIR)/node-v$(PKG_NODE_VERSION)/ \
	PREFIX="$(PKG_INSTALL_DIR)/usr/" \
	npm install -g $(PKG_BUILD_DIR)
endef

define Package/node-socket.io-legacy/install
	mkdir -p $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/node_modules $(1)/usr/lib/
	rm -rf $(1)/usr/lib/node_modules/$(PKG_NPM_NAME)/node_modules/socket.io-client
endef

$(eval $(call BuildPackage,node-socket.io-legacy))

