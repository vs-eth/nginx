# nginx

Tested on: Debian 9

Configure nginx for general use. TLS is always enabled per default enforced (in that case HTTP only gets you a HTTP 302). HSTS is always enabled. To use, simply put your own config under `/etc/nginx/base.d`. It will automatically be included in the default server block. If the specified certificate does not exist (yet), we upload `snakeoil` certificates(that is, a dummy certificate contained in this repositoriy along with it's private key) so that nginx is able to start. You're supposed to use our `letsencrypt` role to replace them with real certificates ;)

## Configuration
|Var|Default value|Description|
|---|-------------|-----------|
|nginx_fqdn|`ansible_fqdn`|The FQDN of this server, used especially for HTTPS|
|nginx_ssl_protocols|TLSv1.2|The SSL protocols to support, see below|
|nginx_ssl_ciphers|(see `defaults/main.yml`)|A 'modern' suite of ciphers. For more general settings or general configuration advice on this matter, see [Mozilla's excellent wiki](https://wiki.mozilla.org/Security/Server_Side_TLS) on that matter.|
|nginx_ledir|/var/www/letsencrypt|Where will Let's Encrypt challenges be located?|
|nginx_sslonly|True|Whether to enforce SSL by only sending 302 redirects on the HTTP port|
|nginx_enable_proxy|False|Whether to enable listening with the proxy protocol on port 444|
|nginx_cert_path|(see `defaults/main.yml`)|Path to the SSL certificate|
|nginx_key_path|(see `defaults/main.yml`)|Path to the SSL certificate's key|
|nginx_fullchain_path|(see `defaults/main.yml`)|Path to the certificate bundle used to verify an OCSP server's response|
|nginx_extra_hosts|`[]`| List of extra vhosts to create. They work just like the base vhost, see `defaults/main.yml` for how to format the list|
|nginx_proxy_subnet|`192.168.0.0/24`|Subnet to trust with origin IP when using the proxy protocol|
|nginx_clientcert|unset| Path to a valid CA cert for client certificates. Client certificates will be requested but not required. Mutually exclusive with OCSP stapling at the moment. |
|nginx_enable_spnego|`False`| Whether to enable SPNEGO (Kerberos) support|
|nginx_spnego_realm|`EXAMPLE.ORG`| Which kerberos realm to use|
|nginx_spenego_users|`[]`| If set, which users to limit auth to|

## Dependencies
None.

## SPNEGO
Support is based on [this](https://github.com/stnoonan/spnego-http-auth-nginx-module) module.
You're expected to set up a keytab with `HTTP/{{ fqdn }}@REALM` in `/etc/nginx/krb5-ngx.keytab`, authentication is enabled by setting `auth_gss on;`.
