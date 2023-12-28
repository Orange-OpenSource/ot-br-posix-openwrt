#
#  Copyright (c) 2020, The OpenThread Authors.
#  All rights reserved.
#
#  Redistribution and use in source and binary forms, with or without
#  modification, are permitted provided that the following conditions are met:
#  1. Redistributions of source code must retain the above copyright
#     notice, this list of conditions and the following disclaimer.
#  2. Redistributions in binary form must reproduce the above copyright
#     notice, this list of conditions and the following disclaimer in the
#     documentation and/or other materials provided with the distribution.
#  3. Neither the name of the copyright holder nor the
#     names of its contributors may be used to endorse or promote products
#     derived from this software without specific prior written permission.
#
#  THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
#  AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
#  IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE
#  ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE
#  LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR
#  CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF
#  SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS
#  INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN
#  CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE)
#  ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE
#  POSSIBILITY OF SUCH DAMAGE.
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
PKG_HASH:=

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

	$(INSTALL_DIR) $(1)/usr/lib/lua/luci/controller/admin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/openwrt/controller/thread.lua $(1)/usr/lib/lua/luci/controller/admin

	$(INSTALL_DIR) $(1)/usr/lib/lua/luci/view
	$(CP) $(PKG_BUILD_DIR)/src/openwrt/view/admin_thread $(1)/usr/lib/lua/luci/view

	$(INSTALL_DIR) $(1)/etc/dbus-1/system.d/
	$(INSTALL_CONF) ./files/system.conf $(1)/etc/dbus-1/system.d/system.conf

endef

$(eval $(call BuildPackage,openthread-br))
