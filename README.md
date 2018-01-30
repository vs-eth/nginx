# nginx

Tested on: Debian 8, 9

Configure nginx for general use. TLS is always enabled per default enforced (in that case HTTP only gets you a HTTP 302). To use, simply put your own config under `/etc/nginx/base.d`. It will automatically be included in the default server block. If the specified certificate does not exist (yet), we upload `snakeoil` certificates(that is, a dummy certificate contained in this repositoriy along with it's private key) so that nginx is able to start. You're supposed to use our `letsencrypt` role to replace them with real certificates ;)

## Configuration
|Var|Default value|Description|
|---|-------------|-----------|
|nginx_fqdn|`ansible_fqdn`|The FQDN of this server, used especially for HTTPS|
|nginx_ssl_protocols|TLSv1.2|The SSL protocols to support, see below|
|nginx_ssl_ciphers|(see `defaults/main.yml`)|All PFS ciphers from TLS1.2. For more general settings or general configuration advice on this matter, see [Mozilla's excellent wiki](https://wiki.mozilla.org/Security/Server_Side_TLS) on that matter.|
|nginx_ledir|/var/www/letsencrypt|Where will Let's Encrypt challenges be located?|
|nginx_sslonly|True|Whether to enforce SSL by only sending 302 redirects on the HTTP port|
|nginx_enable_proxy|False|Whether to enable listening with the proxy protocol on port 444|
|nginx_cert_path|(see `defaults/main.yml`)|Path to the SSL certificate|
|nginx_key_path|(see `defaults/main.yml`)|Path to the SSL certificate's key|
|nginx_fullchain_path|(see `defaults/main.yml`)|Path to the certificate bundle used to verify an OCSP server's response|

# Dependencies
None.
