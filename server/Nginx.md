## Nginx 代理

```js
sudo apt update
sudo apt install nginx -y
```

安装完毕后，启动并设置开机自启：

```js
sudo systemctl start nginx
sudo systemctl enable nginx
sudo systemctl status nginx  # 查看是否运行中
```



```js
sudo nano /etc/nginx/sites-available/pocketbase
```

```js
server {
    listen 80;
    server_name 43.157.251.114;  # 你的公网 IP 或域名

    location / {
        proxy_pass http://127.0.0.1:8090;  # PocketBase 默认端口
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```



```js
sudo ln -s /etc/nginx/sites-available/pocketbase /etc/nginx/sites-enabled/
sudo nginx -t  # 测试配置是否正确
sudo systemctl reload nginx  # 重载 Nginx
```



可访问：http://43.157.251.114:8090/api/collections/users/records





#### pay.larkpay.io

- 如果原来的网站没有配置，就不能直接加协商缓存readm

```js
server {
    listen 80;
    server_name pay.larkpay.io;

    # 开启 ETag，配合协商缓存
    etag on;

    root /usr/share/nginx/html/tk_payment;
    index index.html;

    # 1) 静态资源：强缓存（CDN + 浏览器）
    location ~* \.(?:js|css|png|jpg|jpeg|gif|svg|ico|webp|woff|woff2|ttf|otf|eot)$ {
        expires 365d;
        add_header Cache-Control "public, max-age=31536000, immutable" always;
        access_log off;
        try_files $uri =404;
    }

    # 2) HTML/SPA 入口：协商缓存（CDN 有效，不会强缓存）
    location / {
        try_files $uri $uri/ /index.html;

        add_header Cache-Control "public, max-age=0, must-revalidate" always;
    }

    # 3) 明确对 index.html 单独兜底（可选但建议）
    location = /index.html {
        add_header Cache-Control "public, max-age=0, must-revalidate" always;
    }
}
```

