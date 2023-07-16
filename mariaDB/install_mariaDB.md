# mariaDB 내 리눅스에 설치하기
```bash
0. 작업 디렉토리
# pwd
# /root

1. 의존 라이브러리 설치
# yum install -y gcc
# yum install -y gcc-c++
# yum install -y libtermcap-devel
# yum install -y gdbm-devel
# yum install -y zlib*
# yum install -y libxml*
# yum install -y freetype*
# yum install -y libpng* 
# yum install -y flex
# yum install -y gmp
# yum install -y ncurses-devel
# yum install -y cmake.x86_64
# yum install -y libaio

iconv 설치
1-1. 소스 다운로드
# wget --no-check-certificate https://ftp.gnu.org/pub/gnu/libiconv/libiconv-1.17.tar.gz

1-2. 압축 풀기
# tar xvfz libiconv-1.17.tar.gz

1-3. 빌드 & 설치
# cd libiconv-1.17
# ./configure --prefix=/usr/local
# make
# make install

2. 소스 다운로드
# wget --no-check-certificate https://downloads.mariadb.org/interstitial/mariadb-10.1.48/source/mariadb-10.1.48.tar.gz 

3. 압축 풀기
# tar xvfz mariadb-10.1.48.tar.gz

4. 소스 이동
# cd mariadb-10.1.48

5. 빌드 환경 설정 
# cmake -DCMAKE_INSTALL_PREFIX=/usr/local/bootcamp/mariadb -DMYSQL_USER=mysql -DMYSQL_TCP_PORT=3306 -DMYSQL_DATADIR=/usr/local/bootcamp/mariadb/data -DMYSQL_UNIX_ADDR=/usr/local/bootcamp/mariadb/tmp/mariadb.sock -DINSTALL_SYSCONFDIR=/usr/local/bootcamp/mariadb/etc -DINSTALL_SYSCONF2DIR=/usr/local/bootcamp/mariadb/etc/my.cnf.d -DDEFAULT_CHARSET=utf8 -DDEFAULT_COLLATION=utf8_general_ci -DWITH_EXTRA_CHARSETS=all -DWITH_ARIA_STORAGE_ENGINE=1 -DWITH_XTRADB_STORAGE_ENGINE=1 -DWITH_ARCHIVE_STORAGE_ENGINE=1 -DWITH_INNOBASE_STORAGE_ENGINE=1 -DWITH_PARTITION_STORAGE_ENGINE=1 -DWITH_BLACKHOLE_STORAGE_ENGINE=1 -DWITH_FEDERATEDX_STORAGE_ENGINE=1 -DWITH_PERFSCHEMA_STORAGE_ENGINE=1 -DWITH_READLINE=1 -DWITH_SSL=bundled -DWITH_ZLIB=system

6. 빌드
# make

7. 설치
# make install
# cd
# pwd
/root

8. 계정 생성
# groupadd mysql
# useradd -M -g mysql mysql 

9. 인스톨 디렉토리 /usr/local/bootcamp/mariadb 소유자 변경
# chown -R mysql:mysql /usr/local/bootcamp/mariadb

10. 설정파일 위치 변경
# cp -R /usr/local/bootcamp/mariadb/etc/my.cnf.d /etc

[root@localhost ~]# cp -R /usr/local/bootcamp/mariadb/etc/my.cnf.d /etc
cp: overwrite '/etc/my.cnf.d/mysql-clients.cnf'? y

11. 기본(관리) 데이터베이스(mysql) 생성
# /usr/local/bootcamp/mariadb/scripts/mysql_install_db --user=mysql --basedir=/usr/local/bootcamp/mariadb --defaults-file=/usr/local/bootcamp/mariadb/etc/my.cnf --datadir=/usr/local/bootcamp/mariadb/data

12. 서버 구동
# /usr/local/bootcamp/mariadb/bin/mysqld_safe &

엔터 한번 쳐야함.

13. root 패스워드 설정
# /usr/local/bootcamp/mariadb/bin/mysqladmin -u root password

14. 데이터베이스 접속 테스트
# /usr/local/bootcamp/mariadb/bin/mysql -u root -p

15. path 설정(/etc/profile)
# vi /etc/profile
-----------------------------------------------------
# mysql
export PATH=$PATH:/usr/local/bootcamp/mariadb/bin
-----------------------------------------------------
# source /etc/profile

16. 서비스(데몬, Daemon) 등록/시작/중지
# cp /usr/local/bootcamp/mariadb/support-files/mysql.server /etc/init.d/mariadb
# systemctl start mariadb
# systemctl enable mariadb

17. mariaDB 부팅 시 자동 시작
# sudo chkconfig mariadb on
# sudo service mariadb start
```
