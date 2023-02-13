## How to install BIND9 + DNSTAP on Ubuntu 20.04 | 22.04

### Install the depending packages

python3-ply: ```apt install python3-ply```
python-ply: ```apt install python-ply```
OpenSSL: ```apt install libssl-dev```

To build BIND with dnstap support you must ensure that the runtime libraries required by the dnstap sender (which will be embedded in BIND) are present. There are two nonstandard library packages that dnstap requires, and one of those depends in turn on a Google library package. These three packages are all available from github, and you must install them in this order:

### Git clone fstrm
```
git clone https://github.com/farsightsec/fstrm.git
```
=========================

### Install Protobuf
```
wget https://github.com/protocolbuffers/protobuf/archive/refs/tags/v21.12.tar.gz
tar -xvf v21.12.tar.gz
cd protobuf-21.12/
./autogen.sh
./configure
make
make install
```
=========================

### Install Protobuf-c
```
git clone https://github.com/protobuf-c/protobuf-c
cd protobuf-c/
./configure
make
make install
```
=========================
### Install libevent
```
wget https://github.com/libevent/libevent/archive/refs/tags/release-2.1.12-stable.tar.gz
tar -xvf release-2.1.12-stable.tar.gz
cd libevent-release-2.1.12-stable/
./autogen.sh
./configure
make
make install
```
=========================
### Download & install BIND9.11 package:
```
wget https://ftp.isc.org/isc/bind9/9.11.37/bind-9.11.37.tar.gz
tar -xvf bind-9.11.37.tar.gz
cd bind-9.11.37/
./configure  --enable-dnstap
make all
make install
```

Check BIND9 version
```
root@dns-server1:~# named -V
BIND 9.11.37 (Extended Support Version) <id:796133c>
running on Linux x86_64 5.4.0-139-generic #156-Ubuntu SMP Fri Jan 20 17:27:18 UTC 2023
built by make with '--enable-dnstap'
compiled by GCC 9.4.0
compiled with OpenSSL version: OpenSSL 1.1.1f  31 Mar 2020
linked to OpenSSL version: OpenSSL 1.1.1f  31 Mar 2020
compiled with zlib version: 1.2.11
linked to zlib version: 1.2.11
compiled with protobuf-c version: 1.4.1
linked to protobuf-c version: 1.4.1
threads support is enabled

default paths:
  named configuration:  /etc/named.conf
  rndc configuration:   /etc/rndc.conf
  DNSSEC root key:      /etc/bind.keys
  nsupdate session key: /var/run/named/session.key
  named PID file:       /var/run/named/named.pid
  named lock file:      /var/run/named/named.lock
```
=====================
### DNSTAP configuration in BIND9
```
nano /etc/bind/named.conf.options
options {
…

//DNSTAP config

dnstap {auth; resolver query;};
dnstap-output file "/etc/bind/test.dnstap";

dnstap-identity "Test DNSTAP";
dnstap-version "Unknown";

…
};
```


### How to Read DNSTAP files
```
dnstap-read /etc/bind/test.dnstap
```










