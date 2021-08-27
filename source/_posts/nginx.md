---
title: nginx
---
## nginx 的安装

### ubuntu 下安装
```linux
apt install nginx
```
## nginx 虚拟主机配置
```linux
sudo vim /etc/nginx/nginx.conf

在 http {}中添加

server{
        	listen 80; //监听端口
      		server_name www.xuecheng.com; //服务名
        	ssi on; //是否开启ssi
        	ssi_silent_errors on;
        	location / {
                	alias  /home/spiderbao/webStrom_dev/xc-ui-pc-static-portal/;//主机映射的路径
                	index  index.html;
        	}
	}

```