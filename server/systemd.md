### systemd基于**Linux**的系统**管理服务**

- nginx、docker、redis、ssh都可以用 systemd 管理。

```js
systemd
 ├─ docker.service
 │    └─ 容器（umami / postgres）
 ├─ cloudflared.service
 └─ pm2 (用户态进程)
      └─ node 爬虫 / api
```

#### 查看当前的有哪些服务单元

systemctl 是 systemd 的控制命令

`````
systemctl list-units --type=service
`````

