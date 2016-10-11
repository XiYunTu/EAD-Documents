### 1.Ngnix安装

#### 1.1 创建ngnix用户

1. 通过root用户登录两台，执行  
`# useradd -d /data/ngnix -m ngnix`  
2. 修改密码  
`# passwd ngnix`  
`ngnix****`  

#### 1.2 上传Ngnix及依赖安装包

1. 创建Ngnix安装目录  
通过ngnix用户登录  
`# cd /data/ngnix`  
`# mkdir install`  
2. 通过ngnix用户，将ngnix安装包nginx-1.9.9.tar.gz及依赖包openssl-1.0.0o.tar.gz、pcre-8.12.tar.gz、zlib-1.2.8.tar.gz上传到install目录  
3. 解压所有安装包  
`# cd /data/ngnix/install`  
`# tar -xzvf nginx-1.9.9.tar.gz`  
`# tar -xzvf openssl-1.0.0o.tar.gz`  
`# tar -xzvf pcre-8.12.tar.gz`  
`# tar -xzvf zlib-1.2.8.tar.gz`  

#### 1.3 安装依赖包

1. 安装zlib依赖包  
`# cd /data/ngnix/install`  
`# cd zlib-1.2.8/`  
`# ./configure`  
`# make && make install`  
2. 安装pcre依赖包  
`# cd /data/ngnix/install`  
`# chmod -R 777 pcre-8.12`  
`# cd pcre-8.12`  
`# ./configure`  
`# make && make install`  
3. 设置pcre lib包  
`# mkdir -p /data/ngnix/install/pcre-8.12/.libs`  
`# cp /usr/local/lib/libpcre.a /data/ngnix/install/pcre-8.12/libpcre.a`  
`# cp /usr/local/lib/libpcre.a /data/ngnix/install/pcre-8.12/libpcre.la`  
`# cp /usr/local/lib/libpcre.a /data/ngnix/install/pcre-8.12/.libs/libpcre.a`  
`# cp /usr/local/lib/libpcre.a /data/ngnix/install/pcre-8.12/.libs/libpcre.la`  
4. 安装openssl依赖包  
`# cd /data/ngnix/install`  
`# cd openssl-1.0.0o/`  
`# ./config`  
`# make && make install`  

#### 1.4 安装Ngnix  

`# cd /data/ngnix/install`  
`# cd nginx-1.9.9/`  
`# ./configure --user=nobody --group=nobody --prefix=/usr/local/webserver/nginx --with-http_stub_status_module --with-http_ssl_module --with-pcre=/data/ngnix/install/pcre-8.12 --with-openssl=/data/ngnix/install/openssl-1.0.0o`  
`# make && make install`  

#### 1.5 配置Ngnix

修改Ngnix的配置文件 /usr/local/webserver/nginx/conf/nginx.conf，内容如下：  

`#user  nobody;`  
`worker_processes  4;`  

`#error_log  logs/error.log;`  
`error_log  logs/error.log  notice;`  
`#error_log  logs/error.log  info;`  

`pid        logs/nginx.pid;`  

`events {`  
`    worker_connections  10240;`  
`}`   

`http`   
`{`   
`  include       mime.types;`  
`  default_type  application/octet-stream;`   
`  fastcgi_intercept_errors on;`   
`  charset  utf-8;`   
`  server_names_hash_bucket_size 128;`   
`  client_header_buffer_size 4k;`   
`  large_client_header_buffers 4 32k;`   
`  client_max_body_size 300m;`   
`  fastcgi_buffers 8 128k;`  
`  sendfile on;`   
`  tcp_nopush     on;`   
      
`  keepalive_timeout 60;`   

`  tcp_nodelay on;`   
`  client_body_buffer_size  512k;`   
    
`  proxy_connect_timeout    5;`   
`  proxy_read_timeout       60;`   
`  proxy_send_timeout       5;`   
`  proxy_buffer_size        16k;`   
`  proxy_buffers            4 64k;`   
`  proxy_busy_buffers_size 128k;`   
`  proxy_temp_file_write_size 128k;`   
  
`  gzip on;`   
`  gzip_min_length  1k;`   
`  gzip_buffers     4 16k;`   
`  gzip_http_version 1.1;`   
`  gzip_comp_level 2;`   
`  gzip_types       text/plain application/x-javascript text/css application/xml;`   
`  gzip_vary on;`   

`### change nginx logs`   
`log_format  main  '$http_x_forwarded_for - $remote_user [$time_local] "$request" '  `
`              '$status $body_bytes_sent "$http_referer" '`  
`              '"$http_user_agent"  $request_time $remote_addr';`  
                  
`upstream web_app {`   
` server eas.whchem.com:8080;`  
`}`   
    
`#### eas.whchem.com`   
`server {`   
`    listen 8080;`   
`    server_name  eas.whchem.com;`   
`    index index.jsp index.html index.html;`   
`    #发布目录`  
`    root  /;`   
`location /`  
`    {`  
`    proxy_next_upstream http_502 http_504 error timeout invalid_header;`  
`    proxy_set_header Host  $host:$server_port;`  
`    proxy_set_header X-Real-IP $remote_addr;`  
`    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;`  
`    proxy_pass http://web_app;`  
`    }`  
`  }`  
`}`  

#### 1.6 Ngnix常用命令

1. 停止  
查询nginx主进程号  
`ps -ef | grep nginx`  
从容停止   `kill -QUIT 主进程号`  
快速停止   `kill -TERM 主进程号`  
强制停止   `kill -9 nginx`  
2. 启动  
`# cd  /usr/local/webserver/nginx/sbin/`  
`# ./nginx -c /usr/local/webserver/nginx/conf/nginx.conf`  
3. 判断配置文件是否正确  
`# cd  /usr/local/webserver/nginx/sbin/`  
`# nginx -t -c /usr/local/nginx/conf/nginx.conf`  
或者  
`# ./nginx -t`  
3. 重新加载  
`# cd  /usr/local/webserver/nginx/sbin/`  
`# ./nginx -s reload`
