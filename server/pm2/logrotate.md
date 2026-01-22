# ✅ PM2 Logrotate 配置与使用（纯代码块笔记版）

目的：防止 PM2 日志无限增长，占满磁盘  
方式：使用 pm2-logrotate 插件自动切割 / 压缩 / 删除旧日志  
适用：所有使用 PM2 的 Node 服务（开发/生产）

---

## 1）安装 pm2-logrotate（一次即可）

$ pm2 install pm2-logrotate

安装完成后会多一个 PM2 进程：pm2-logrotate

---

## 2）查看当前 logrotate 配置

$ pm2 conf pm2-logrotate

查看关键参数：

$ pm2 get pm2-logrotate:max_size
$ pm2 get pm2-logrotate:retain
$ pm2 get pm2-logrotate:compress
$ pm2 get pm2-logrotate:rotateInterval
$ pm2 get pm2-logrotate:dateFormat

---

## 3）推荐配置（生产环境通用）

目标策略：
- 每个日志文件超过 10MB 自动切割
- 保留 30 份
- 自动 gzip 压缩
- 每天 0 点检查切割（也可按大小触发）

一键设置：

$ pm2 set pm2-logrotate:max_size 10M
$ pm2 set pm2-logrotate:retain 30
$ pm2 set pm2-logrotate:compress true
$ pm2 set pm2-logrotate:rotateInterval '0 0 * * *'
$ pm2 set pm2-logrotate:dateFormat 'YYYY-MM-DD_HH-mm-ss'

---

## 4）配置项说明（常查）

1）按大小切割（最重要）
$ pm2 set pm2-logrotate:max_size 10M
说明：超过 10MB 就切割

2）保留份数（删除旧日志）
$ pm2 set pm2-logrotate:retain 30
说明：只保留 30 份日志，旧的自动删

3）启用压缩（gzip）
$ pm2 set pm2-logrotate:compress true
说明：切割后的旧日志自动压缩成 .gz

4）按时间切割（cron）
$ pm2 set pm2-logrotate:rotateInterval '0 0 * * *'
说明：每天 0 点切割；大小触发仍会生效（可共存）

5）文件名时间格式
$ pm2 set pm2-logrotate:dateFormat 'YYYY-MM-DD_HH-mm-ss'

---

## 5）手动触发切割（测试用）

$ pm2 trigger pm2-logrotate rotate

---

## 6）查看 logrotate 运行状态

$ pm2 list
$ pm2 logs pm2-logrotate

---

## 7）日志文件目录

```js
$ ls -lh ~/.pm2/logs/
```

---

## 8）维护命令

清空所有 PM2 日志（危险：会删除历史日志）：
$ pm2 flush

卸载插件：
$ pm2 uninstall pm2-logrotate

---

## 9）最常用一套（复制即可）



`````
$ pm2 install pm2-logrotate
$ pm2 set pm2-logrotate:max_size 10M
$ pm2 set pm2-logrotate:retain 30
$ pm2 set pm2-logrotate:compress true
$ pm2 set pm2-logrotate:rotateInterval '0 0 * * *'
$ pm2 set pm2-logrotate:dateFormat 'YYYY-MM-DD_HH-mm-ss'
$ pm2 trigger pm2-logrotate rotate
$ pm2 logs pm2-logrotate
`````

