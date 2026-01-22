# PM2 ecosystem.config.js Demo（中文注释版）

> 用法：
> 1）把本文件保存为：ecosystem.config.js
> 2）启动：pm2 start ecosystem.config.js
> 3）重启：pm2 restart ecosystem.config.js
> 4）查看：pm2 list / pm2 logs



```javascript
  module.exports = {
  apps: [
    {
      // 应用名称（pm2 list 里显示的名字）
      name: "my-app",// 启动入口（你的 Node 文件）
  script: "./app.js",

  // 启动参数（可选）
  // args: "arg1 arg2",

  // 工作目录（可选，默认是当前目录）
  cwd: "/var/www/my-app",

  // 监听文件变化并自动重启（开发环境推荐，生产慎用）
  // watch: true,
  watch: false,

  // 忽略监听的目录/文件（watch=true 时才有意义）
  ignore_watch: ["node_modules", "logs", ".git"],

  // 实例数量（集群模式）
  // - 1: 单实例
  // - 4: 启动 4 个进程
  // - "max": 自动按 CPU 核数启动
  instances: "max",

  // 开启集群模式（多核提升吞吐，适合 Web 服务）
  exec_mode: "cluster",

  // 最大内存限制（超过会自动重启，防止内存泄漏）
  // 常用：256M / 512M / 1G
  max_memory_restart: "512M",

  // 自动重启（进程崩溃会自动拉起）
  autorestart: true,

  // 进程异常退出后的延迟重启时间（毫秒）
  restart_delay: 3000,

  // 最大重启次数（避免无限重启）
  max_restarts: 10,

  // 启动超时（超过时间没启动成功会判定失败）
  // 常用于启动较慢的服务
  startup_timeout: 10000,

  // kill 超时（超过时间强制 kill）
  kill_timeout: 5000,

  // 合并日志：集群模式下多个实例的日志合并到一个文件
  // true：合并
  // false：每个实例单独文件
  merge_logs: true,

  // 日志文件路径（建议指定到项目 logs 目录）
  out_file: "./logs/out.log",   // 标准输出日志
  error_file: "./logs/err.log", // 错误日志
  log_file: "./logs/all.log",   // 统一日志（可选）

  // 日志时间格式（配合 log_date_format 会在日志前缀打印时间）
  log_date_format: "YYYY-MM-DD HH:mm:ss",

  // 环境变量（默认环境：development）
  env: {
    NODE_ENV: "development",
    PORT: 3000
  },

  // 生产环境变量（使用 --env production 或 ecosystem 里指定 env_production）
  env_production: {
    NODE_ENV: "production",
    PORT: 3000
  }
}  ],

  // 可选：部署配置（需要 pm2 deploy 才会用到）
  // deploy: {
  //   production: {
  //     user: "root",
  //     host: "1.2.3.4",
  //     ref: "origin/main",
  //     repo: "git@github.com:xxx/my-app.git",
  //     path: "/var/www/my-app",
  //     "post-deploy":
  //       "npm i --production && npm run build && pm2 reload ecosystem.config.js --env production"
  //   }
  // }
};
```

