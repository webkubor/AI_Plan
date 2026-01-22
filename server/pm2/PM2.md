### 关于快捷访问

**[运行时加载]** Node.js 原生不会识别 `@/`，需要在运行脚本时加载路径解析工具。

最佳方案是 tsx+ node ，使用 tsx做开发运行时



GPT-5-CodeX提供的方案是tsconfig-paths，需要引入tsconfig-paths，并且配置 + 所有的命令前缀加入tsconfig-paths

Claude Sonnect 4.5

| 运行器      | 是否需要 tsconfig-paths | 说明           |
| :---------- | :---------------------- | :------------- |
| **tsx** ✅   | 不需要                  | 内置支持 paths |
| **ts-node** | 需要                    | 需要手动注册   |
| **node**    | 需要                    | 需要编译或转换 |



在 [app.pm2.io](https://app.pm2.io/) 注册一个免费账号

## PM2 不能直接运行 TS 文件

以 **PM2 模式的脚本一定要跑 .js**，这是行业统一做法

- **PM2 不能直接运行 TS 文件**（尤其是 Node 22）。
- Node 22 对 ESM loader 的限制导致 resolveSync() 错误。
- 但是 Node/PM2 → ❌默认 CommonJS 模式
