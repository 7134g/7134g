## nginx
```

# 进程数
worker_processes  1;

# 工作数
events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;



    sendfile        on;

    keepalive_timeout  65;



    server {
        listen       8888;
        server_name  localhost;



        location / {
			allow all;
			add_header 'Access-Control-Allow-Origin' '*';
            add_header 'Access-Control-Allow-Credentials' 'true';
            add_header 'Access-Control-Allow-Methods' '*';
            add_header 'Access-Control-Allow-Headers' 'DNT,X-CustomHeader,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type';
            #重新定义或者添加发往后端服务器的请求头
            #给请求头中添加客户请求主机名
            proxy_set_header Host $host;
            #给请求头中添加客户端IP
            proxy_set_header X-Real-IP $remote_addr;
            #将$remote_addr变量值添加在客户端“X-Forwarded-For”请求头的后面，并以逗号分隔。 如果客户端请求未携带“X-Forwarded-For”请求头，$proxy_add_x_forwarded_for变量值将与$remote_addr变量相同  
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            #给请求头中添加客户端的Cookie
            proxy_set_header Cookie $http_cookie;
            #将使用代理服务器的主域名和端口号来替换。如果端口是80，可以不加。
			
			# ws连接设置
			proxy_http_version 1.1;
			proxy_read_timeout   3600s;
			proxy_set_header Upgrade $http_upgrade;
			proxy_set_header Connection "upgrade";
			
			# 静态文件夹位置
			root   D:/project/web_path_bruter/app/service/dist;
			# 起始页
            index  index.html index.htm;
            # 后端地址
			proxy_pass http://127.0.0.1:8000;
        }

        # 错误页面显示
        #error_page  404              /404.html;
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }




    }


}

```
