- OAMlab
- https://github.com/oamlab

# 关于《编译与软件包》

---

注意：以下为提供参考的安装或编译过程，具体过程可根据自有情况进行调整。

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

[Nginx-QUIC官方安装文档](https://quic.nginx.org/readme.html)

#### 1、编译主机与操作系统
- 用于编译的机器，推荐为8核32G
- 操作系统为CentOS 7.x.xxxx x86 64bit

#### 2、安装相关的软件
- 安装部分基础组件

``` bash
yum install epel-release.noarch
yum install "Development Tools"
yum install lrzsz wget unzip hg git gcc-c++ make automake bzip2
yum install libunwind-devel openssl-devel
yum install go
```

#### 3、编译安装 gcc-11.1.0
- 编译安装gcc

``` bash
wget http://ftp.gnu.org/gnu/gcc/gcc-10.1.0/gcc-10.1.0.tar.gz

tar -zxvf gcc-11.1.0.tar.gz
cd gcc-11.1.0/

./contrib/download_prerequisites
2023-04-26 00:00:00 URL:http://gcc.gnu.org/pub/gcc/infrastructure/gmp-6.1.0.tar.bz2 [2383840/2383840] -> "./gmp-6.1.0.tar.bz2" [1]
2023-04-26 00:00:00 URL:http://gcc.gnu.org/pub/gcc/infrastructure/mpfr-3.1.4.tar.bz2 [1279284/1279284] -> "./mpfr-3.1.4.tar.bz2" [1]
2023-04-26 00:00:00 URL:http://gcc.gnu.org/pub/gcc/infrastructure/mpc-1.0.3.tar.gz [669925/669925] -> "./mpc-1.0.3.tar.gz" [1]
2023-04-26 00:00:00 URL:http://gcc.gnu.org/pub/gcc/infrastructure/isl-0.18.tar.bz2 [1658291/1658291] -> "./isl-0.18.tar.bz2" [1]

yum install bzip2

bzip2 -d gmp-6.1.0.tar.bz2
tar xf gmp-6.1.0.tar gmp-6.1.0
cd gmp-6.1.0
./configure --prefix=/usr/local/gmp-6.1.0
make -j8
make install
ll /usr/local/gmp-6.1.0

bzip2 -d mpfr-3.1.4.tar.bz2
tar xf mpfr-3.1.4.tar mpfr-3.1.4
cd ./mpfr-3.1.4
./configure --prefix=/usr/local/mpfr-3.1.4 --with-gmp=/usr/local/gmp-6.1.0
make -j8
make install
ll /usr/local/mpfr-3.1.4

tar xzvf mpc-1.0.3.tar.gz
cd ./mpc-1.0.3
./configure --prefix=/usr/local/mpc-1.0.3 --with-gmp=/usr/local/gmp-6.1.0 --with-mpfr=/usr/local/mpfr-3.1.4
make -j8
make install
ll /usr/local/mpc-1.0.3

bzip2 -d isl-0.18.tar.bz2
tar xf isl-0.18.tar isl-0.18
cd ./isl-0.18
./configure -prefix=/usr/local/isl-0.18 --with-gmp-prefix=/usr/local/gmp-6.1.0
make -j8
make install
ll /usr/local/isl-0.18

./configure -prefix=/usr/local/gcc-11.1.0 --enable-threads=posix --disable-checking --disable-multilib --enable-languages=c,c++ --with-gmp=/usr/local/gmp-6.1.0 --with-mpfr=/usr/local/mpfr-3.1.4 --with-mpc=/usr/local/mpc-1.0.3
make -j8
make install

cd /bin
mv gcc gcc_bak
mv g++ g++_bak
mv c++ c++_bak
ln -s /usr/local/gcc-11.1.0/bin/gcc gcc
ln -s /usr/local/gcc-11.1.0/bin/g++ g++
ln -s /usr/local/gcc-11.1.0/bin/c++ c++
gcc -v

echo "export LD_LIBRARY_PATH=/usr/local/gcc-11.1.0/lib:/usr/local/gcc-11.1.0/lib64:$LD_LIBRARY_PATH" >> /etc/profile
cat /etc/profile
source /etc/profile
# reboot
```

#### 4、编译安装 cmake
- 编译安装cmake

``` bash
yum remove cmake
wget https://cmake.org/files/v3.18/cmake-3.18.2.tar.gz
tar -zxvf cmake-3.18.2.tar.gz
cd cmake-3.18.2
# make uninstall
./bootstrap
gmake -j8
gmake install
cmake -version
```

#### 5、编译安装 boringssl
- 编译安装boringssl

``` bash
git clone https://github.com/google/boringssl.git
cd boringssl
mkdir build
cd build
cmake ../
make -j8
make install
```

#### 7、编译安装Nginx-QUIC
- 获取Nginx-QUIC源码包: https://hg.nginx.org/nginx-quic

``` bash
wget http://labs.frickle.com/files/ngx_cache_purge-2.3.tar.gz
tar zxf ngx_cache_purge-2.3.tar.gz

unzip nginx.zip
./auto/configure --prefix=/usr/local/nginx --add-module=/software/ngx_cache_purge-2.3 --with-http_stub_status_module --with-http_addition_module  --with-http_ssl_module  --with-http_v2_module --with-http_v3_module --with-cc-opt="-I/software/boringssl/include" --with-ld-opt="-L/software/boringssl/build/ssl -L/software/boringssl/build/crypto"
make -j8
make install
```

#### 8、制品
- [nginx.zip](./nginx.zip)

``` bash
# sha256sum nginx.zip
f146b4a56b488f59649bdef581e4b4c78d830e71b302046d9b7baa3bab3d5e39  nginx.zip
```