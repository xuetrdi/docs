====================
Mac 下的go get 步骤:
====================

1. 安装,启动,配置Shadowsocks,官网下载.然后配置自己的VPS服务器
2. 安装配置启动privoxy

   - 安装: 
   .. code-block:: shell
    
        $ brew install privoxy

   - 配置: 
   .. code-block:: shell
    
        $ vim /usr/local/etc/privoxy/config
        
   - 末尾追加内容如下:: 
   
       listen-address 0.0.0.0:8118

       forward-socks5 / 127.0.0.1:1080 .

       [注意] 最后的那个点。
         
   - 启动: 
   .. code-block:: shell
    
        $ sudo /usr/local/sbin/privoxy /usr/local/etc/privoxy/config

        # 不报错说明启动成功. 

3. 设置go 代理

   .. code-block:: shell
    
        $ alias go='https_proxy=127.0.0.1:8118 go'

4. 执行go get 命令就可以正常了。
