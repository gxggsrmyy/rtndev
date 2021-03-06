#
# Copyright (C) 2011-2017 Entware
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
# Debian differences: threads, geoip, http_upstream_zone, stream_upstream_zone
# http_perl modules turned off

include $(TOPDIR)/rules.mk

PKG_NAME:=nginx-extras
PKG_VERSION:=1.11.9
PKG_RELEASE:=1

PKG_SOURCE:=nginx-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://nginx.org/download
PKG_HASH:=dc22b71f16b551705930544dc042f1ad1af2f9715f565187ec22c7a4b2625748

PKG_BUILD_DIR:=$(BUILD_DIR)/nginx-$(PKG_VERSION)

PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/nginx-extras
	SECTION:=net
	CATEGORY:=Network
	SUBMENU:=Web Servers/Proxies
	TITLE:=Nginx web server
	URL:=http://nginx.org/
	DEPENDS:=+libpam +libexpat +libopenssl +libexslt +libgd +liblua +libpcre
	CONFLICTS:=nginx
endef

define Package/nginx-extras/description
 nginx is an HTTP and reverse proxy server, as well as a mail proxy server,
 written by Igor Sysoev. This is mainline version where all available
 plugins and patches are taken from Debian nginx-extras package.
endef

TARGET_CFLAGS += \
	-I$(STAGING_DIR)/opt/include/libxml2

TARGET_LDFLAGS += \
	-lxml2

define Build/Configure
	( cd $(PKG_BUILD_DIR) ; \
		./configure \
			--crossbuild=Linux::$(ARCH) \
			--with-cc="$(TARGET_CC)" \
			--with-cc-opt="$(TARGET_CPPFLAGS) $(TARGET_CFLAGS)" \
			--with-ld-opt="$(TARGET_LDFLAGS)" \
			--prefix=/opt \
			--conf-path=/opt/etc/nginx/nginx.conf \
			--http-log-path=/opt/var/log/nginx/access.log \
			--error-log-path=/opt/var/log/nginx/error.log \
			--lock-path=/opt/var/lock/nginx.lock \
			--pid-path=/opt/var/run/nginx.pid \
			--modules-path=/opt/lib/nginx \
			--http-client-body-temp-path=/opt/var/lib/nginx/body \
			--http-fastcgi-temp-path=/opt/var/lib/nginx/fastcgi \
			--http-proxy-temp-path=/opt/var/lib/nginx/proxy \
			--http-scgi-temp-path=/opt/var/lib/nginx/scgi \
			--http-uwsgi-temp-path=/opt/var/lib/nginx/uwsgi \
			--with-debug \
			--with-pcre-jit \
			--with-http_ssl_module \
			--with-http_stub_status_module \
			--with-http_realip_module \
			--with-http_auth_request_module \
			--with-http_v2_module \
			--with-http_dav_module \
			--with-http_slice_module \
			\
			--without-http_upstream_zone_module \
			--without-stream_upstream_zone_module \
			\
			--with-http_addition_module \
			--with-http_flv_module \
			--with-http_gunzip_module \
			--with-http_gzip_static_module \
			--with-http_image_filter_module=dynamic \
			--with-http_mp4_module \
			--with-http_random_index_module \
			--with-http_secure_link_module \
			--with-http_sub_module \
			--with-http_xslt_module=dynamic \
			--with-mail=dynamic \
			--with-mail_ssl_module \
			--with-stream=dynamic \
			--with-stream_ssl_module \
			--add-dynamic-module=$(PKG_BUILD_DIR)/modules/headers-more-nginx-module \
			--add-dynamic-module=$(PKG_BUILD_DIR)/modules/nginx-auth-pam \
			--add-dynamic-module=$(PKG_BUILD_DIR)/modules/nginx-cache-purge \
			--add-dynamic-module=$(PKG_BUILD_DIR)/modules/nginx-dav-ext-module \
			--add-dynamic-module=$(PKG_BUILD_DIR)/modules/nginx-development-kit \
			--add-dynamic-module=$(PKG_BUILD_DIR)/modules/nginx-echo \
			--add-dynamic-module=$(PKG_BUILD_DIR)/modules/ngx-fancyindex \
			--add-dynamic-module=$(PKG_BUILD_DIR)/modules/nchan \
			--add-dynamic-module=$(PKG_BUILD_DIR)/modules/nginx-lua \
			--add-dynamic-module=$(PKG_BUILD_DIR)/modules/nginx-upload-progress \
			--add-dynamic-module=$(PKG_BUILD_DIR)/modules/nginx-upstream-fair \
			--add-dynamic-module=$(PKG_BUILD_DIR)/modules/ngx_http_substitutions_filter_module \
	)
endef

define Package/nginx-extras/conffiles
/opt/etc/nginx/
endef

define Package/nginx-extras/install
	$(INSTALL_DIR) $(1)/opt/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/opt/sbin/nginx $(1)/opt/sbin
	$(INSTALL_DIR) $(1)/opt/lib/nginx
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/opt/lib/nginx/ngx_*.so $(1)/opt/lib/nginx
	$(INSTALL_DIR) $(1)/opt/html
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/opt/html/*.html $(1)/opt/html
	$(INSTALL_DIR) $(1)/opt/etc/nginx/{sites-available,sites-enabled,snippets}
	$(INSTALL_CONF) $(PKG_INSTALL_DIR)/opt/etc/nginx/* $(1)/opt/etc/nginx
	$(RM) $(1)/opt/etc/nginx/*.default $(1)/opt/etc/nginx/nginx.conf
	$(INSTALL_CONF) files/default $(1)/opt/etc/nginx/sites-available
	$(INSTALL_CONF) files/{fastcgi-php,snakeoil}.conf $(1)/opt/etc/nginx/snippets
	$(INSTALL_CONF) files/nginx.conf $(1)/opt/etc/nginx
	$(INSTALL_DIR) $(1)/opt/etc/init.d
	$(INSTALL_BIN) files/S80nginx $(1)/opt/etc/init.d
	$(INSTALL_DIR) $(1)/opt/var/log/nginx $(1)/opt/share/www \
		$(1)/opt/var/lib/nginx/{body,proxy,fastcgi}
endef

define Package/nginx-extras/postinst
#!/bin/sh

if [ -e /opt/etc/nginx/sites-enabled/default ]; then
    echo '/opt/etc/nginx/sites-enabled/default exists, not linking default one.'
else
    ln -s /opt/etc/nginx/sites-available/default /opt/etc/nginx/sites-enabled
    echo '/opt/etc/nginx/sites-enabled/default site enabled.'
fi
exit 0
endef

$(eval $(call BuildPackage,nginx-extras))
