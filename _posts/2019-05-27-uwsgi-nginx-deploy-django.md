---
layout: post
title: uWSGI, Nginx 搭建 Django 正式环境
categories: python
comments: true
---

## 安装 uwsgi

```bash
pipenv install uwsgi
```

启动uwsgi ，这边要使用 socket 设置端口号，只有socket 协议可以和 nginx 交互。

```bash
uwsgi --socket :8000 --module project.wsgi
```

## 下载 uwsgi_params

uwsgi_params 文件在后面配置nginx的时候会使用到

```bash
wget https://raw.githubusercontent.com/nginx/nginx/master/conf/uwsgi_params
```

## 配置 Nginx

```nginx
# djangpo.conf

# the upstream component nginx needs to connect to
upstream django {
    # server unix:///path/to/your/mysite/mysite.sock; # for a file socket
    server 127.0.0.1:8000; # for a web port socket (we'll use this first)
}

# configuration of the server
server {
    # the port your site will be served on
    listen      8080;
    # the domain name it will serve for
#    server_name 127.0.0.1; # substitute your machine's IP address or FQDN
    charset     utf-8;

    # max upload size
    client_max_body_size 75M;   # adjust to taste

    # Django media
    location /media  {
        alias /home/ubuntu/project/staticfiles;  # your Django project's media files - amend as required
    }

	# static 会导致 Django的 admin css 获取失败，暂时先不配置；
    #location /static {
    #    alias /home/ubuntu/project/app/static; # your Django project's static files - amend as required
    #}

    # Finally, send all non-media requests to the Django server.
    location / {
        uwsgi_pass  django;
        include     /home/ubuntu/res/uwsgi_params; # the uwsgi_params file you installed
    }
}
```

## 备注

uwsgi_params 文件内容

```properties

uwsgi_param  QUERY_STRING       $query_string;
uwsgi_param  REQUEST_METHOD     $request_method;
uwsgi_param  CONTENT_TYPE       $content_type;
uwsgi_param  CONTENT_LENGTH     $content_length;

uwsgi_param  REQUEST_URI        $request_uri;
uwsgi_param  PATH_INFO          $document_uri;
uwsgi_param  DOCUMENT_ROOT      $document_root;
uwsgi_param  SERVER_PROTOCOL    $server_protocol;
uwsgi_param  REQUEST_SCHEME     $scheme;
uwsgi_param  HTTPS              $https if_not_empty;

uwsgi_param  REMOTE_ADDR        $remote_addr;
uwsgi_param  REMOTE_PORT        $remote_port;
uwsgi_param  SERVER_PORT        $server_port;
uwsgi_param  SERVER_NAME        $server_name;

```
