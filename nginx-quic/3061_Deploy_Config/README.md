- OAMlab
- https://github.com/oamlab

# 关于《配置文件》

- ----------------------------

## 在原来的配置文件内增加http/3的配置：

### 1、参考如下

``` bash
server {
	 listen 443 quic reuseport;
	 listen 443 ssl http2;

	 server_name abc.com www.abc.com;

	 ssl_protocols TLSv1.1 TLSv1.2 TLSv1.3;
	 ssl_ecdh_curve X25519:P-256:P-384;
	 ssl_ciphers TLS13-CHACHA20-POLY1305-SHA256:TLS13-AES-256-GCM-SHA384:TLS13-AES-128-GCM-SHA256:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-CHACHA20-POLY1305:EECDH+CHACHA20:EECDH+AES128;

	 ssl_early_data on;
	 
	 proxy_set_header Early-Data $ssl_early_data;

	 add_header Strict-Transport-Security "max-age=63072000; includeSubDomains; preload";
	 add_header Alt-Svc 'h3-27=":443"; h3-28=":443"; h3-29=":443"; ma=86400; quic=":443"';

	 ...
	 ..
	 .
}
```