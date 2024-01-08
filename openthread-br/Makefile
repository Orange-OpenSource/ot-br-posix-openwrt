#
# Software Name: ot-br-posix-openwrt
# SPDX-FileCopyrightText: Copyright (c) Orange SA
# SPDX-License-Identifier: BSD-3-Clause
#
# This software is distributed under the BSD 3-Clause "New" or "Revised" License,
# the text of which is available at https://spdx.org/licenses/BSD-3-Clause.html
# or see the "LICENSE" file for more details.
#
# Authors: Maximilien BAUMANN <maximilien.baumann@orange.com>
# Software description: Project to allow adding an OpenWRT feed for OTBR
#
include $(TOPDIR)/rules.mk

PKG_NAME:=openthread-br
PKG_LICENSE:=BSD-3-Clause
PKG_LICENSE_FILES:=LICENSE
PKG_RELEASE:=1
PKG_VERSION:=1.2
PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=98db630
PKG_SOURCE_URL:=https://github.com/openthread/ot-br-posix.git
PKG_MIRROR_HASH:=72ad1d42cc01f9c3384d5fd8a66312fb23f1df40f2fd3da232c55e1f0c7be41b

PKG_BUILD_DEPENDS:=node/host protobuf/host

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

CMAKE_OPTIONS+= \
	-DBUILD_TESTING=OFF \
	-DCMAKE_INSTALL_PREFIX=/usr \
	-DOTBR_BORDER_AGENT=ON \
	-DOTBR_BORDER_ROUTING=ON \
	-DOTBR_INFRA_IF_NAME="br-lan" \
	-DOTBR_MDNS="avahi" \
	-DOTBR_OPENWRT=ON \
	-DOTBR_SRP_ADVERTISING_PROXY=ON \
	-DOTBR_DNSSD_DISCOVERY_PROXY=ON \
	-DOT_FIREWALL=ON \
	-DOT_POSIX_SETTINGS_PATH=\"/etc/openthread\" \
	-DOT_READLINE=OFF \
	-DOTBR_REST=ON \
	-DOTBR_WEB=OFF \
	-DOTBR_DBUS=ON \
	-DOT_THREAD_VERSION=1.3 \
	-DOTBR_BACKBONE_ROUTER=ON \
	-DOT_BACKBONE_ROUTER_MULTICAST_ROUTING=ON \
	-DOT_DIAGNOSTIC=ON \
	-DOTBR_DUA_ROUTING=ON \
	-DOT_DUA=ON \
	-DOT_MLR=ON \
	-DOT_SRP_CLIENT=ON \
	-DOT_DNS_CLIENT=ON \
	-DOTBR_TREL=OFF \
	-DOTBR_NAT64=OFF \
	-DOTBR_DHCP6_PD=ON \
	-DOTBR_REFERENCE_DEVICE=1 \
	-DOTBR_NETWORK_MANAGER=0 \
	-DOTBR_VENDOR_NAME=$(CONFIG_PRPL_OTBR_VENDOR_NAME) \
	-DPROTOBUF_PROTOC_EXECUTABLE=$(STAGING_DIR_HOSTPKG)/bin/protoc

TARGET_CFLAGS += -DOPENTHREAD_POSIX_CONFIG_DAEMON_SOCKET_BASENAME=\\\"/var/run/openthread-%s\\\"
TARGET_CFLAGS += -lboost_filesystem

define Package/$(PKG_NAME)
	SECTION:=base
	CATEGORY:=Network
	TITLE:=OpenThread Border Router
	DEPENDS:=+libstdcpp +libjson-c +libubus \
			+libblobmsg-json +libavahi-client +avahi-daemon \
			+iptables-mod-extra +ipset +jsoncpp +boost +boost-filesystem \
			+protobuf-lite +libnetfilter-queue
endef

define Package/$(PKG_NAME)/description
    A Thread border router for POSIX-based platforms.
endef

define Package/$(PKG_NAME)/config
	source "$(SOURCE)/Config.in"
endef


define Package/openthread-br/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/otbr-agent $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/ot-ctl $(1)/usr/sbin

	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/etc/init.d/* $(1)/etc/init.d

	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_CONF) $(PKG_INSTALL_DIR)/etc/config/* $(1)/etc/config

	$(INSTALL_DIR) $(1)/etc/dbus-1/system.d
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/etc/dbus-1/system.d/* $(1)/etc/dbus-1/system.d

	$(INSTALL_DIR) $(1)/usr/lib/lua/luci/controller/admin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/openwrt/controller/thread.lua $(1)/usr/lib/lua/luci/controller/admin

	$(INSTALL_DIR) $(1)/usr/lib/lua/luci/view
	$(CP) $(PKG_BUILD_DIR)/src/openwrt/view/admin_thread $(1)/usr/lib/lua/luci/view

endef

$(eval $(call BuildPackage,openthread-br))
