#### umami-docker

```js
sudo docker ps -a
CONTAINER ID   IMAGE                                            COMMAND                  CREATED        STATUS                    PORTS     NAMES
319d22918aab   ghcr.io/umami-software/umami:postgresql-latest   "docker-entrypoint.s…"   45 hours ago   Exited (1) 2 hours ago              umami-umami-1
317baf9e0ed3   postgres:14                                      "docker-entrypoint.s…"   45 hours ago   Exited (0) 2 hours ago              umami-db-1
6141fa1242ef   hello-world                                      "/hello"                 46 hours ago   Exited (0) 46 hours ag
```

- 服务器重启- docker 会挂掉

```js
sudo find / -name "docker-compose.yml" -o -name "compose.yml" 2>/dev/null
/opt/umami/docker-compose.yml
cd /opt/umami
sudo docker compose up -d
```

```js
cat /opt/umami/docker-compose.yml
version: '3'

services:
  umami:
    image: ghcr.io/umami-software/umami:postgresql-latest
    ports:
      - "3002:3000"
    environment:
      DATABASE_URL: postgresql://umami:umami@db:5432/umami
      APP_SECRET: a3f4b9e28c1d45cbe8ca923ff3c77ad08a62bfa11f9eb21f04ddc18cf5537e9c
    depends_on:
      - db

  db:
    image: postgres:14
    environment:
      POSTGRES_USER: umami
      POSTGRES_PASSWORD: umami
      POSTGRES_DB: umami
    volumes:
      - ./db-data:/var/lib/postgresql/data
```







修改逻辑

```js
服务器重启
  ↓
systemd 启动
  ↓
docker.service（enabled）
  ↓
docker 发现容器 restart: unless-stopped
  ↓
自动拉起 umami + postgres
  ↓
cloudflared 已在 systemd 里跑 → 直接连上
```

-

更新后的配置

`````js
cat /opt/umami/docker-compose.yml
services:
  umami:
    image: ghcr.io/umami-software/umami:postgresql-latest
    ports:
      - "3002:3000"
    restart: unless-stopped
    environment:
      DATABASE_URL: postgresql://umami:umami@db:5432/umami
      APP_SECRET: a3f4b9e28c1d45cbe8ca923ff3c77ad08a62bfa11f9eb21f04ddc18cf5537e9c
    depends_on:
      - db

  db:
    image: postgres:14
    restart: unless-stopped
    environment:
      POSTGRES_USER: umami
      POSTGRES_PASSWORD: umami
      POSTGRES_DB: umami
    volumes:
      - ./db-data:/var/lib/postgresql/data
`````

