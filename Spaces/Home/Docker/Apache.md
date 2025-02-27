



`./apache/htdocs` 디렉토리는 Apache HTTP Server 컨테이너의 정적 웹 콘텐츠를 제공하기 위한 디렉토리입니다. 따라서, 해당 디렉토리에는 웹 서버를 통해 접근 가능한 파일들을 위치시켜야 합니다.

예를 들어, `index.html`이라는 파일을 `./apache/htdocs` 디렉토리에 추가하고 싶다면, 호스트의 `./apache/htdocs` 디렉토리에 `index.html` 파일을 생성하면 됩니다. Apache HTTP Server 컨테이너가 실행되면 해당 파일이 웹 서버를 통해 접근 가능해집니다.

만약 `./apache/htdocs` 디렉토리에 여러 파일을 추가하고 싶다면, 해당 디렉토리에 원하는 파일들을 생성하거나 복사하면 됩니다. 이렇게 함으로써 Apache HTTP Server 컨테이너는 해당 파일들을 제공할 수 있게 됩니다.

예를 들어, `index.html`, `styles.css`, `script.js`라는 파일을 `./apache/htdocs` 디렉토리에 추가하고 싶다면, 호스트의 `./apache/htdocs` 디렉토리에 해당 파일들을 생성하거나 복사하면 됩니다. 이후 Apache HTTP Server 컨테이너가 실행되면 이러한 파일들이 웹 서버를 통해 접근 가능해집니다.

따라서, `./apache/htdocs` 디렉토리에는 웹 서버를 통해 접근 가능한 파일들을 위치시키면 됩니다.

```
version: '3'
services:
  tomcat:
    image: tomcat:9.0.84
    container_name: myTomcat
    ports:
      - 8080:8080
    volumes:
      - ./CrawlerTest-1.0-SNAPSHOT.war:/usr/local/tomcat/webapps/ROOT.war
    # 톰캣 설정 등 추가 설정

  mysql:
    image: mysql:latest
    container_name: mySqls
    ports:
      - 3306:3306
    environment:
      - MYSQL_ROOT_PASSWORD=1234
      - MYSQL_DATABASE=test
      - MYSQL_USER=test
      - MYSQL_PASSWORD=1234
      - MYSQL_SSL_MODE=DISABLED
      - MYSQL_ALLOW_CLEARTEXT_PLUGIN=1
      - MYSQL_SSL=false
    # MySQL 설정 등 추가 설정

  apache:
    image: httpd:latest
    container_name: myApache
    ports:
      - 80:80
    volumes:
      - ./apache/httpd.conf:/usr/local/apache2/conf/httpd.conf
    #  - ./apache/workers.properties:/usr/local/apache2/conf/workers.properties
      - ./apache/htdocs:/usr/local/apache2/htdocs # war파일을 톰캣에 배포한 경우 필요 x
    depends_on:
      - tomcat


```






### httpd.conf
`httpd.conf` 파일은 Apache HTTP Server (아파치 웹 서버)의 주요 구성 파일입니다. 이 파일은 웹 서버의 동작 방식, 가상 호스트 설정, 모듈 활성화, 액세스 제어 등 다양한 기능을 구성하는 데 사용됩니다.

`httpd.conf` 파일은 아파치 웹 서버의 설정을 지정하는 역할을 합니다. 몇 가지 중요한 설정을 예로 들어보겠습니다:

1. 가상 호스트 설정: `httpd.conf` 파일을 통해 가상 호스트를 구성할 수 있습니다. 가상 호스트는 동일한 서버에서 여러 개의 웹 사이트를 호스팅하는 방법을 제공합니다. 각 가상 호스트는 별도의 설정을 가지며, `httpd.conf` 파일에서 해당 가상 호스트에 대한 설정을 지정할 수 있습니다.
    
2. 모듈 활성화: 아파치 웹 서버는 다양한 모듈을 제공하여 추가 기능을 확장할 수 있습니다. `httpd.conf` 파일에서는 필요한 모듈을 활성화하고 설정할 수 있습니다. 예를 들어, SSL 모듈을 활성화하여 HTTPS 연결을 지원하거나, PHP 모듈을 활성화하여 PHP 스크립트 실행을 지원할 수 있습니다.
    
3. 액세스 제어: `httpd.conf` 파일을 사용하여 웹 서버의 액세스 제어를 구성할 수 있습니다. 예를 들어, 특정 IP 주소나 디렉토리에 대한 액세스를 허용하거나 거부할 수 있습니다. 또한, 인증과 권한 설정을 통해 접근 제한을 구성할 수도 있습니다.
    

이 외에도 `httpd.conf` 파일을 통해 로깅 설정, MIME 타입 지정, 리디렉션 설정 등 다양한 웹 서버의 동작을 제어하는 옵션들을 설정할 수 있습니다. 따라서 `httpd.conf` 파일은 아파치 웹 서버의 동작을 세부적으로 구성하기 위해 필요한 중요한 파일입니다.




```
<VirtualHost *:80>
    ServerName www.example1.com
    DocumentRoot /var/www/example1
    <Directory /var/www/example1>
        Options Indexes FollowSymLinks
        AllowOverride None
        Require all granted
    </Directory>
</VirtualHost>

<VirtualHost *:80>
    ServerName www.example2.com
    DocumentRoot /var/www/example2
    <Directory /var/www/example2>
        Options Indexes FollowSymLinks
        AllowOverride None
        Require all granted
    </Directory>
</VirtualHost>

```

- `DocumentRoot`: `DocumentRoot`는 웹 서버가 웹 사이트의 파일을 찾는 기본 디렉토리를 지정합니다. 이 설정이 없으면 웹 서버는 어느 디렉토리에서 파일을 찾아야 하는지 알 수 없으므로, 클라이언트 요청에 대한 적절한 응답을 제공할 수 없습니다. 따라서, 각 가상 호스트에는 적절한 `DocumentRoot`가 설정되어야 합니다.
    
- `<Directory>`: `<Directory>`는 특정 디렉토리에 대한 추가적인 설정을 지정하는 블록입니다. 이 블록을 사용하여 디렉토리에 대한 액세스 제어, 보안 설정, 디렉토리 내 파일에 대한 옵션 등을 구성할 수 있습니다. `<Directory>` 블록이 없으면 디렉토리에 대한 추가적인 설정이 적용되지 않으며, 기본 설정이 적용될 수 있습니다.

`<VirtualHost>` 블록 내에서 `ProxyPass`와 `ProxyPassReverse`를 사용하여 프록시 설정을 한 경우에는 `DocumentRoot`와 `<Directory>` 설정이 없어도 웹 서버가 동작할 수 있습니다.

`ProxyPass`와 `ProxyPassReverse`는 아파치 웹 서버에서 프록시 기능을 구현하기 위해 사용되는 지시자입니다. 이 설정을 사용하면 웹 서버는 클라이언트의 요청을 받아서 지정된 프록시 서버로 전달하고, 프록시 서버로부터 받은 응답을 클라이언트에게 전달합니다.

위의 예시에서는 `/` 경로로 들어오는 모든 요청을 `http://tomcat:8080/`로 프록시하도록 설정되어 있습니다. 즉, 클라이언트의 요청은 아파치 웹 서버를 통해 톰캣 서버로 전달되고, 톰캣 서버에서 처리된 응답은 다시 아파치 웹 서버를 통해 클라이언트에게 전달됩니다.

이 경우에는 `DocumentRoot`와 `<Directory>` 설정이 필요하지 않습니다. 프록시 설정을 통해 아파치 웹 서버는 클라이언트의 요청을 프록시 서버로 전달하므로, 웹 서버 자체에서 파일을 찾을 필요가 없기 때문입니다.

따라서, 프록시 설정을 사용하면 `DocumentRoot`와 `<Directory>` 설정 없이도 웹 서버가 동작할 수 있습니다. 하지만 일반적인 웹 사이트 호스팅을 위해서는 `DocumentRoot`와 `<Directory>` 설정을 적절히 구성해야 합니다.

> 프록시란?
> 프록시는 클라이언트와 서버 사이에서 중개자 역할을 수행하는 컴퓨터 시스템 또는 애플리케이션입니다. 클라이언트는 프록시 서버에 요청을 보내고, 프록시 서버는 해당 요청을 대신하여 서버에 전달하고, 서버로부터 받은 응답을 클라이언트에게 다시 전송합니다.
프록시를 사용하는 이유는 다양합니다. 주요한 목적은 보안과 성능 향상입니다. 몇 가지 주요한 사용 사례는 다음과 같습니다.
> 1. 캐싱: 프록시 서버는 이전에 서버로부터 받은 응답을 캐시하여, 동일한 요청이 들어왔을 때 서버에 요청하지 않고 캐시된 응답을 제공할 수 있습니다. 이로써 응답 시간을 단축시키고 네트워크 대역폭을 절약할 수 있습니다. 
>2. 보안: 프록시 서버는 클라이언트와 서버 사이에서 중개자로서 동작하므로, 클라이언트와 서버 간의 직접적인 통신을 차단하고 보안을 강화할 수 있습니다. 프록시 서버는 클라이언트의 요청을 검사하고, 필터링하거나 보완 조치를 취할 수 있습니다.  
>3. 로드 밸런싱: 프록시 서버는 여러 개의 백엔드 서버에 요청을 분산시키는 로드 밸런싱 기능을 수행할 수 있습니다. 이를 통해 트래픽을 균형적으로 분산시켜 서버의 성능을 향상시킬 수 있습니다.
>4. 익명성 보장: 프록시 서버를 통해 인터넷에 접속할 경우, 클라이언트의 IP 주소를 감추고 익명성을 보장할 수 있습니다. 이를 통해 개인 정보 보호 및 온라인 익명성을 유지할 수 있습니다.
프록시는 다양한 용도로 사용되며, 웹 서버에서도 프록시 설정을 통해 클라이언트의 요청을 다른 서버로 전달하는 기능을 구현할 수 있습니다.



```



ServerRoot "/usr/local/apache2"
Listen 80

LoadModule mpm_event_module modules/mod_mpm_event.so
LoadModule authn_file_module modules/mod_authn_file.so
LoadModule authn_core_module modules/mod_authn_core.so
LoadModule authz_host_module modules/mod_authz_host.so
LoadModule authz_groupfile_module modules/mod_authz_groupfile.so
LoadModule authz_user_module modules/mod_authz_user.so
LoadModule authz_core_module modules/mod_authz_core.so
LoadModule access_compat_module modules/mod_access_compat.so
LoadModule auth_basic_module modules/mod_auth_basic.so
LoadModule reqtimeout_module modules/mod_reqtimeout.so
LoadModule filter_module modules/mod_filter.so
LoadModule mime_module modules/mod_mime.so
LoadModule env_module modules/mod_env.so
LoadModule headers_module modules/mod_headers.so
LoadModule setenvif_module modules/mod_setenvif.so
LoadModule version_module modules/mod_version.so
LoadModule unixd_module modules/mod_unixd.so
LoadModule status_module modules/mod_status.so
LoadModule autoindex_module modules/mod_autoindex.so


<Files ".ht*">
    Require all denied
</Files>


<Directory "/usr/local/apache2/cgi-bin">
    AllowOverride None
    Options None
    Require all granted
</Directory>


<IfModule proxy_html_module>
Include conf/extra/proxy-html.conf
</IfModule>

<IfModule ssl_module>
SSLRandomSeed startup builtin
SSLRandomSeed connect builtin
</IfModule>

LoadModule proxy_module modules/mod_proxy.so
LoadModule proxy_http_module modules/mod_proxy_http.so

<VirtualHost *:80>
    ServerName localhost
    ProxyPass / http://tomcat:8080/ retry=1 acquire=3000 timeout=600
    ProxyPassReverse / http://tomcat:8080/
</VirtualHost>
```


localhost의 기본 home도 설정하고 싶을 때 DocumentRoot /usr/local/apache2/htdocs 추가
retry=1 acquire=3000 timeout=600 넣는 이유 : 연결 시간이 초과되면 프록시 에러가 뜸 프록시 에러를 잡기 위해


```



ServerRoot "/usr/local/apache2"
Listen 80

LoadModule mpm_event_module modules/mod_mpm_event.so
LoadModule authn_file_module modules/mod_authn_file.so
LoadModule authn_core_module modules/mod_authn_core.so
LoadModule authz_host_module modules/mod_authz_host.so
LoadModule authz_groupfile_module modules/mod_authz_groupfile.so
LoadModule authz_user_module modules/mod_authz_user.so
LoadModule authz_core_module modules/mod_authz_core.so
LoadModule access_compat_module modules/mod_access_compat.so
LoadModule auth_basic_module modules/mod_auth_basic.so
LoadModule reqtimeout_module modules/mod_reqtimeout.so
LoadModule filter_module modules/mod_filter.so
LoadModule mime_module modules/mod_mime.so
LoadModule env_module modules/mod_env.so
LoadModule headers_module modules/mod_headers.so
LoadModule setenvif_module modules/mod_setenvif.so
LoadModule version_module modules/mod_version.so
LoadModule unixd_module modules/mod_unixd.so
LoadModule status_module modules/mod_status.so
LoadModule autoindex_module modules/mod_autoindex.so


<Files ".ht*">
    Require all denied
</Files>


<Directory "/usr/local/apache2/cgi-bin">
    AllowOverride None
    Options None
    Require all granted
</Directory>


<IfModule proxy_html_module>
Include conf/extra/proxy-html.conf
</IfModule>

<IfModule ssl_module>
SSLRandomSeed startup builtin
SSLRandomSeed connect builtin
</IfModule>

LoadModule proxy_module modules/mod_proxy.so
LoadModule proxy_http_module modules/mod_proxy_http.so

<VirtualHost *:80>
    DocumentRoot /usr/local/apache2/htdocs
    ProxyPass / http://tomcat:8080/ retry=1 acquire=3000 timeout=600
    ProxyPassReverse / http://tomcat:8080/
</VirtualHost>
```


### 전체 httpd.conf 코드

```
#
# This is the main Apache HTTP server configuration file.  It contains the
# configuration directives that give the server its instructions.
# See <URL:http://httpd.apache.org/docs/2.4/> for detailed information.
# In particular, see 
# <URL:http://httpd.apache.org/docs/2.4/mod/directives.html>
# for a discussion of each configuration directive.
#
# Do NOT simply read the instructions in here without understanding
# what they do.  They're here only as hints or reminders.  If you are unsure
# consult the online docs. You have been warned.  
#
# Configuration and logfile names: If the filenames you specify for many
# of the server's control files begin with "/" (or "drive:/" for Win32), the
# server will use that explicit path.  If the filenames do *not* begin
# with "/", the value of ServerRoot is prepended -- so "logs/access_log"
# with ServerRoot set to "/usr/local/apache2" will be interpreted by the
# server as "/usr/local/apache2/logs/access_log", whereas "/logs/access_log" 
# will be interpreted as '/logs/access_log'.

#
# ServerRoot: The top of the directory tree under which the server's
# configuration, error, and log files are kept.
#
# Do not add a slash at the end of the directory path.  If you point
# ServerRoot at a non-local disk, be sure to specify a local disk on the
# Mutex directive, if file-based mutexes are used.  If you wish to share the
# same ServerRoot for multiple httpd daemons, you will need to change at
# least PidFile.
#
ServerRoot "/usr/local/apache2"

#
# Mutex: Allows you to set the mutex mechanism and mutex file directory
# for individual mutexes, or change the global defaults
#
# Uncomment and change the directory if mutexes are file-based and the default
# mutex file directory is not on a local disk or is not appropriate for some
# other reason.
#
# Mutex default:logs

#
# Listen: Allows you to bind Apache to specific IP addresses and/or
# ports, instead of the default. See also the <VirtualHost>
# directive.
#
# Change this to Listen on specific IP addresses as shown below to 
# prevent Apache from glomming onto all bound IP addresses.
#
#Listen 12.34.56.78:80
Listen 80

#
# Dynamic Shared Object (DSO) Support
#
# To be able to use the functionality of a module which was built as a DSO you
# have to place corresponding `LoadModule' lines at this location so the
# directives contained in it are actually available _before_ they are used.
# Statically compiled modules (those listed by `httpd -l') do not need
# to be loaded here.
#
# Example:
# LoadModule foo_module modules/mod_foo.so
#
LoadModule mpm_event_module modules/mod_mpm_event.so
#LoadModule mpm_prefork_module modules/mod_mpm_prefork.so
#LoadModule mpm_worker_module modules/mod_mpm_worker.so
LoadModule authn_file_module modules/mod_authn_file.so
#LoadModule authn_dbm_module modules/mod_authn_dbm.so
#LoadModule authn_anon_module modules/mod_authn_anon.so
#LoadModule authn_dbd_module modules/mod_authn_dbd.so
#LoadModule authn_socache_module modules/mod_authn_socache.so
LoadModule authn_core_module modules/mod_authn_core.so
LoadModule authz_host_module modules/mod_authz_host.so
LoadModule authz_groupfile_module modules/mod_authz_groupfile.so
LoadModule authz_user_module modules/mod_authz_user.so
#LoadModule authz_dbm_module modules/mod_authz_dbm.so
#LoadModule authz_owner_module modules/mod_authz_owner.so
#LoadModule authz_dbd_module modules/mod_authz_dbd.so
LoadModule authz_core_module modules/mod_authz_core.so
#LoadModule authnz_ldap_module modules/mod_authnz_ldap.so
#LoadModule authnz_fcgi_module modules/mod_authnz_fcgi.so
LoadModule access_compat_module modules/mod_access_compat.so
LoadModule auth_basic_module modules/mod_auth_basic.so
#LoadModule auth_form_module modules/mod_auth_form.so
#LoadModule auth_digest_module modules/mod_auth_digest.so
#LoadModule allowmethods_module modules/mod_allowmethods.so
#LoadModule isapi_module modules/mod_isapi.so
#LoadModule file_cache_module modules/mod_file_cache.so
#LoadModule cache_module modules/mod_cache.so
#LoadModule cache_disk_module modules/mod_cache_disk.so
#LoadModule cache_socache_module modules/mod_cache_socache.so
#LoadModule socache_shmcb_module modules/mod_socache_shmcb.so
#LoadModule socache_dbm_module modules/mod_socache_dbm.so
#LoadModule socache_memcache_module modules/mod_socache_memcache.so
#LoadModule socache_redis_module modules/mod_socache_redis.so
#LoadModule watchdog_module modules/mod_watchdog.so
#LoadModule macro_module modules/mod_macro.so
#LoadModule dbd_module modules/mod_dbd.so
#LoadModule bucketeer_module modules/mod_bucketeer.so
#LoadModule dumpio_module modules/mod_dumpio.so
#LoadModule echo_module modules/mod_echo.so
#LoadModule example_hooks_module modules/mod_example_hooks.so
#LoadModule case_filter_module modules/mod_case_filter.so
#LoadModule case_filter_in_module modules/mod_case_filter_in.so
#LoadModule example_ipc_module modules/mod_example_ipc.so
#LoadModule buffer_module modules/mod_buffer.so
#LoadModule data_module modules/mod_data.so
#LoadModule ratelimit_module modules/mod_ratelimit.so
LoadModule reqtimeout_module modules/mod_reqtimeout.so
#LoadModule ext_filter_module modules/mod_ext_filter.so
#LoadModule request_module modules/mod_request.so
#LoadModule include_module modules/mod_include.so
LoadModule filter_module modules/mod_filter.so
#LoadModule reflector_module modules/mod_reflector.so
#LoadModule substitute_module modules/mod_substitute.so
#LoadModule sed_module modules/mod_sed.so
#LoadModule charset_lite_module modules/mod_charset_lite.so
#LoadModule deflate_module modules/mod_deflate.so
#LoadModule xml2enc_module modules/mod_xml2enc.so
#LoadModule proxy_html_module modules/mod_proxy_html.so
#LoadModule brotli_module modules/mod_brotli.so
LoadModule mime_module modules/mod_mime.so
#LoadModule ldap_module modules/mod_ldap.so
LoadModule log_config_module modules/mod_log_config.so
#LoadModule log_debug_module modules/mod_log_debug.so
#LoadModule log_forensic_module modules/mod_log_forensic.so
#LoadModule logio_module modules/mod_logio.so
#LoadModule lua_module modules/mod_lua.so
LoadModule env_module modules/mod_env.so
#LoadModule mime_magic_module modules/mod_mime_magic.so
#LoadModule cern_meta_module modules/mod_cern_meta.so
#LoadModule expires_module modules/mod_expires.so
LoadModule headers_module modules/mod_headers.so
#LoadModule ident_module modules/mod_ident.so
#LoadModule usertrack_module modules/mod_usertrack.so
#LoadModule unique_id_module modules/mod_unique_id.so
LoadModule setenvif_module modules/mod_setenvif.so
LoadModule version_module modules/mod_version.so
#LoadModule remoteip_module modules/mod_remoteip.so
#LoadModule proxy_module modules/mod_proxy.so
#LoadModule proxy_connect_module modules/mod_proxy_connect.so
#LoadModule proxy_ftp_module modules/mod_proxy_ftp.so
#LoadModule proxy_http_module modules/mod_proxy_http.so
#LoadModule proxy_fcgi_module modules/mod_proxy_fcgi.so
#LoadModule proxy_scgi_module modules/mod_proxy_scgi.so
#LoadModule proxy_uwsgi_module modules/mod_proxy_uwsgi.so
#LoadModule proxy_fdpass_module modules/mod_proxy_fdpass.so
#LoadModule proxy_wstunnel_module modules/mod_proxy_wstunnel.so
#LoadModule proxy_ajp_module modules/mod_proxy_ajp.so
#LoadModule proxy_balancer_module modules/mod_proxy_balancer.so
#LoadModule proxy_express_module modules/mod_proxy_express.so
#LoadModule proxy_hcheck_module modules/mod_proxy_hcheck.so
#LoadModule session_module modules/mod_session.so
#LoadModule session_cookie_module modules/mod_session_cookie.so
#LoadModule session_crypto_module modules/mod_session_crypto.so
#LoadModule session_dbd_module modules/mod_session_dbd.so
#LoadModule slotmem_shm_module modules/mod_slotmem_shm.so
#LoadModule slotmem_plain_module modules/mod_slotmem_plain.so
#LoadModule ssl_module modules/mod_ssl.so
#LoadModule optional_hook_export_module modules/mod_optional_hook_export.so
#LoadModule optional_hook_import_module modules/mod_optional_hook_import.so
#LoadModule optional_fn_import_module modules/mod_optional_fn_import.so
#LoadModule optional_fn_export_module modules/mod_optional_fn_export.so
#LoadModule dialup_module modules/mod_dialup.so
#LoadModule http2_module modules/mod_http2.so
#LoadModule proxy_http2_module modules/mod_proxy_http2.so
#LoadModule md_module modules/mod_md.so
#LoadModule lbmethod_byrequests_module modules/mod_lbmethod_byrequests.so
#LoadModule lbmethod_bytraffic_module modules/mod_lbmethod_bytraffic.so
#LoadModule lbmethod_bybusyness_module modules/mod_lbmethod_bybusyness.so
#LoadModule lbmethod_heartbeat_module modules/mod_lbmethod_heartbeat.so
LoadModule unixd_module modules/mod_unixd.so
#LoadModule heartbeat_module modules/mod_heartbeat.so
#LoadModule heartmonitor_module modules/mod_heartmonitor.so
#LoadModule dav_module modules/mod_dav.so
LoadModule status_module modules/mod_status.so
LoadModule autoindex_module modules/mod_autoindex.so
#LoadModule asis_module modules/mod_asis.so
#LoadModule info_module modules/mod_info.so
#LoadModule suexec_module modules/mod_suexec.so
<IfModule !mpm_prefork_module>
	#LoadModule cgid_module modules/mod_cgid.so
</IfModule>
<IfModule mpm_prefork_module>
	#LoadModule cgi_module modules/mod_cgi.so
</IfModule>
#LoadModule dav_fs_module modules/mod_dav_fs.so
#LoadModule dav_lock_module modules/mod_dav_lock.so
#LoadModule vhost_alias_module modules/mod_vhost_alias.so
#LoadModule negotiation_module modules/mod_negotiation.so
LoadModule dir_module modules/mod_dir.so
#LoadModule imagemap_module modules/mod_imagemap.so
#LoadModule actions_module modules/mod_actions.so
#LoadModule speling_module modules/mod_speling.so
#LoadModule userdir_module modules/mod_userdir.so
LoadModule alias_module modules/mod_alias.so
#LoadModule rewrite_module modules/mod_rewrite.so

<IfModule unixd_module>
#
# If you wish httpd to run as a different user or group, you must run
# httpd as root initially and it will switch.  
#
# User/Group: The name (or #number) of the user/group to run httpd as.
# It is usually good practice to create a dedicated user and group for
# running httpd, as with most system services.
#
User www-data
Group www-data

</IfModule>

# 'Main' server configuration
#
# The directives in this section set up the values used by the 'main'
# server, which responds to any requests that aren't handled by a
# <VirtualHost> definition.  These values also provide defaults for
# any <VirtualHost> containers you may define later in the file.
#
# All of these directives may appear inside <VirtualHost> containers,
# in which case these default settings will be overridden for the
# virtual host being defined.
#

#
# ServerAdmin: Your address, where problems with the server should be
# e-mailed.  This address appears on some server-generated pages, such
# as error documents.  e.g. admin@your-domain.com
#
ServerAdmin you@example.com

#
# ServerName gives the name and port that the server uses to identify itself.
# This can often be determined automatically, but we recommend you specify
# it explicitly to prevent problems during startup.
#
# If your host doesn't have a registered DNS name, enter its IP address here.
#
#ServerName www.example.com:80

#
# Deny access to the entirety of your server's filesystem. You must
# explicitly permit access to web content directories in other 
# <Directory> blocks below.
#
<Directory />
    AllowOverride none
    Require all denied
</Directory>

#
# Note that from this point forward you must specifically allow
# particular features to be enabled - so if something's not working as
# you might expect, make sure that you have specifically enabled it
# below.
#

#
# DocumentRoot: The directory out of which you will serve your
# documents. By default, all requests are taken from this directory, but
# symbolic links and aliases may be used to point to other locations.
#
DocumentRoot "/usr/local/apache2/htdocs"
<Directory "/usr/local/apache2/htdocs">
    #
    # Possible values for the Options directive are "None", "All",
    # or any combination of:
    #   Indexes Includes FollowSymLinks SymLinksifOwnerMatch ExecCGI MultiViews
    #
    # Note that "MultiViews" must be named *explicitly* --- "Options All"
    # doesn't give it to you.
    #
    # The Options directive is both complicated and important.  Please see
    # http://httpd.apache.org/docs/2.4/mod/core.html#options
    # for more information.
    #
    Options Indexes FollowSymLinks

    #
    # AllowOverride controls what directives may be placed in .htaccess files.
    # It can be "All", "None", or any combination of the keywords:
    #   AllowOverride FileInfo AuthConfig Limit
    #
    AllowOverride None

    #
    # Controls who can get stuff from this server.
    #
    Require all granted
</Directory>

#
# DirectoryIndex: sets the file that Apache will serve if a directory
# is requested.
#
<IfModule dir_module>
    DirectoryIndex index.html
</IfModule>

#
# The following lines prevent .htaccess and .htpasswd files from being 
# viewed by Web clients. 
#
<Files ".ht*">
    Require all denied
</Files>

#
# ErrorLog: The location of the error log file.
# If you do not specify an ErrorLog directive within a <VirtualHost>
# container, error messages relating to that virtual host will be
# logged here.  If you *do* define an error logfile for a <VirtualHost>
# container, that host's errors will be logged there and not here.
#
ErrorLog /proc/self/fd/2

#
# LogLevel: Control the number of messages logged to the error_log.
# Possible values include: debug, info, notice, warn, error, crit,
# alert, emerg.
#
LogLevel warn

<IfModule log_config_module>
    #
    # The following directives define some format nicknames for use with
    # a CustomLog directive (see below).
    #
    LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\"" combined
    LogFormat "%h %l %u %t \"%r\" %>s %b" common

    <IfModule logio_module>
      # You need to enable mod_logio.c to use %I and %O
      LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\" %I %O" combinedio
    </IfModule>

    #
    # The location and format of the access logfile (Common Logfile Format).
    # If you do not define any access logfiles within a <VirtualHost>
    # container, they will be logged here.  Contrariwise, if you *do*
    # define per-<VirtualHost> access logfiles, transactions will be
    # logged therein and *not* in this file.
    #
    CustomLog /proc/self/fd/1 common

    #
    # If you prefer a logfile with access, agent, and referer information
    # (Combined Logfile Format) you can use the following directive.
    #
    #CustomLog "logs/access_log" combined
</IfModule>

<IfModule alias_module>
    #
    # Redirect: Allows you to tell clients about documents that used to 
    # exist in your server's namespace, but do not anymore. The client 
    # will make a new request for the document at its new location.
    # Example:
    # Redirect permanent /foo http://www.example.com/bar

    #
    # Alias: Maps web paths into filesystem paths and is used to
    # access content that does not live under the DocumentRoot.
    # Example:
    # Alias /webpath /full/filesystem/path
    #
    # If you include a trailing / on /webpath then the server will
    # require it to be present in the URL.  You will also likely
    # need to provide a <Directory> section to allow access to
    # the filesystem path.

    #
    # ScriptAlias: This controls which directories contain server scripts. 
    # ScriptAliases are essentially the same as Aliases, except that
    # documents in the target directory are treated as applications and
    # run by the server when requested rather than as documents sent to the
    # client.  The same rules about trailing "/" apply to ScriptAlias
    # directives as to Alias.
    #
    ScriptAlias /cgi-bin/ "/usr/local/apache2/cgi-bin/"

</IfModule>

<IfModule cgid_module>
    #
    # ScriptSock: On threaded servers, designate the path to the UNIX
    # socket used to communicate with the CGI daemon of mod_cgid.
    #
    #Scriptsock cgisock
</IfModule>

#
# "/usr/local/apache2/cgi-bin" should be changed to whatever your ScriptAliased
# CGI directory exists, if you have that configured.
#
<Directory "/usr/local/apache2/cgi-bin">
    AllowOverride None
    Options None
    Require all granted
</Directory>

<IfModule headers_module>
    #
    # Avoid passing HTTP_PROXY environment to CGI's on this or any proxied
    # backend servers which have lingering "httpoxy" defects.
    # 'Proxy' request header is undefined by the IETF, not listed by IANA
    #
    RequestHeader unset Proxy early
</IfModule>

<IfModule mime_module>
    #
    # TypesConfig points to the file containing the list of mappings from
    # filename extension to MIME-type.
    #
    TypesConfig conf/mime.types

    #
    # AddType allows you to add to or override the MIME configuration
    # file specified in TypesConfig for specific file types.
    #
    #AddType application/x-gzip .tgz
    #
    # AddEncoding allows you to have certain browsers uncompress
    # information on the fly. Note: Not all browsers support this.
    #
    #AddEncoding x-compress .Z
    #AddEncoding x-gzip .gz .tgz
    #
    # If the AddEncoding directives above are commented-out, then you
    # probably should define those extensions to indicate media types:
    #
    AddType application/x-compress .Z
    AddType application/x-gzip .gz .tgz

    #
    # AddHandler allows you to map certain file extensions to "handlers":
    # actions unrelated to filetype. These can be either built into the server
    # or added with the Action directive (see below)
    #
    # To use CGI scripts outside of ScriptAliased directories:
    # (You will also need to add "ExecCGI" to the "Options" directive.)
    #
    #AddHandler cgi-script .cgi

    # For type maps (negotiated resources):
    #AddHandler type-map var

    #
    # Filters allow you to process content before it is sent to the client.
    #
    # To parse .shtml files for server-side includes (SSI):
    # (You will also need to add "Includes" to the "Options" directive.)
    #
    #AddType text/html .shtml
    #AddOutputFilter INCLUDES .shtml
</IfModule>

#
# The mod_mime_magic module allows the server to use various hints from the
# contents of the file itself to determine its type.  The MIMEMagicFile
# directive tells the module where the hint definitions are located.
#
#MIMEMagicFile conf/magic

#
# Customizable error responses come in three flavors:
# 1) plain text 2) local redirects 3) external redirects
#
# Some examples:
#ErrorDocument 500 "The server made a boo boo."
#ErrorDocument 404 /missing.html
#ErrorDocument 404 "/cgi-bin/missing_handler.pl"
#ErrorDocument 402 http://www.example.com/subscription_info.html
#

#
# MaxRanges: Maximum number of Ranges in a request before
# returning the entire resource, or one of the special
# values 'default', 'none' or 'unlimited'.
# Default setting is to accept 200 Ranges.
#MaxRanges unlimited

#
# EnableMMAP and EnableSendfile: On systems that support it, 
# memory-mapping or the sendfile syscall may be used to deliver
# files.  This usually improves server performance, but must
# be turned off when serving from networked-mounted 
# filesystems or if support for these functions is otherwise
# broken on your system.
# Defaults: EnableMMAP On, EnableSendfile Off
#
#EnableMMAP off
#EnableSendfile on

# Supplemental configuration
#
# The configuration files in the conf/extra/ directory can be 
# included to add extra features or to modify the default configuration of 
# the server, or you may simply copy their contents here and change as 
# necessary.

# Server-pool management (MPM specific)
#Include conf/extra/httpd-mpm.conf

# Multi-language error messages
#Include conf/extra/httpd-multilang-errordoc.conf

# Fancy directory listings
#Include conf/extra/httpd-autoindex.conf

# Language settings
#Include conf/extra/httpd-languages.conf

# User home directories
#Include conf/extra/httpd-userdir.conf

# Real-time info on requests and configuration
#Include conf/extra/httpd-info.conf

# Virtual hosts
#Include conf/extra/httpd-vhosts.conf

# Local access to the Apache HTTP Server Manual
#Include conf/extra/httpd-manual.conf

# Distributed authoring and versioning (WebDAV)
#Include conf/extra/httpd-dav.conf

# Various default settings
#Include conf/extra/httpd-default.conf

# Configure mod_proxy_html to understand HTML4/XHTML1
<IfModule proxy_html_module>
Include conf/extra/proxy-html.conf
</IfModule>

# Secure (SSL/TLS) connections
#Include conf/extra/httpd-ssl.conf
#
# Note: The following must must be present to support
#       starting without SSL on platforms with no /dev/random equivalent
#       but a statically compiled-in mod_ssl.
#
<IfModule ssl_module>
SSLRandomSeed startup builtin
SSLRandomSeed connect builtin
</IfModule>

LoadModule proxy_module modules/mod_proxy.so
LoadModule proxy_http_module modules/mod_proxy_http.so

<VirtualHost *:80>
    ServerName localhost
    ProxyPass / http://tomcat:8080/ retry=1 acquire=3000 timeout=600
    ProxyPassReverse / http://tomcat:8080/
</VirtualHost>





```


### workers.properties

```
worker.list=worker1

worker.worker1.type=ajp13
worker.worker1.host=tomcat #yml에서 설정한 컨테이너 이름
worker.worker1.port=8080 # 포트는 똑같이

```






---


httpd.conf 안에 추가를 안해도 되는데 혹시 몰라서 저장
```
# mod_jk 추가
LoadModule jk_module modules/mod_jk.so
JkWorkersFile conf/workers.properties
JkLogFile logs/mod_jk.log
JkLogLevel info
JkLogStampFormat "[%a %b %d %H:%M:%S %Y]"
JkOptions +ForwardKeySize +ForwardURICompat -ForwardDirectories
```


workers.properties 두명일때 예시

```
worker.list=worker1, worker2

worker.worker1.type=ajp13
worker.worker1.host=tomcat1
worker.worker1.port=8009

worker.worker2.type=ajp13
worker.worker2.host=tomcat2
worker.worker2.port=8009

```





---
# 도커 컨테이너만 적용 시킬 때

network랑 같이 잡아줘서 컨테이너 생성
```
docker run --network=mynetwork --name apaches -d -p 80:80 httpd:latest
```



Dockerfile은 파일에서 확장자 켜기해서 뒤에 확장자 지우기
Dockerfile 작성시 
만약 여러개의 도커 파일 사용시 파일명/Dockerfile  이렇게 작성
만약 도커 파일 안에서 경로 잡아 줄 때 docker-compose.yml 기준으로 잡아야함
```
FROM ubuntu:latest

RUN apt-get update && apt-get install -y httpd tomcat8

RUN echo "ProxyPass /tomcat http://localhost:8080/ retry=1 acquire=3000 timeout=600" >> /etc/apache2/sites-available/000-default.conf
RUN echo "ProxyPassReverse /tomcat http://tomcat:8080/" >> /etc/apache2/sites-available/000-default.conf

CMD apachectl -DFOREGROUND

```



이미지 빌드
```
docker build -t apache-tomcat .

```

컨테이너 실행
```
docker run -d -p 80:80 apache-tomcat
```




---
도커 파일 작성 하지 않고 내부로 들어가서 설정

```
docker exec -it apaches /bin/bash >>설정 파일 진입
```

vim 없을경우
```
apt update
apt install vim
```

설정 파일 위치 
```
httpd -V | grep SERVER_CONFIG_FILE

```


vim 편집기 사용 위치가 여기에 있음
```
vim conf/httpd.conf
```

들어가서 밑에 내용 추가 뒤에 tomcat-test는 내 컨테이너 이름으로 변경
```
ProxyPass / http://tomcat-test:8080/ retry=1 acquire=3000 timeout=600
ProxyPassReverse / http://tomcat-test:8080/
```


추가후 :wq로 저장 앞에 꼭 :붙여야함


---
참조 - https://know-one-by-one.tistory.com/108 멀티 톰캣 아파치 설치


https://mvje.tistory.com/165