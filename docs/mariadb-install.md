## MariaDB 설치

0. 작업 디렉토리 확인
```bash
# pwd
/root
```

1. 의존 라이브러리 설치
```bash
# yum install -y gcc
# yum install -y gcc-c++
# yum install -y zlib*
# yum install -y libxml*
# yum install -y freetype*
# yum install -y libpng* 
# yum install -y flex
# yum install -y gmp
# yum install -y ncurses-devel
# yum install -y gnutls-devel
# yum install -y libaio
```
2. iconv 소스 컴파일 설치를 한다.
```bash
# wget https://ftp.gnu.org/pub/gnu/libiconv/libiconv-1.17.tar.gz
# tar xvfz libiconv-1.17.tar.gz
# cd libiconv-1.17
# ./configure --prefix=/usr/local
# make
# make install
```

3. 소스 다운로드
```bash
# wget https://downloads.mariadb.org/interstitial/mariadb-10.6.17/source/mariadb-10.6.17.tar.gz 
```

4. 압축 풀기
```bash
# tar xvfz mariadb-10.6.17.tar.gz
```

5. 소스 디렉토리 이동
```bash
# cd mariadb-10.6.17
```

6. 빌드 환경 설정 
```bash
# cmake -DCMAKE_INSTALL_PREFIX=/usr/local/poscodx/mariadb -DMYSQL_USER=mysql -DMYSQL_TCP_PORT=3306 -DMYSQL_DATADIR=/usr/local/poscodx/mariadb/data -DMYSQL_UNIX_ADDR=/usr/local/poscodx/mariadb/tmp/mariadb.sock -DINSTALL_SYSCONFDIR=/usr/local/poscodx/mariadb/etc -DINSTALL_SYSCONF2DIR=/usr/local/poscodx/mariadb/etc/my.cnf.d -DDEFAULT_CHARSET=utf8 -DDEFAULT_COLLATION=utf8_general_ci -DWITH_EXTRA_CHARSETS=all -DWITH_ARIA_STORAGE_ENGINE=1 -DWITH_XTRADB_STORAGE_ENGINE=1 -DWITH_ARCHIVE_STORAGE_ENGINE=1 -DWITH_INNOBASE_STORAGE_ENGINE=1 -DWITH_PARTITION_STORAGE_ENGINE=1 -DWITH_BLACKHOLE_STORAGE_ENGINE=1 -DWITH_FEDERATEDX_STORAGE_ENGINE=1 -DWITH_PERFSCHEMA_STORAGE_ENGINE=1 -DWITH_READLINE=1 -DWITH_SSL=bundled -DWITH_ZLIB=system
```

7. 빌드
```bash
# make
```

8. 설치
```bash
# make install
```

9. 설정 작업을 위해 root 홈디렉토리 이동
```bash
# cd 
# pwd
/root
```

10. 계정 생성
```bash
# groupadd mysql
# useradd -M -g mysql mysql 
```

11. mariadb 인스톨 디렉토리 소유자 변경
```bash
# chown -R mysql:mysql /usr/local/poscodx/mariadb
```

12. 설정파일 위치 변경
```bash
cp -R /usr/local/poscodx/mariadb/etc/my.cnf.d /etc
```

13. 기본(관리) 데이터베이스(mysql) 생성
```bash
# /usr/local/poscodx/mariadb/scripts/mysql_install_db --user=mysql --basedir=/usr/local/poscodx/mariadb --defaults-file=/usr/local/poscodx/mariadb/etc/my.cnf --datadir=/usr/local/poscodx/mariadb/data
```
14. 서버 구동
```bash
# /usr/local/poscodx/mariadb/bin/mysqld_safe &
```

15. root 패스워드 설정
```bash
# /usr/local/poscodx/mariadb/bin/mysqladmin -u root password '........'
```

16. 데이터베이스 접속 테스트
```bash
# /usr/local/poscodx/mariadb/bin/mysql -u root -p
```

17. path 설정(/etc/profile)
```bash
# mysql
export PATH=$PATH:/usr/local/poscodx/mariadb/bin
```

18. 서버 강제 종료
```bash
# ps -ef | grep mysql
root       865     1  0 16:23 ?        00:00:00 /bin/sh /usr/local/poscodx/mariadb/bin/mysqld_safe --datadir=/usr/local/poscodx/mariadb/data --pid-file=/usr/local/poscodx/mariadb/data/lx.poscodx.me.pid
mysql      968   865  0 16:23 ?        00:00:00 /usr/local/poscodx/mariadb/bin/mysqld --basedir=/usr/local/poscodx/mariadb --datadir=/usr/local/poscodx/mariadb/data --plugin-dir=/usr/local/poscodx/mariadb/lib/plugin --user=mysql --log-error=/usr/local/poscodx/mariadb/data/lx.poscodx.me.err --pid-file=/usr/local/poscodx/mariadb/data/lx.poscodx.me.pid
# kill -9 865 968
# ps -ef | grep mysql
```

19. mariadb systemd service script(mariadb.service) 작성
```
#
# mariadb systemd service file
#

[Unit]
Description=MariaDB 10.6.11 Server
After=network.target
After=syslog.target

[Install]
WantedBy=multi-user.target
Alias=mariadb.service

[Service]
User=mysql
Group=mysql

# Execute pre and post scripts as root
PermissionsStartOnly=true

# Needed to create system tables etc.
# ExecStartPre=/usr/bin/mysql-systemd-start pre

# Start main service
ExecStart=/usr/local/poscodx/mariadb/bin/mysqld_safe

# Don't signal startup success before a ping works
# ExecStartPost=/usr/bin/mysql-systemd-start post

# Give up if ping don't get an answer
TimeoutSec=600
Restart=always
PrivateTmp=false
```

21. 서비스(데몬, Daemon) 등록/시작/중지
```bash
# ls /etc/systemd/system/mariadb.service
/etc/systemd/system/mariadb.service
# systemctl enable mariadb.service
# systemctl start mariadb
# ps -ef | grep mysql
```

21. 재부팅 후, mysql 클라이언트로 접속 테스트
```sh
# mysql -u root -p
password:
MariaDB [(none)]>
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbNTI5MDQxMjEwXX0=
-->
