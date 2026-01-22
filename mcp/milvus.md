## Cursor



配置正确，前提是：

1. Milvus 服务运行在 `localhost:19530`
2. `documents` collection 已存在
3. Ollama 服务运行（默认 `http://127.0.0.1:11434`）用于 embedding

```js
   "milvus": {
      "command": "node",
      "args": ["/Users/webkubor/Documents/milvus-tools/milvus-mcp-server.js"],
      "env": {
        "MILVUS_HOST": "localhost",
        "MILVUS_PORT": "19530",
        "MILVUS_COLLECTION": "documents"
      }
    }
```

