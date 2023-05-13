- OAMlab
- https://github.com/oamlab

# 关于《编译或软件包》

- ----------------------------

注意：以下为提供参考的安装或编译过程，具体根据自有情况进行调整。

## CentOS Stream 9：

#### 1、引入Nginx-QUIC的软件源
- 当前文件目录内有 [nginx-quic.repo](./nginx-quic.repo) ，放入到目录/etc/yum.repos.d
- 目标：/etc/yum.repos.d/nginx-quic.repo

``` bash
[nginx-quic]
name=nginx-quic repo
baseurl=https://packages.nginx.org/nginx-quic/rhel/9/$basearch/
gpgcheck=1
enabled=1
gpgkey=https://nginx.org/keys/nginx_signing.key
```

#### 2、使用dnf命令进行安装

``` bash
dnf install nginx-quic
```

## CentOS 7：

#### 1、编译安装...
- 用于编译的机器，推荐为8核32G
- 操作系统为CentOS 7.x.xxxx x86 64bit

#### 2、编译安装...
- ...

#### 3、编译安装...
- ...

#### 4、编译安装...
- ...

#### 5、编译安装 gcc-11.1.0
- 编译gcc
``` bash
./configure -prefix=/usr/local/gcc-11.1.0 --enable-threads=posix --disable-checking --disable-multilib --enable-languages=c,c++ --with-gmp=/usr/local/gmp-6.1.0 --with-mpfr=/usr/local/mpfr-3.1.4 --with-mpc=/usr/local/mpc-1.0.3
make -j8
make install
```

#### 6、编译安装...
- ...

#### 7、编译安装...
- ...

#### 8、编译安装Nginx-QUIC
- 编译时长大概为30分钟

``` bash
./auto/configure --prefix=/usr/local/nginx --add-module=/software/ngx_cache_purge-2.3 --with-http_stub_status_module --with-http_addition_module  --with-http_ssl_module  --with-http_v2_module --with-http_v3_module --with-cc-opt="-I/software/boringssl/include" --with-ld-opt="-L/software/boringssl/build/ssl -L/software/boringssl/build/crypto"
make -j8
make install
```

#### 9、制品
- [nginx.zip](./nginx.zip)

``` bash
# sha256sum nginx.zip
f146b4a56b488f59649bdef581e4b4c78d830e71b302046d9b7baa3bab3d5e39  nginx.zip
```