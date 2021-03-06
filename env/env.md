1. 配置linux源

  ```shell
  sudo mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOSBase.repo.bak
  sudo wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
  ```

2. 设置字符集

```shell
   su root
   echo LANG="en_GB.utf8"
```

3. Python 安装配置

```shell
PYTHON_VERSION=3.8.5
sudo wget https://www.python.org/ftp/python/${PYTHON_VERSION}/Python-${PYTHON_VERSION}.tgz
tar -xf Python-${PYTHON_VERSION}.tgz
cd Python-${PYTHON_VERSION}

# Linux
sudo ./configure --with-ssl --prefix=/usr/local/python3
sudo make
sudo make install

# Mac
./configure --enable-optimizations --prefix=/usr/local/python --with-openssl=/usr/local/opt/openssl  --with-tcltk-includes='-I/usr/local/opt/tcl-tk/include' --with-tcltk-libs='-L/usr/local/opt/tcl-tk/lib'
make && make install
```

[注意]编译安装过程依赖包zlib，负责不会成功.报INFO的需要tcl-dev，tk-dev，tcl,tk但是不影响编译安装成功

配置pip工具（下载源，格式）

```shell
[global]
index-url = http://pypi.douban.com/simple
trusted-host = pypi.douban.com
[list]
format=columns
```

4. MYSQL 安装配置

```shell
sudo rpm -ivh http://dev.mysql.com/get/mysql-community-release-el6-5.noarch.rpm
sudo yum install -y mysql mysql-server mysql-devel zlib-devel 
```

配置mysql服务器(/etc/my.cnf)

```
[mysqld]
default-storage-engine = innodb
innodb_file_per_table
collation-server = utf8_general_ci
init-connect = 'SET NAMES utf8'
character-set-server = utf8 
```

启动mysql-server

```shell
sudo service mysqld start
```

设置或者修改root密码

```shell
cd ~/
mysql_secure_installation
```

5. 配置vim

```shell
sudo yum -y install vim
```

配置vim(~/.vimrc)

```shell
set tabstop=4
set shiftwidth=4
set softtabstop=4
set expandtab
set fileformat=unix
set nobomb
set ff=unix
set ambiwidth=double
set fileencodings=utf-8,ucs-bom,cp936
syntax on
filetype plugin on
set nocompatible
set completeopt=preview
set ai
set hls
set nu
```



