GPU相关问题
===========

GPU安装
-------

sudo yum whatprovides */lspci

sudo yum install pciutils

查看GPU信息问题
---------------

lspci | grep -i nvidia