### 代理接口

```js
https://api.webkubor.xyz/api
```

## 查看运行状态

```js
ps aux | grep cloudflared
journalctl -u cloudflared -n 100 --no-pager

sudo systemctl restart cloudflared
```

