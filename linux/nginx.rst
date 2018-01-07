Nginx - 高性能 web 服务器
=========================

`Nginx <http://nginx.org/>`_ （发音同engine x）是一款由俄罗斯程序员Igor Sysoev所开发轻量级的网页服务器、反向代理服务器以及电子邮件（IMAP/POP3）代理服务器。整体采用模块化设计是nginx的一个重大特点，甚至http服务器核心功能也是一个模块。要注意的是：nginx的模块是静态的，添加和删除模块都要对nginx进行重新编译，这一点与Apache的动态模块完全不同。

安装
----

编译
^^^^

::

 apt-get install libpcre3-dev zlib1g-dev
 wget http://nginx.org/download/nginx-1.12.2.tar.gz
 tar zxvf nginx-1.12.2.tar.gz
 cd nginx-1.12.2
 useradd nginx
 ./configur
 make
 make install

选项
"""""

运行./configure 时可用的选项

::

 --help                             列出选项列表

::

 --prefix=PATH                      安装路径，默认为 /usr/local/nginx
 --sbin-path=PATH                   二进制可执行文件路径，默认为 <prefix>/sbin/nginx
 --conf-path=PATH                   nginx.conf路径，默认为 <prefix>/conf/nginx.conf
 --error-log-path=PATH              错误日志路径，默认为<prefix>/logs/error.log
 --pid-path=PATH                    nginx.pid 路径，默认为<prefix>/logs/nginx.pid
 --lock-path=PATH                   nginx.lock 路径，默认为<prefix>/logs/nginx.lock

::

 --user=USER                        为 worker processes 设定非特权用户,默认为 nobody
 --group=GROUP                      为 worker processes 设定非特权组,默认为 nobody

::

 --builddir=DIR                     设置构建目录

::

 --with-rtsig_module                enable rtsig module
 --with-select_module               enable select module
 --without-select_module            disable select module
 --with-poll_module                 enable poll module
 --without-poll_module              disable poll module

::

 --with-file-aio                    enable file AIO support
 --with-ipv6                        enable IPv6 support

::

 --with-http_ssl_module             enable ngx_http_ssl_module  需要安装 OpenSSL library  # yum install openssl-devel | # apt-get install libssl-dev
 --with-http_spdy_module            enable ngx_http_spdy_module
 --with-http_realip_module          enable ngx_http_realip_module
 --with-http_addition_module        enable ngx_http_addition_module
 --with-http_xslt_module            enable ngx_http_xslt_module  需要安装 libxml2/libxslt libraries # yum install libxslt-devel | # apt-get install libxslt1-dev
 --with-http_image_filter_module    enable ngx_http_image_filter_module 需要安装 GD library # yum install gd-devel | # apt-get install libgd2-xpm-dev
 --with-http_geoip_module           enable ngx_http_geoip_module 需要安装 GeoIP library # yum install GeoIP-devel | # apt-get install libgeoip-dev
 --with-http_sub_module             enable ngx_http_sub_module
 --with-http_dav_module             enable ngx_http_dav_module
 --with-http_flv_module             enable ngx_http_flv_module
 --with-http_mp4_module             enable ngx_http_mp4_module
 --with-http_gunzip_module          enable ngx_http_gunzip_module
 --with-http_gzip_static_module     enable ngx_http_gzip_static_module
 --with-http_auth_request_module    enable ngx_http_auth_request_module
 --with-http_random_index_module    enable ngx_http_random_index_module
 --with-http_secure_link_module     enable ngx_http_secure_link_module
 --with-http_degradation_module     enable ngx_http_degradation_module
 --with-http_stub_status_module     enable ngx_http_stub_status_module

::

 --without-http_charset_module      disable ngx_http_charset_module
 --without-http_gzip_module         disable ngx_http_gzip_module
 --without-http_ssi_module          disable ngx_http_ssi_module
 --without-http_userid_module       disable ngx_http_userid_module
 --without-http_access_module       disable ngx_http_access_module
 --without-http_auth_basic_module   disable ngx_http_auth_basic_module
 --without-http_autoindex_module    disable ngx_http_autoindex_module
 --without-http_geo_module          disable ngx_http_geo_module
 --without-http_map_module          disable ngx_http_map_module
 --without-http_split_clients_module disable ngx_http_split_clients_module
 --without-http_referer_module      disable ngx_http_referer_module
 --without-http_rewrite_module      disable ngx_http_rewrite_module
 --without-http_proxy_module        disable ngx_http_proxy_module
 --without-http_fastcgi_module      disable ngx_http_fastcgi_module
 --without-http_uwsgi_module        disable ngx_http_uwsgi_module
 --without-http_scgi_module         disable ngx_http_scgi_module
 --without-http_memcached_module    disable ngx_http_memcached_module
 --without-http_limit_conn_module   disable ngx_http_limit_conn_module
 --without-http_limit_req_module    disable ngx_http_limit_req_module
 --without-http_empty_gif_module    disable ngx_http_empty_gif_module
 --without-http_browser_module      disable ngx_http_browser_module
 --without-http_upstream_ip_hash_module
                                    disable ngx_http_upstream_ip_hash_module
 --without-http_upstream_least_conn_module
                                    disable ngx_http_upstream_least_conn_module
 --without-http_upstream_keepalive_module
                                    disable ngx_http_upstream_keepalive_module

::

 --with-http_perl_module            enable ngx_http_perl_module 需要安装 perl # yum install perl-ExtUtils-Embed | # apt-get install libperl-dev
 --with-perl_modules_path=PATH      set Perl modules path
 --with-perl=PATH                   set perl binary pathname

::


 --http-log-path=PATH               set http access log pathname
 --http-client-body-temp-path=PATH  http 客户端请求的临时文件路径，默认为<prefix>/client_body_temp
 --http-proxy-temp-path=PATH        http 代理的临时文件路径，默认为<prefix>/proxy_temp
 --http-fastcgi-temp-path=PATH      http fastcgi 的临时文件路径，默认为<prefix>/fastcgi_temp
 --http-uwsgi-temp-path=PATH        http uwsgi 的临时文件路径
 --http-scgi-temp-path=PATH         http scgi 的临时文件路径

::

 --without-http                     disable HTTP server
 --without-http-cache               disable HTTP cache

::

 --with-mail                        enable POP3/IMAP4/SMTP proxy module
 --with-mail_ssl_module             enable ngx_mail_ssl_module
 --without-mail_pop3_module         disable ngx_mail_pop3_module
 --without-mail_imap_module         disable ngx_mail_imap_module
 --without-mail_smtp_module         disable ngx_mail_smtp_module

::

 --with-google_perftools_module     enable ngx_google_perftools_module 需要安装 Google perftools library # yum install gperftools-devel | # apt-get install libgoogle-perftools-dev
 --with-cpp_test_module             enable ngx_cpp_test_module 需要安装c++ # yum install gcc-c++ | # apt-get install libstdc++6-4.7-dev

::

 --add-module=PATH                  enable an external module

::

 --with-cc=PATH                     C编译器路径
 --with-cpp=PATH                    C预处理器路径
 --with-cc-opt=OPTIONS              额外的C编译器选项
 --with-ld-opt=OPTIONS              额外的连接器选项
 --with-cpu-opt=CPU                 指定编译的CPU, 有效值:
                                    pentium, pentiumpro, pentium3, pentium4,
                                    athlon, opteron, sparc32, sparc64, ppc64

::

 --without-pcre                     disable PCRE library usage
 --with-pcre                        force PCRE library usage
 --with-pcre=DIR                    set path to PCRE library sources
 --with-pcre-opt=OPTIONS            set additional build options for PCRE
 --with-pcre-jit                    build PCRE with JIT compilation support

::

 --with-md5=DIR                     set path to md5 library sources
 --with-md5-opt=OPTIONS             set additional build options for md5
 --with-md5-asm                     use md5 assembler sources

::

 --with-sha1=DIR                    set path to sha1 library sources
 --with-sha1-opt=OPTIONS            set additional build options for sha1
 --with-sha1-asm                    use sha1 assembler sources

::

 --with-zlib=DIR                    set path to zlib library sources
 --with-zlib-opt=OPTIONS            set additional build options for zlib
 --with-zlib-asm=CPU                use zlib assembler sources optimized
                                    for the specified CPU, valid values:
                                    pentium, pentiumpro

::

 --with-libatomic                   force libatomic_ops library usage
 --with-libatomic=DIR               set path to libatomic_ops library sources

::

 --with-openssl=DIR                 set path to OpenSSL library sources
 --with-openssl-opt=OPTIONS         set additional build options for OpenSSL

::

 --with-debug                       启用调试日志

示例
"""""

    依据apt标准编译

    ./configure
    --prefix=/etc/nginx
    --sbin-path=/usr/sbin/nginx
    --modules-path=/usr/lib/nginx/modules
    --conf-path=/etc/nginx/nginx.conf
    --error-log-path=/var/log/nginx/error.log
    --http-log-path=/var/log/nginx/access.log
    --pid-path=/var/run/nginx.pid
    --lock-path=/var/run/nginx.lock
    --http-client-body-temp-path=/var/cache/nginx/client_temp
    --http-proxy-temp-path=/var/cache/nginx/proxy_temp
    --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp
    --http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp
    --http-scgi-temp-path=/var/cache/nginx/scgi_temp
    --user=nginx
    --group=nginx
    --with-compat
    --with-file-aio
    --with-threads
    --with-http_addition_module
    --with-http_auth_request_module
    --with-http_dav_module
    --with-http_flv_module
    --with-http_gunzip_module
    --with-http_gzip_static_module
    --with-http_mp4_module
    --with-http_random_index_module
    --with-http_realip_module
    --with-http_secure_link_module
    --with-http_slice_module
    --with-http_ssl_module
    --with-http_stub_status_module
    --with-http_sub_module
    --with-http_v2_module
    --with-mail
    --with-mail_ssl_module
    --with-stream
    --with-stream_realip_module
    --with-stream_ssl_module
    --with-stream_ssl_preread_module
    --with-cc-opt='-g -O2 -fdebug-prefix-map=/data/builder/debuild/nginx-1.12.2/debian/debuild-base/nginx-1.12.2=. -specs=/usr/share/dpkg/no-pie-compile.specs -fstack-protector-strong -Wformat -Werror=format-security -Wp,-D_FORTIFY_SOURCE=2 -fPIC' --with-ld-opt='-specs=/usr/share/dpkg/no-pie-link.specs -Wl,-z,relro -Wl,-z,now -Wl,--as-needed -pie'


apt
^^^^^^^

::

 wget http://nginx.org/keys/nginx_signing.key
 apt-key add nginx_signing.key

编辑/etc/apt/sources.list，加入以下内容：

::

 deb http://nginx.org/packages/mainline/debian/ codename nginx
 deb-src http://nginx.org/packages/mainline/debian/ codename nginx

根据操作系统的版本，将codename替换成 stretch 或者 jessie

::

 apt-get <apt-get> update
 apt-get <apt-get> install nginx

配置
-----

::

 user  nginx;
 worker_processes  auto;
 error_log  /var/log/nginx/error.log warn;
 pid        /var/run/nginx.pid;
 worker_rlimit_nofile 16384;

event
^^^^^

::

 events {
    worker_connections  2048;
    multi_accept on;
    use epoll;
 }

http
^^^^^

::

 include       /etc/nginx/mime.types;
 default_type  application/octet-stream;
 log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                  '$status $body_bytes_sent "$http_referer" '
                  '"$http_user_agent" "$http_x_forwarded_for"';
 sendfile        on;
 tcp_nopush on;
 keepalive_timeout  65;
 server_tokens off;
 gzip  on;
 gzip_min_length 1000;
 gzip_comp_level 4;
 gzip_types text/plain text/css application/json application/x-javascript text/xml application/xml text/javascript;
 client_max_body_size 20M;
 client_body_buffer_size 1024k;
 include /etc/nginx/conf.d/*.conf;

server
^^^^^^^^

::

 listen	80;
 server_name	localhost;
 access_log  PATH  main;

 #跳转https
 listen	80;
 server_name localhost;
 return 301 https://$host$request_uri;

ssl模块
""""""""

::

    listen       443 ssl http2 default;
    server_name  localhost;
    add_header Strict-Transport-Security "max-age=31536000; includeSubdomains; preload";
    ssl_certificate  <ssl.crt>;
    ssl_certificate_key  <ssl.key>;
    ssl_protocols  TLSv1 TLSv1.2;
    ssl_ciphers 'AES128+EECDH:AES128+EDH:!aNULL';
    ssl_prefer_server_ciphers   on;
    ssl_dhparam /etc/ssl/certs/dhparams.pem;
    ssl_session_cache   shared:ssl:10m;
    ssl_session_timeout  10m;

location
^^^^^^^^

::

 location / {
  root   <PATH>;
  index  index.html index.htm;
     }

